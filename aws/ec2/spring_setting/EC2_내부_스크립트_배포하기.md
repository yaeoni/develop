# 명령어 배포하기 / 스크립트 배포하기

## 명령어 배포하기

1. git clone / pull 받기
2. root 폴더에서 build 하기
```shell
$ ./gradlew build 

# 나는 테스트에서 렉먹으니 테스트 제거
$ ./gradlew build -x test
```
3. jar 파일 실행(build/libs 에 존재)
* nohup으로 실행시킬 파일은 ~~~반드시 755 퍼미션~~~을 가지고 있어야 합니다.
```shell
$ sudo chmod 755 [파일]
```
* profile 다르게 했다면 뒤에 설정 까지 체크
```shell
$ java -jar [jar 파일] --spring.profiles.active=prod

# nohup 를 이용해 백그라운드에서 돌게 하기(nohup.out log 파일 따로 생성)
$ nohup java -jar [jar 파일] --spring.profiles.active=prod & 
# nohub.out 파일 안생기게 하는 옵션
$ nohup java -jar [jar 파일] --spring.profiles.active=prod & > /dev/null
```

#### 참고 (intelli 에서는 ?!)
1. Run > Edit Configurations... 선택 
2. Spring Boot Application 선택
3. Active profiles 에서 원하는 profile 명을 입력 후 실행

## 배포 스크립트 만들기
1. project 한 칸 바깥 위치에 deploy.sh 파일 생성
```sh
REPOSITORY=/home/ubuntu/resource

cd $REPOSITORY/crewspace-resource/

echo "> GIT pull"

git pull

echo "> 프로젝트 build 시작 without test"

./gradlew build -x test

echo "> build 파일 복사"

cp ./build/libs/*.jar $REPOSITORY/

echo >" 현재 구동중인 app PID 확인"

CURRENT_PID=$(pgrep -f crewspace-resource)

echo >"> PID : $CURRENT_PID"

if [ -z $CURRENT_PID ]; then
        echo "> 현재 구동중인 app이 없습니다."
else
        echo "> kill -15 $CURRENT_PID"
        kill -15 $CURRENT_PID
        sleep 5
fi

echo "> 새로운 app 배포"

# 새로 구동 시킬 jar 파일 검색하여 변수 저장
# -n 1 : 같은 이름으로 생길 경우 가장 마지막에 생성된 파일을 가지고 온다.
JAR_NAME=$(ls $REPOSITORY/ | grep 'crewspace-resource' | tail -n 1)

echo "> JAR NAME : $JAR_NAME"
echo "> 실행 합니다."

nohup java -jar $REPOSITORY/$JAR_NAME --spring.profiles.active=prod &
```
4. 스크립트 파일 실행 권한 부여
```shell
$ sudo chmod 755 ./deploy.sh
```
5. 조금 기다려야한다,,, 마음의 인내심을 가지고,,,,, 20초,,!!!


# 완성 ! _ ! WoW