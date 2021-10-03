# 그런 REST API로 괜찮은가

: 네이버 D2에서 진행했던 세션 시청 및 정리
https://www.youtube.com/watch?v=RP_f5dMoHFc

## REST

- REpresentatioal State Transfer
- 컴퓨터 시스템간의 상호 운용성 전달
- 분산 하이퍼미디어 시스템(웹)을 위한 아키텍쳐 스타일(제약 조건의 집합)
- Roy Thomas Fielding(로이 필딩)이 창시

## API

- SOAP vs REST => REST의 승리!

## REST API

: REST 아키텍쳐 스타일을 따르는 API

- client-server
- stateless
- cache
- uniform interface
- layered system
- code-on-demand(ex. javascript)

### Uniform Interface

- identification of resources
  - 리소스는 URI로 식별되야한다
- manipulation of resources through representations
  - resource는 만들거나, 업데이트하거나, 삭제할 때 response msg에 전달하자.
- self-descriptive messages
  - 메시지는 스스로를 설명해야한다.(host, content-type과 그 속의 명세 표기)
  - content-type : application/json-**_patch+json_**
- hypermedia as the engine of application state(HATEOAS)
  - HATEOAS : 애플리케이션의 상태는 Hyperlink를 이용해 전이되어야한다.
  - html은 a 태그 이용 전이 가능
  - json의 경우 header 속 link 속성 활용 가능

#### Why Uniform Interface?

: 서버와 클라이언트는 각각 **_독립적_**으로 진화한다!

- 서버의 기능이 변경되어도 클라이언트를 업데이트할 필요 없다!

## REST API는 위의 제약 조건들을 다 지켜야한다 ! !

라고 로이필딩씨는 말했다.

# 왜 API는 REST가 잘 안될까?

- 웹 페이지의 media type은 html(hyperlink[a tag], self-descriptive[html 명세])
- API는 **_JSON_** (only 문법해석만 가능, API 문서 정의 따로 필요)

### self-descriptive

: 확장 가능한 커뮤니케이션

- 서버나 클라이언트가 변경되더라도 오고가는 메시지를 언제나 해석 가능하다

### HATEOAS

: 애플리케이션 상태 전이의 late binding

- 어떤 상태로 전이가 완료되고 나서야 그 다음 전이 상태가 결정된다
- 링크는 동적으로 변경될 수 있다

# JSON 활용 API를 REST에 맞게 바꿔보자

## Self-descriptive

### 방법 1 : Media type

1. 미디어 타입 하나 정의
2. 미디어 타입 문서 작성 (json 속 data 의미 작성)
3. IANA 사이트에 타입 등록

### 방법 2 : Profile

1. data 의미 정의한 명세 작성
2. link 헤더에 profile relation으로 명세 링크

- 단점
  - 클라이언트가 link, profile 이해해야함
  - Content negotiation 불가

## HATEOAS

### 방법 1 : data로 하이퍼링크 표현

- 단점 : 어떤식으로 정의할 지 정해야한다

### 방법 2: HTTP 헤더로 (link, location)

# 느낀점

- 내가 지금껏 만들었던 API들은 REST API가 아니였구나! (이젠 HTTP API라고 불러야하나 ㅎ)
- REST API URI 형식에 대한 이야기보다는, REST라는 아키텍쳐에 대해 좀 더 이해할 수 있게 된 것 같다.
- 의도했던 내용은 아니었지만 유익했다!
