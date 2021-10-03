# AWS 서비스를 사용해서 배포를 자동화하자2 - CodePipeline

: Github에 push를 할 때마다 자동으로 build -> deploy 과정을 거치게하자

## 1. Codepipeline 파이프라인 생성

1. aws codepipeline에서 파이프라인 생성
2. 이름 설정 & 새 서비스 역할 체크(자동으로 역할 생성해준다.)
3. 고급 설정 : 사용자 지정 위치 -> build 파일이 있는 버킷 선택 & 암호화 키 -> 기본 aws관리형 키

## 2. 소스 스테이지 추가

: 소스 저장소 및 브랜치 선택, 어떤 방식으로 감시할 것인지 선택

1. 공급자 -> GitHub(버전1, 토큰활용)
   - 버전2는 github app을 활용해야한다.
2. 리포지토리와 브랜치명 입력

## 3. 빌드 스테이지 추가

: 빌드 공급자 정보 입력(code build)

- 만들어놓은 빌드(CodeBuild) 선택

## 4. 배포 스테이지 추가

: 인스턴스에 배포하는 정보 입력

- 만들어놓은 배포(CodeDeploy) 선택

## 5. 파이프라인 생성

# Webhook 설정할 수 없다는 에러 [Webhook could not be registered with GitHub.]

: 이 에러를 해결하느라 눈깔이 빠지는 줄 알았다

- OAuth App(여기선 CodePipeline)이 organization 속 repository에 대한 접근 권한이 없기 때문에 생겨났다

### 해결 방안

1. github 자신의 프로필 속 settings -> Applications -> Authorized OAuth Apps
2. 사용하려는 OAuth app name 클릭(CodePipeline)
3. Organization access 에서 pipeline 사용하려는 organization grant 설정
4. 해 ~ 결 ,,,,, ㅎr

# 완료 ,, 하루를 걸친 배포 자동화,,, 쓸 일 있을거야,, 라며 위로해보자

<img src="https://user-images.githubusercontent.com/63635886/135759376-7f504a00-9c3d-4868-98f7-68deb6795f2a.png" width="40%" height="40%"/>
