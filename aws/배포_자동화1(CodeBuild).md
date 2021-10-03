# AWS 서비스를 사용해서 배포를 자동화하자1 - CodeBuild

## 0. aws s3에 빌드될 파일이 올라갈 버킷을 만들어야한다.

## 1. 빌드 프로젝트 생성

## 2. 소스 설정 (github 레포지토리 활용)

- develop branch가 업데이트 될 때마다 배포 시킬 것이므로 소스버전에 develop
  <img src="https://user-images.githubusercontent.com/63635886/135744910-6c5c9df6-1b2b-4def-a964-08fd571dd712.png" width="40%" height="40%"/>

## 3. 환경 설정 (인터넷 참고)

<img src="https://user-images.githubusercontent.com/63635886/135744935-c9e61ecf-0246-45cc-9aae-50bb1433379f.png" width="40%" height="40%"/>

## 4. 빌드 스펙 파일 설정

- buildspec.yml 로 설정 (프로젝트의 root 디렉토리에 작성)

## 5. 아티팩트 설정

- Code Deploy 사용할 것이라면 zip파일로 올라갈 수 있도록 한다
  <img src="https://user-images.githubusercontent.com/63635886/135745223-e65c0375-e899-4f04-8f71-dbb33cf24122.png" width="40%" height="40%"/>

## 6. 로그 설정

- 나는 s3에 로그 파일을 올리겠음!

## 7. 프로젝트 루트 폴더에 buildspec.yml 추가

```yml
version: 0.2

phases:
  build:
    commands:
      - echo Build Starting on `date`
      - chmod +x ./gradlew
      - ./gradlew build
  post_build:
    commands:
      - echo $(basename ./build/libs/*.jar)
      - pwd

artifacts:
  files:
    - appspec.yml
    - build/libs/*.jar
  discard-paths: yes

cache:
  paths:
    - "/root/.gradle/caches/**/*"
```
