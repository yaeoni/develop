## 1. 서버 키 다운로드

1. firebase 프로젝트 설정 -> 서비스 계정 -> 새 비공개 키 생성(json 파일)
2. SDK 추가

- 해당 코드를 함수로 빼서 init할 때 실행시켰다.

```javascript
var admin = require("firebase-admin");

var serviceAccount = require("키 위치");

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
});
```

## 2. 메시지 생성

```javascript
const push: pushDTO = {
  data: {
    date: date,
    title: "추천",
    body: body,
  },
  token: fcmToken,
};
```

- tokens key로 여러개의 fcmToken 배열에 한꺼번에 보낼 수 있다.

## 3. 전송

```javascript
// import 시킨 admin 모듈
admin
    .messaging()
    .send(push)
    .then((response) => {
      console.log("Successfully sent message:", response);
    })
    .catch((error) => {
      console.log("Error sending message:", error);
    });
});

```
