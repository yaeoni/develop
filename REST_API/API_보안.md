# REST API 보안

- API 통신에 있어 접근 제한을 두기 위한 방법 / 이론공부

## 5가지의 보안 관점

#### 1. 인증(Authentication)

> 누가 서비스를 사용하는가? 확인
> ex) 아이디 + 비밀번호

#### 2. 인가(Authorization)

> 어떤 리소스를 사용자가 사용할 "권한"이 있는가 ? 확인

#### 3. 네트워크 레벨 암호화

> 해커들이 네트워크 통신을 낚아챌 수 없도록 하는 것
> 네트워크 프로토콜단에서 처리 (Https)

#### 4. 메시지 무결성 보장

> 전달되는 메시지가 외부 요인에 의해 변조되지 않게 하는 것

#### 5. 메시지 본문 암호화

> 네트워크 레벨의 암호화를 신뢰할 수 없을 때 추가적으로 메시지 자체 암호화
> 애플리케이션 단에서 구현

## "인증" 방법

#### 1. API key 방식

: 특정 API key를 발급받고, API를 호출할 때 API key를 메시지 안에 넣어 호출
: 모든 클라이언트들이 같은 key 공유하게 된다. -> 보안성 낮아짐

#### 2. API token 방식

: ID, PASSWORD를 이용한 사용자 인증 후 일정 기간 동안 유효한 token 발급
: token을 탈취 당하면 API 자체는 호출 가능하나, 사용자의 개인 정보는 가져갈 수는 없다.

#### 3. HTTP Basic Auth

: 사용자 ID, PASSWORD를 http header에 base64 인코딩 형태로 넣어 인증 요청.
: Authorization 이라는 헤더 키 값에 넣어서 서버에 전송한다.

#### 4. Digest access Authentication

: 3의 방식을 좀 더 보완하는 법
: 서버 -> 클라이언트 일정 난수값 받고, 해당 값 이용해 hash화 해서 전송

#### 5. 클라이언트 인증 추가

: 사용자 인증 + 클라이언트 인증
: ex) API token 발급받으려면 client ID, Secret 함께 입력해야함

#### 6. 제 3자 인증 방식

: 소셜 로그인, 대형 API 서비스 제공자들이 대신 인증처리
: API 서비스 제공자들이 파트너 app 에 많이 적용하는 방법

#### 7. IP White List 터널링

: 특정 IP 주소를 White List로 유지
: 서버간의 통신에 유용

#### 8.Bi-directional Certification (Mutual SSL)

: 가장 높은 수준의 인증 방식
: 보통의 https 통신 시 서버에 인증서 두고 단방향 SSL 제공
: 이 방식은 클라이언트에도 인증서를 두고 양방향으로 이용하는 법

#### 9. JWT 방식

: claim 기반의 JSON 토큰 방식 <= 앞으로 내가 활용해볼 것
_ claim : 사용자에 대한 프로퍼티나 속성
_ claim 기반 토큰 : OAuth에 의해 발급되는 access_tokens을 통해 사용자와 연관된 권한을 구별해 허용해주는 구조.
: 토큰을 생성하는 단계에서 서버가 별도로 유지하고 있을 필요 X
: Base64로 인코딩해 HTTP header에 쉽게 삽입 가능
: but claim속 데이터 많아질수록 JWT 토큰 길이 길어짐 = 네트워크 대역폭 낭비 가능성
: 한 번 발급되면 서버에서 더이상 변경이 불가능
=> Expired time을 명시적으로 두고, refresh token이용해 중간마다 재발행 필요

## "인가" 방법

: 추후에 채웁니다!
(https://hirlawldo.tistory.com/100?category=904034 참조)
