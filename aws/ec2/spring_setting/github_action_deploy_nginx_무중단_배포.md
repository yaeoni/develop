# Github actions / AWS Code Deploy / nginx 이용한 무중단 배포 

## github actions

### 1. workflow 생성
- 깃 레포의 Actions 카테고리에서 **set up a workflow yourself** 로 새 워크스페이스 생성
- Marketplace : 미리 만들어놓은 스크립트 / 필요시 추가 가능
- Documentation : workflow 설명
- yml의 형식으로, 레포/.github/workflows 에 저장된다.
```yml
name: crewspace CI

# 아래를 사용하게되면 수동으로 빌드/배포
# on:
#   workflow_dispatch:

on:
  push:
    branches: [ develop ]
  pull_request:
    branches: [ develop ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 11
      uses: actions/setup-java@v2
      with:
        java-version: '11'
        distribution: 'adopt'
        cache: gradle
        
    - name: Grant execute permission for gradlew
      run: chmod +x gradlew
    - name: Build with Gradle
      run: ./gradlew build -x test

```

## 2. commit & push (= 생성)
- 레포 Actions 칸에 상태를 볼 수 있다.
![github](https://user-images.githubusercontent.com/63635886/143608849-2e7536ff-0446-4f8c-a564-d157420338c8.png)

## 3. AWS IAM user 생성
- 아이디 / 키 방식
- 정책 : Amazons3FullAccess, AWSCodeDeployFullAccess

## 4. github actions에 s3 로 build된 jar 파일 올리는 스크립트 추가
```yml
name: crewspace CI

env: # 추가
    S3_BUCKET_NAME: crewspace-server
    PROJECT_NAME: crewspace

on:
    push:
        branches: [ develop ]
        pull_request:
        branches: [ develop ]

jobs:
    build:
    runs-on: ubuntu-latest

    steps:
    
#중략

    # 아래부터 추가
    - name: Make directory
      run : mkdir -p deploy

    - name : Copy Jar
      run : cp ./build/libs/*.jar ./deploy
      
    - name: Make zip file
      run: zip -r ./crewspace-jar.zip ./deploy
      shell: bash
      
    - name : Check zip file
      run : ls
      
    - name: Configure AWS credential
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}
        
    - name: Upload S3
      run: aws s3 cp --region ap-northeast-2 --acl private ./crewspace-jar.zip s3://crewspace-server/crewspace
```
## 5. github 레포 access key, secret key 저장
1. 레포 settings -> secrets -> New Secret 
2. AWS_ACCESS_KEY / AWS_SECRET_KEY / AWS_REGION 3가지 값 등록


# 추가) gitignore에 정의된 파일 함께 넣는법
- gitaction 으로 빌드 -> s3 upload -> ec2 전송 시 gitignore에 정의된 환경 파일들은 전송이 같이 안된다.
- 환경파일 압축 -> 해당 압축파일 gitignore 추가 -> 압축파일 암호화 -> 암호화된 파일 git 업로드 -> git secret에 비밀번호 추가 -> workflow 에 복호화/파일 복구

## 1. 환경파일 압축
- 나는 ./src/main/resources 에 환경 파일이 존재했다.
```bash
$ cd src/main/resource
$ tar cvf env.tar ./env.yml ./aws.yml
```
## 2. 압축된 환경 파일 .gitignore에 추가

## 3. env.tar 암호화
```bash
$ gpg -c env.tar
```
## 4. github secrets에 압축 비밀번호 등록
## 5. 암호화된 압축 파일은 깃에 업로드
## 6. workflow에 복호화 및 파일 복구 프로세스 추가
```yml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 11
      uses: actions/setup-java@v2
      with:
        java-version: '11'
        distribution: 'adopt'
        cache: gradle

    # 추가
    - name: Decrypt ENV
      run: gpg --quiet --batch --yes --always-trust --decrypt --passphrase=${{ secrets.ENV_SECRET }} --output ./src/main/resources/env.tar ./src/main/resources/env.tar.gpg

    - name: Unzip env
      run: tar xvf ./src/main/resources/env.tar

    # 중략 # 아래부턴 빌드
```
```
### gpg,,? 암호키
1. gpg 다운로드
```bash
$ brew install gpg
```
2. 요로면 인제 암호화 명령어 수행 시 비밀번호를 칠 수 있다.

# code deploy 로그!!! 제발 확인 두군데,,
- /var/log/aws/codedeploy-agent/codedeploy-agent.log
- /opt/codedeploy-agent/deployment-root/deployment-logs/codedeploy-agent-deployments.log

# 돌아가는 포트 확인 
- ps -ef | grep java
- sudo lsof -i :8081