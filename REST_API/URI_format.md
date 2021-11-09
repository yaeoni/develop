# REST API 상 URI format defacto

## URI 형식

```
scheme "://" authority "/" path ["?" query] ["#" fragment]
```

### scheme

- URI를 어떤 규칙에 따라 기술하고, 자원에 어떻게 접근하는가?
- ex) http, ftp, https,,

### authority

- 서버명과 도메인명으로 구성
- 서버명 : www ...
- 도메인명 : example.co.kr
- 보통 userinfo(option) @ host(ip) : port

### path

- 파일 시스템의 경로
- 여러 개의 resource 위치도 나타낼 수 있다.

### query

- 폴더명과 파일명 구성
- 서버 내부 자원의 위치 나타냄

### fragment

- "#"로 식별
- HTML 에서 id로 식별된 어떤 특정 section을 스크롤할 때 사용

---

## URI Format 설계

### ✅ 앞에 붙는 / 는 계층 관계를 나타내기 위해 사용되어야한다.

### ✅ 맨 뒤의 / 는 URI에 포함되면 안된다.

- URI는 모든 문자를 식별자로 쓰기 때문

### ✅ 하이픈(-)은 URI의 가독성을 향상시키기 위해 사용되어야한다.

### ✅ 언더바(\_)는 URI에서 사용하면 안된다.

- 응용프로그램의 폰트에 따라 깨지거나 숨겨질 수 있다.
- 대신 하이픈(-)을 사용하자

### ✅ URI path에서 소문자 사용을 권장한다.

### ✅ URI에서 파일 확장자는 포함되면 안된다.

- body의 내용을 처리하기 위해 Content-Type header 를 통해 media type을 알린다

---

## URI Authority 설계

### ✅ API에는 일관된 서브 도메인 이름이 사용되어야한다.

- API의 최상위 레벨의 도메인과 첫번째 서브도메인 이름은 어떤 서비스인지 알 수 있어야한다.
- ex) http://api.soccer.restapi.org

### ✅ 개발자 문서에도 일관된 서브 도메인 사용되어야한다.

- API 문서, access key 등
- ex) http://developer.soccer/restapi.org

---

## URI Resource Modeling

### ✅리소스는 URI Format 과 마찬가지로 계층적으로 구성해야한다.

---

## URI Resource Archetypes

: 책의 저자가 정의한 REST API의 4가지 리소스 타입

### Document

- 객체 인스턴스 or 데이터베이스의 레코드 => 단일 개념
- 1 개의 개체를 나타내는 것
- 어떤 값을 나타내는 Field, 다른 Document로 연결되는 link로 구성

### Collection

- 서버 관리 리소스의 디렉토리 개념
- resources을 모아두는 곳(= document의 묶음)
- 단일 리소스가 아닌 다량의 리소스가 필요할 때 호출

### Store

- client 입장의 resource 저장소
- ex) 장바구니

### Controller

- 실행 가능한 함수
- REST는 보통 하나의 Controller가 CRUD중 하나에 매핑되어 작업 수행

### Collection -contains-> Store -stores-> document

---

## URI Path 설계

### ✅ Document 이름으로 단수형 명사 사용

- ~/leagues/seattle/teams/trebuchet/players/**_claudio_**

### ✅ Collection 이름으로 복수형 명사 사용

- ~/device-management/**_managed-devices_**

### ✅ Store 이름으로 복수형 명사 사용

- ~/cart-management/users/{id}/**_carts_**
- ~/user-management/**_users_**

### ✅ Controller 이름으로 동사 or 동사구문 사용

- ~/cart-management/users/{id}/cart/**_checkout_**
- ~/song-management/users/{id}/playlist/**_play_**

### ✅ 변수형의 path 는 ID 기반 값으로 대체

- ~/leagues/seattle/teams/trebuchet/players/**_21_**

### ✅ CRUD 함수 명은 URI에서 사용하면 안된다!!!! _ 헉 _

- DELETE /users/1234 (O)
- GET /deleteUser?id=1234 (X)

---

## URI Query 설계

### ✅ Query는 Collection이나 Store를 필터링하기 위해 사용되어야한다.

- GET /users (모든 user 정보 받기)
- GET /users?**_role=admin_** (필터링)

### ✅ Query는 Collection이나 Store의 결과를 페이징 하기 위해 사용되어야한다.

- 페이징 처리할 때 query 영역 사용
- GET /users?**_pageSize=25&pageStartIndex=50_**

#### reference

- https://changrea.io/Web/rest-api-design-1/
- https://sabarada.tistory.com/28
- REST API Design RuleBook

#### ~~나는 지금껏 하지말라는거 다했따ㅎ~~
