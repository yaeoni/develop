# AWS 서비스를 사용해서 배포를 자동화하자2 - CodeDeploy

: Github에 올라간 파일들을 EC2에 전달 완료 !!!

## 1. IAM -> 역할 -> 역할 만들기

: code deploy 와 s3 접근권한(아티팩트 가져오기 위함) 필요

#### (1) codedeploy

- 서비스 & 사용 사례 : Code Deploy 설정(정책 파트 스킵된다.)
- 역할 : codedeploy

#### (2) ec2 - s3 연결

- 사용 사례 : ec2
- 정책 : AmazonEC2RoleforAWSCodeDeploy 선택
- 역할 : ec2-s3-deploy

## 2. EC2 에 ec2-s3 IAM 역할 부여

- 작업 -> 보안 -> IAM 역할 수정에서 ec2-s3-deploy 역할 선택

## 3. EC2 서버에서 jdk(java) 설치

- 내가 사용하는 스프링부트 자바 버전은 11이므로 11로 설치한다

```bash
$ sudo apt update
$ sudo apt install openjdk-11-jdk
$ java --version
```

## 4. EC2에 code deploy agent 설치

```bash
$ sudo apt install awscli
$ wget https://aws-codedeploy-ap-northeast-2.s3.amazonaws.com/latest/install
$ chmod +x ./install
$ sudo apt install ruby
$ sudo apt install -y -q gdebi-core
$ sudo ./install deb
$ sudo service codedeploy-agent status
```

## 5. AWS Code Deploy 설정

1. aws code deploy 접속 -> 애플리케이션 -> 애플리케이션 생성
   ![image](https://user-images.githubusercontent.com/63635886/135753091-9464222c-4650-4998-a0a3-7eb3a0216d0e.png)
2. 해당 application 접속 -> 배포 그룹 생성
3. 배포 그룹명 입력 & 서비스 역할은 배포\_자동화1 에서 만든 codedeploy 역할 부여 & 배포 유형은 현재 위치
4. 환경 구성 -> Amazon EC2 인스턴스 체크(인스턴스에 태그 추가해서 해당 태그로 탐색하기)
5. 배포 그룹 생성

## 6. 소스코드에 appspec와 start.sh 추가

: AWS code deploy는 appspec.yml을 통해서 어떤 파일을 어떤 위치에 배포하고, 어떤 스크립트를 실행시킬 것인지 관리한다.
: start.sh는 배포 후 실행될 스크립트(동일한 jar 파일명으로 실행중인 프로세스를 죽이고, ec2에 새로 생성된 jar파일을 실행시킨다)

#### (1) appspec.yml

```yml
version: 0.0
os: linux
# destination에 아티팩트가 unzip된 결과가 생성될 디렉토리명을 넣어준다.
files:
  - source: /
    destination: /home/ubuntu/build/
permissions:
  - object: /
    pattern: "**"
    owner: ubuntu
    group: ubuntu

hooks:
  # 주의 할 점은 빈칸 yml파일 특성상 빈칸 개수를 중시해야하고 Tab을 쓰면 안된다는점
  # AfterInstall은 배포를 완료한 후 실행되는 스크립트를 명시하며
  # ApplicationStart나 ValidateService 등을 대신 쓸 수 있다.
  AfterInstall:
    - location: start.sh
      timeout: 60
      runas: ubuntu
```

#### (2) scripts/start.sh

```sh
#!/bin/bash
BUILD_JAR=$(ls /home/ubuntu/build/*.jar)
JAR_NAME=$(basename $BUILD_JAR)
echo "> build 파일명: $JAR_NAME" >> /home/ubuntu/deploy.log

echo "> build 파일 복사" >> /home/ubuntu/deploy.log
DEPLOY_PATH=/home/ubuntu/
cp $BUILD_JAR $DEPLOY_PATH

echo "> 현재 실행중인 애플리케이션 pid 확인" >> /home/ubuntu/deploy.log
CURRENT_PID=$(pgrep -f $JAR_NAME)

if [ -z $CURRENT_PID ]
then
  echo "> 현재 구동중인 애플리케이션이 없으므로 종료하지 않습니다." >> /home/ubuntu/deploy.log
else
  echo "> kill -15 $CURRENT_PID"
  kill -15 $CURRENT_PID
  sleep 5
fi

DEPLOY_JAR=$DEPLOY_PATH$JAR_NAME
echo "> DEPLOY_JAR 배포"    >> /home/ubuntu/deploy.log
nohup java -jar $DEPLOY_JAR >> /home/ubuntu/deploy.log 2>/home/ubuntu/deploy_err.log &
```

## 7. buildspec.yml 파일 artifacts 부분 수정 (appspec, start.sh 포함토록)

```yml
artifacts:
  files:
    - build/libs/*.jar
    - appspec.yml # CodeDeploy 적용 시 추가
    - scripts/** # CodeDeploy 적용 시 추가
  discard-paths: yes
```

## 8. code build 빌드 수행

- 수행 성공되면 s3에 zip파일이 생성된다.

## 9. code deploy 배포 수행

1. 애플리케이션 -> 생성한 애플리케이션 -> 생성한 배포그룹 -> 배포 생성
2. 배포 그룹 : 만들었던 그룹
3. 개정 유형 : S3, 개정 위치 : codebuild로 수행된 파일 위치(s3 URI 복사해오면 된다) & 확장자 .zip
4. 롤백 구성 비활성화 체크 (테스트용이므로)

## 10. 배포 완료되면 배포 상태 [성공] 으로 뜬다 !!! 우와앙

## 추가) 에러 로그 보는법

```bash
$ vim /var/log/aws/codedeploy-agent/codedeploy-agent.log
```
