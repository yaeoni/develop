# 1. npm 프로젝트 생성

```bash
$ npm init
```

# 2. typescript 모듈 추가 (yarn global 설치 완료 가정)

```bash
$ yarn add typescript ts-node --dev
$ yarn add @types/node --dev
$ yarn add @types/express -dev
```

# 3. ts config 파일 추가

```bash
$ tsc --init
```

- tsconfig.json 파일 생성되면 성공

# 4. tsconfig.json 수정(입맛 따라)

```
{
  "compilerOptions": {
    "module": "commonjs",
    "declaration": true,
    "removeComments": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "target": "es2017",
    "sourceMap": true,
    "outDir": "./dist",
    "baseUrl": "./",
    "incremental": true,
    "resolveJsonModule": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true
  },
  "include": ["./src/**/*"],
  "exclude": ["node_modules"]
}

```

### 추가

- js에 특화되어있는 모듈들은 잘 찾아보면 @types/[모듈] 이 의외로 잘 마련되어있다.
  (ex: multer, multerS3, express 등 등,,)
- type 에러 날 때 잘 사용하자.
