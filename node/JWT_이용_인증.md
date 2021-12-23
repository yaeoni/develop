# 절차

0. 로그인 절차를 거친 뒤 / 또는 API key를 DB 저장해 요청에 함께 보내고 인증
   => 이후 jwt token 발급
1. 사용자(클라이언트) -> request header에 token 담아 보냄
2. 서버 -> 해당 token 검증
3. 인증 성공 시 토큰의 내용이 반환되어 req.decoded에 저장
4. 유효기간을 설정하고 만료 되었을 때에는 재발급 받도록 함.

### 방식

- 토큰 인증(verify)
- 토큰 발급 및 재발급
- 둘 다 순서대로 middleware로서 동작하게끔 만들면 될 것 같다.

# 서버에 적용

## 1. jwt 모듈 설치

```bash
$ npm i jsonwebtoken
```

## 2. JWT_SECRET 키 설정(env 파일)

- 아무 문자열이나 사용할 수 있다.
- jwt.sign 과 jwt.verify에 사용된다.

## 테스트

- postman으로 token 검증 시, 발급받은 token을 header 속 key : Authorization 의 value값으로 넣어 보내야한다.
