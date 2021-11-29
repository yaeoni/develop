# password 이용한 접속

## 1. 유저 생성 및 비밀번호 생성

```bash
$ adduser [유저 이름]
$ sudo passwd [유저 이름]
$ 비밀번호 생성
```

## 2. sudoers에 유저 추가

```bash
$ sudo chmod u+w /etc/sudoers
$ sudo vim /etc/sudoers
```

- 하단에 [유저 이름] ALL=(ALL:ALL) ALL 추가

## 3. sshd_config 파일 수정

```bash
$ sudo vim /etc/ssh/sshd_config
```

- PasswordAuthentication : yes로 수정

## 4. 서비스 재시작

```bash
$ sudo service sshd restart
```

## 5. 접속

```bash
$ ssh [유저이름]@[주소]
```
