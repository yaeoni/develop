# https 적용기를 기록하자!!!

## 1. 내도메인 -> 인증서 발급
- 로그인
- New Certificate
## 2. Enter Domain 란에 등록할 도메인 입력
- 90-Day Certificate 선택(돈나간다,,)
## 3. verification 방식 DNS 선택
![github](https://user-images.githubusercontent.com/63635886/144078780-7861f3ce-366c-43ee-a229-526cdbf8a92d.png)
- 나온 Name과 Point To 내도메인 별칭 영역에 작성
- Name중 나의 도메인 앞에 적인 랜덤문자 -> CNAME 속 도메인 옆 빈칸에
- Point To 는 뒤에 칸에 그대로 복붙

## 4. verify 이후 Certificate key 설치 (default format 이면 okay)
![github](https://user-images.githubusercontent.com/63635886/144079323-27ec71b2-16a5-48d4-95e5-eaaa40d60001.png)

## 5. 받은 zip 파일 서버로 옮기기
```bash
$ scp -i [key] [zip file] ubuntu@[domain]:
```
- 끝에 : 꼭 써주기

## 6. zip 파일 풀기
```bash
$ sudo apt install unzip
$ unzip [zip 파일]
```

## 7. 도메인.crt 파일 생성
```bash
$ cat certificate.crt > [도메인.crt]
$ cat ca_bundle.crt >> [도메인.crt]
```
- 만약 중간 END CERTIFICATE와 BEGIN CERTIFICATE가 붙어있다면 떼어줘야함(하이픈 다섯개씩)

![github](https://user-images.githubusercontent.com/63635886/144081419-e611452d-47a3-4a42-af59-32c69fe9013e.png)

## 8. nginx config 파일 수정 (sites-avaliable)
```conf
server {
        listen 80;
        server_name [서버이름];
        return 301 https://$host$request_uri;
}

server {
        listen 443 ssl;

        server_name [서버이름];
        ssl_certificate /home/ubuntu/https/[합친 certificate파일].crt;
        ssl_certificate_key /home/ubuntu/https/private.key;
        ssl on;

        location / {
                proxy_pass [요청 돌릴 서버];
        }
}
```
## 9. nginx 문법 검사 / restart
```bash
$ sudo nginx -t
$ sudo service nginx restart
```
