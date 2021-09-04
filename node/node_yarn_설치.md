# node & yarn 설치

```bash
$ sudo apt-get install curl
$ curl -sL https://deb.nodesource.com/setup_[버전].x | sudo -E bash -
$ sudo apt-get install -y nodejs
$ sudo apt-get install build-essential
```

# nvm 설치

: Node Version Manager

- curl을 이용해 깔으려니 아래와 같은 오류가 나서, git clone하는 방식으로 nvm 설치

```bash
You have $NVM_DIR set to "/home/ubuntu/.nvm[", but that directory does not exist. Check your profile files and environment.
```

### 1. git 이용 설치

```bash
$ cd ~/
$ git clone https://github.com/nvm-sh/nvm.git .nvm
$ cd ~/.nvm
$ git checkout v0.38.0
$ . ./nvm.sh
```

### 2. bashrc 수정(ec2 환경, 이외에 bash_profile, zshrc 등이 있음)

```bash
$ vim ~/.bashrc
```

- 아래 라인을 추가한다

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

### 3. nvm 활용 node 설치 / 버전 관리

```bash
$ nvm install v[버전(ex.16)]
$ nvm uninstall
$ nvm use v[버전]
$ nvm ls
```

### 4. yarn 설치

```bash
$ npm install -g yarn
```
