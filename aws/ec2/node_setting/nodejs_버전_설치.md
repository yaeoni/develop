# 일반 apt install로는 낮은 버전으로 설치되는 node를 깔아보자

## 원하는 버전 curl로 얻어오기

```bash
$ sudo apt install curl
$ curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```

### [추가] <https could not be found. 에러

- The method driver /usr/lib/apt/methods/<https could not be found.
- 이런 에러가 나면 아래의 패키지 설치

```bash
$ sudo apt install apt-transport-https ca-certificates
```

- /etc/apt/source.list.d 속에 파일이 있을 경우 **_다 삭제 후 설치 진행 후 복구_** (해당 방법으로 해결.)
