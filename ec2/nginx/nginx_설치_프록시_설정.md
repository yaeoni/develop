# 1. nginx 설치

```bash
$ sudo apt update
$ sudo apt-get install nginx
```

# 2. 확인

- 80번 포트로 들어가보기(기본 설정)
- Welcome page 나오면 성공

# 3. 프록시 설정

- ec2에서는 /etc/nginx 에 conf 파일 존재 (cf. mac에선 /usr/local/etc/nginx)

### 3-1. sites-available에 custom conf파일 생성

```
server {
listen 80;

server_name localhost;

location / {
    proxy_pass http://localhost:5000;
    }
}
```

### 3-2. Symbolic link 생성

- 절대 경로로 써야 심볼릭 링크를 제대로 알아먹는다.

```
$ sudo ln -s [available 속 custom conf 파일 경로] [enabled]
```

### 3-3. 메인 conf 파일 속 http 설정 속 include path 추가 -> 기본적으로 되어있었다.

```
include /etc/nginx/sites-enabled/*.conf;
```

# 4. 실행

```bash
// 문법 검사
$ sudo nginx -t

// 실행
$ nginx

// 멈추기
$ nginx -s stop
```
