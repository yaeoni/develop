# EC2에 jdk를 설치하고 (스프링부트)프로젝트를 실행해봅시다.

## 0. jdk 설치

```shell
$ sudo apt update
$ sudo apt install openjdk-11-jdk
$ java --version // 버전 확인
```

## 1. gradle에 권한부여 및 빌드

```shell
$ chmod 777 ./gradlew
$ ./gradlew build
```

## 2. jar 파일 실행

```shell
// build/libs 속
$ java -jar [만들어진 jar 파일]
```

## 추가

- 인스턴스의 8080포트(tomcat)와 80포트(http)를 열어줘야한다.
- 고정 IP 위해 elastic IP를 할당하고, https 적용도 해야한다.
- 터미널 종료 시 프로그램 종료되는 상황 막기 위해 nohub 등의 명령어를 사용한다.
