# OAuth란?

: 다른 서비스의 회원 정보를 안전하게 사용하기 위한 방법
ex) 각종 소셜 로그인

## 과정(구글 로그인)

0. 서비스 등록 (redirect URL, 기본 정보 등 기입)
1. client -> server, 권한 인증 요청(auth.google.com/~)
2. client <- server, 동의 받기(소셜 로그인 화면) => 성공 시 authentication code 발급
3. client -> server, (2)에서 받은 code를 이용해 access token 요청
4. client <- server, access & refresh token발급
5. client -> server, access token 이용해 정보 접근 / resource server는 해당 DB에서 token값 비교

- (0)에서 추가적 보안 위해 client key, client secret 발급
- (4)의 과정에서 redirect URL을 이용(토큰을 발급해주는 페이지)
- (5)에서 만약 access token의 유효기간이 지나면 refresh token 이용해 access token 재발급
  - 해당 재발급 과정은 resource owner에게 허락받지 않아도 된다.

## Service provider 종류

#### Authorization server

: 인증 및 권한 부여 담당

#### Resource server

: 사용자의 데이터 관리

# OAuth 1.0과, OAuth 2.0의 차이?

|   비교    |         OAuth 1.0          |                   OAuth 2.0                    |
| :-------: | :------------------------: | :--------------------------------------------: |
| 서버 분리 |            없다            |     authorization 과 resource 서버의 분리      |
|   토큰    |   request & access token   |             access & refresh token             |
| 유효기간  | access token 유효기간 없음 | access 토큰 유효기간 존재, 만료시 refresh 이용 |

# JWT와 OAuth의 차이?

|   이름    |                |                         특징                         |
| :-------: | :------------: | :--------------------------------------------------: |
| **OAuth** |   Framework    |  명확한 정보가 들어있지 않다. / 일종의 랜덤 문자열   |
|  **JWT**  | 토큰의 한 종류 | 명확한 정보를 가짐. / 해당 정보를 가지고 유효성 검사 |
