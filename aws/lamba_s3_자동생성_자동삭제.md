# Node 환경에서 s3 객체 자동 생성.삭제 하기

### 전제

- original 이미지를 만들면 thumbnail 이미지 자동 **_생성_**
- thumbnail 이미지 삭제 시 original 이미지 자동 **_삭제_**
- typescript 사용

## 필요 모듈(임포트 해오기)

- aws-sdk
- aws-lambda

```javascript
const AWS = require("aws-sdk");
import { Handler, Context } from "aws-lambda";
```

## 생성

```javascript
const s3 = new AWS.S3();

export const handler: Handler = async (event: any, context: Context) => {
  const tokens = Key.split("/");
  const size = tokens.length - 1;
  const filename = tokens[size];
  const path = tokens.slice(0, size - 1).join("/");

  try {
    const s3Object = await s3.getObject({ Bucket: Bucket, Key: Key }).promise();

    // 이미지 전처리

    await s3
      .putObject({
        Bucket,
        Key: `${path}/thumb/${filename}`,
        Body: resizedImage,
      })
      .promise();
  } catch (err) {
    console.log(err);
    process.exit(0);
  }
};
```

## 삭제

```javascript
const s3 = new AWS.S3();

export const handler: Handler = async (event: any, context: Context) => {
  const Bucket = event.Records[0].s3.bucket.name as string;
  const Key = event.Records[0].s3.object.key as string;

  const tokens = Key.split("/");
  const size = tokens.length - 1;
  const filename = tokens[size];
  const path = tokens.slice(0, size - 1).join("/");

  await s3
    .deleteObject({ Bucket, Key: `${path}/original/${filename}` })
    .promise();
};

```

## Lambda 와 연결하기(node_image_resizing/lambda 참조)

0. lambda가 실행할 함수 작성
1. index.js , node_modules 폴더, pacakge.json, package-lock.json 파일 압축
2. AWS lambda 함수생성
3. 핸들러 이름은 반드시 파일이름.핸들러이름(ex. index.handler)로 설정!
4. 구성 -> 일반구성 -> 편집 속 실행역할 - AWS 정책 템플릿에서 새 역할 생성
5. 이름 아무거나 설정 & 정책 템플릿에서 Amazon S3 객체 읽기 전용 권한 추가
6. 트리거 추가 -> s3 / 버킷 설정 / 이벤트 : 모든 객체 생성 이벤트 / 접두사(어떤 폴더에 업로드된 사진을 리사이징 시킬건지) 설정

- 이때 typescript 로 작성한 경우 tsc 통해 js로 컴파일한다.
