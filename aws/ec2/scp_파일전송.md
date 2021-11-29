# scp란?

- ssh 프로토콜 기반으로 한 SecureCopy
- 네트워크 연결된 상태에서 ssh와 동일한 포트(22) 사용해 파일 송수신

## Local -> Remote

```bash
$ scp [옵션] [파일명] [원격지_user]@[원격지IP]:[받는 위치]
```

- 파일들이 포함된 디렉터리 같은것을 보낼 때는 -r 옵션 추가

### 경로 permission denied 시

- 원격지 받는 위치를 정확하게 적어주기(절대 경로로)

## Remote -> Local

```bash
$ scp [옵션] [원격지_user]@[원격지IP]:[원본 위치] [받는 위치]
```
