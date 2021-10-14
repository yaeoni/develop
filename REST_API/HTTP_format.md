# REST API 상 HTTP method와 status code defacto

## Request Method

### ✅ GET을 이용해 자원을 얻는다

### ✅ GET과 POST를 사용해 다른 요청 방법을 터널링해선 안된다.

- GET과 POST는 Resource를 얻거나, 생성하는 용도로만 사용한다.

### ✅ HEAD를 이용해 Response의 Header 정보를 얻는다.

### ✅ PUT을 이용해 자원을 저장하거나 업데이트한다.

### ✅ PUT을 이용해 변하는 자원을 업데이트한다.

- PUT request에는 저장하거나, 업데이트 하고자 하는 자원을 포함해야한다.

### ✅ POST를 이용해 Collection에 "새로운" 자원을 생성한다.

### ✅ POST를 이용해 Controller를 실행한다.

- 어떤 Controller Resource를 대상으로 POST를 사용한다.
- POST는 새로운 자원을 생성하는 것 외에 자원을 얻거나, 저장하거나, 삭제하는데 사용하지 않는다.

### ✅ DELETE를 이용해 자원을 제거한다.

- Collection이나 Store에 있는 자원을 제거하는 데 사용한다.

### ✅ OPTIONS 를 이용해 자원의 이용 가능한 Method의 메타데이터 정보를 얻는다.

- Response header속 Allow 항목에 이용 가능한 Method 목록 확인 가능하다.

---

## PUT vs POST ?

- POST : 리소스 생성 담당
  - 요청 시마다 새로운 리소스 생성
  - 클라이언트가 리소스의 위치(식별자)를 모른다 -> 리소스 결정권이 서버에게 있다.
  - 멱등하지 않다.
- PUT : 리소스 생성과 수정 담당
  - 클라이언트가 자원의 식별자를 알고 있다. -> 리소스 결정권이 클라이언트에게 있다.
  - 리소스의 모든 속성 수정
  - 멱등하다
- [추가] PATCH : 리소스 일부분 수정

---

## HTTP Status Code

| 코드 |   카테고리    |                  설명                  |
| :--: | :-----------: | :------------------------------------: |
| 1xx  | Informational |       Protocol 레벨의 정보 제공        |
| 2xx  |    Success    |    Client의 요청을 성공적으로 받음     |
| 3xx  |  Redirection  | 요청 완료 위해 Client의 추가 작업 필요 |
| 4xx  | Client Error  |            Cliect 측의 에러            |
| 5xx  | Server Error  |            Server 측의 에러            |

### ✅ 200(OK)은 성공을 나타내는 데 사용한다.

### ✅ 200(OK)은 Error를 다루는 용도로 사용해선 안된다.

- 200(OK) 은 반드시 Response Body를 포함한다.
- 204(No Content) 는 Response Body가 없다.

### ✅ 201(Created)는 자원을 성공적으로 생성했음을 나타낸다.

- Collection이 생성되거나, Store이 추가되면 201을 보낸다.

### ✅ 202(Accepted)는 비동기 액션의 성공적인 시작을 나타낸다.

- Client에게 요청은 유효하지만, 최종적으로 처리되기에 문제가 있을 때 보낸다.
- ex) 서버가 다른 요청을 다루고 있을 때 이 코드를 보낸다.

### ✅ 204(No Content)는 응답 메시지의 body가 의도적으로 비어 있다고 알릴 때 사용

### ✅ 301(Moved Permanently)는 자원의 위치가 이전 되었음을 나타낸다.

- URI가 변경 되었을 때 보낸다.
- Response header 속 Location 부분에 변경된 URI 명시해야 한다.

### ✅ 302(Found)는 사용하면 안된다.

### ✅ 303(See Other)은 Client에게 다른 URI를 알려줘야 한다.

- 다른 URI를 Location에 지정한다.

### ✅ 304(Not Modified)는 bandwidth 유지를 위해 사용한다.

- response body가 없다는 점에서 204와 유사
- 304는 이미 Client가 최신 버전의 response를 가지고 있기에 body를 비우고 보내는 것

### ✅ 307(Temporary Redirect)은 Client에게 다른 URI로 요청을 다시 보내라고 할 때 사용한다.

- REST API가 더이상 Client의 요청을 처리하지 않겠다는 의미
- 대신에 다른 URI로 요청하도록 임시 URI를 보낸다.

### ✅ 400(Bad Request)는 요청의 실패를 알릴 때 사용한다.

- 일반적인 Client 측 Error

### ✅ 401(Unauthorized)은 Client의 신용에 문제가 있을 때 사용한다.

- Authorization 없이 자원에 접근하려 할 때 보호하기 위해 사용한다.

### ✅ 403(Forbidden)은 인가된 상태임에도 불구하고 접근을 금지할 때 사용한다.

- Client가 허용된 범위 밖에서 자원에 접근하려할 때 403을 보낸다.
- 서버가 허용하지 않는 웹 페이지나, 미디어에 접근하려할 때 사용

### ✅ 404(Not Found)는 Client의 URI가 자원과의 맵핑이 실패했을 때 사용한다.

### ✅ 405(Method Not Allowed)는 HTTP Method를 지원하지 않을 때 사용한다.

### ✅ 406(Not Acceptable)은 Client가 요청한 media type을 처리할 수 없을 때 사용한다.

### ✅ 409(Conflict)는 자원의 상태를 위반했을 때 사용한다.

- Store의 resource가 비어있지 않은데, 삭제를 요청할 때 사용

### ✅ 412(Precondition Failed)는 조건부 operation이 필요할 경우 사용된다.

- Client가 보낸 request header속 서버의 선결 조건과 맞지 않을 때 사용

### ✅ 415(Unsupported Media Type)은 요청 Payload의 media type을 처리할 수 없을 때 사용

- Client의 Content-type을 API가 처리할 수 없는 type일 때 사용

### ✅ 500(Internal Server Error)은 API가 제대로 작동하지 않을 때 사용

#### reference

- https://changrea.io/Web/rest-api-design-1/
- https://sabarada.tistory.com/29?category=800100
- REST API Design RuleBook
- https://velog.io/@53_eddy_jo/RESTful%ED%95%9C-%EC%84%B8%EA%B3%84%EC%97%90%EC%84%9C%EC%9D%98-POST%EC%99%80-PUT%EC%9D%98-%EC%B0%A8%EC%9D%B4-%EA%B1%B0%EA%B8%B0%EC%97%90-FETCH%EA%B9%8C%EC%A7%80
