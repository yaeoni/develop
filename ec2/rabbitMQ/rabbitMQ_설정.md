# EC2 ubuntu 환경에서 rabbitMQ를 설치하자.

## 1. Sigining key 추가

```bash
$ apt-key adv --keyserver "hkps.pool.sks-keyservers.net" --recv-keys "0x6B73A36E6026DFCA"
```

## 2. source list file 추가

- rabbit MQ 는 Erlang으로 개발되어 Erlang package의 의존성을 가진다.
- erlang package와 rabbitMQ repository 경로를 추가.
- /etc/apt/sources.list.d 의 경로 추가 (이후 https를 통한 설치에 문제가 생길 수 있음)
- https cannot found 어쩌구 에러가 뜨면 해당 폴더 파일들을 다 지우고 설치한 뒤 재설정해주자.

```bash
$ sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list <<EOF
deb <https://dl.bintray.com/rabbitmq-erlang/debian> xenial erlang
deb <https://dl.bintray.com/rabbitmq/debian> xenial main
EOF
```

## 3. Erlang package, rabbitMQ server 설치

```bash
$ sudo apt update
$ sudo apt install rabbitmq-server
```

## 4. rabbit MQ server 시작

```bash
$ sudo service rabbitmq-server start
```

## 5. client의 유저 설정(기본 guest계정으로는 localhost만 접근을 허용)

- 유저 추가

```bash
$ sudo rabbitmqctl add_user [username] [password]
$ sudo rabbitmqctl list_users // 유저 리스트 확인
$ sudo rabbitmqctl set_user_tags [username] administrator
$ sudo rabbitmqctl set_permissions -p / [username] ".*" ".*" ".*"
```

- 또는 웹페이지상에서 해결 가능

## 6. client send 부분 수정

```javascript
amqp.connect("amqp://user:pass@52.78.114.158", (err, conn) => {
    // 연결
}

// user:pass 부분을 위에서 설정한 유저와 패스워드 네임을 넣으면 된다!!!
```

## 6. server 포트포워딩

- 서버 통신을 위해 포트를 열어야 한다.
- 4369 : epmd, 여러 rabbitmq 서버끼리 서로를 찾을 수 있는 네임 서버 역할을 하는 데몬에서 사용
- 5672, 5671 : AMQP 를 사용한 메시지 전달
- 25672 : inter-node 와 CLI Tool 연결
- 15672 : HTTP API, Management UI

## 에러 핸들링 (ec2 linux 기준, mac은 /usr/local 접두사가 붙는다.)

- 로그 파일 위치
  - /var/log/rabbitmq/
- 서비스 재시작
  - [ec2] sudo service rabbitmq-server start
  - [mac] brew services stop rabbitmq / brew services start rabbitmq
- 상태 확인
  - systemctl status rabbitmq-server.service

# CLIENT 에서 유저 설정을 하는지 모르고 참~~~~~ 오랜시간 삽질했다..... 자기전에 알아서 다행이야,, 하 하 하
