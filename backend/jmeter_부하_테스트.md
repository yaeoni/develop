# jmeter를 이용해 부하를 테스트 해보자! (mac 기준)

## 1. jmeter 설치
- https://jmeter.apache.org/download_jmeter.cgi  
- zip 파일 다운로드 후 압축 풀기
#### GUI 모드 실행
- GUI 모드에서 테스트 스크립트를 작성하고 CLI로 실행한다.
```bash
$ sh ./bin/jmeter.sh
or
$ bin/jmeter.sh
```
#### CLI 모드 실행
- jmx 파일은 테스트 파일
- result path는 새로 생길 폴더 명을 지정해줘야한다.
```bash
$ ./bin/jmeter -n -t [jmxfile].jmx -l [logfile].jtl -e -o [loadtest result path]

# for me
$ rm test.jtl
$  ./apache-jmeter-5.4.1/bin/jmeter -n -t v1_space_test.jmx -l test.jtl -e -o ./test
```

---

## HTTP 테스트
### 0. Test plan
: 전체 시나리오를 관장하는 글로벌한 설정 영역

### 1. 쓰레드 그룹 설정(시나리오 덩어리, 플로우 묶음)
- jmeter는 스레드를 이용해 client session을 만들어 테스트한다.
- 쓰레드를 만들기 위해서 쓰레드 그룹 생성

![github](https://user-images.githubusercontent.com/63635886/144047817-3528c8e9-e50b-4e69-93c7-c6f8aa798c43.png)
- Number of Thread : 요청 발생시킬 클라이언트 수
- Ramp-Up Period : 해당 쓰레드 그룹의 진행 시간(스레드들의 생성 시점 결정)
- Loop Count : 해당 스레드 그룹의 발생 횟수

### 2. 샘플러 설정(하나의 트랜잭션, API 단위)
- 생성한 쓰레드 그룹에 HTTP 요청을 하기위한 샘플러 생성
![github](https://user-images.githubusercontent.com/63635886/144228338-736fd218-d0c5-4af4-91ec-8108acbd9861.png)
- Protocol / IP(Host name) / Port Number / Method / Path 설정 가능
- 하단에는 파라미터 설정에 대한 값 추가 가능
- 파일업로드도 가능(파일업로드 탭)

### 3. 리스너 설정
- 결과값 확인 가능한 리스너 (다양한 옵션 제공)
- 여러가지 리스너들을 스레드 그룹에 추가 가능
- 스레드 그룹에 대한 통계를 보고 싶으면 Thread Group을 오른쪽 마우스 클릭
- 특정 http 요청에 대한 통계를 보고 싶다면 해당되는 http request에 오른쪽 마우스 클릭

![github](https://user-images.githubusercontent.com/63635886/144229501-33793951-6448-439e-80f7-38797a91a07b.png)

### 4. 결과값 설명
![github](https://user-images.githubusercontent.com/63635886/144248188-93c4702d-50db-454b-8263-ad02d716e589.png)
- Samples : 몇 회 request 했는가
- Average : 평균 Response time
- Throughput : 초 당 몇 회의 Transaction을 처리했는가(TPS)

---

# 성능 테스트 공부

## Jmeter란
- 클라이언트 - 서버 구조로 된 소프트웨어의 성능 테스트를 위해 만들어진 순수 자바 프로그램
- 단위 / 성능 / 스트레스 테스트 등 다양한 곳에서 활용 가능

## 성능 테스트란
- 서비스 및 서비스 시스템의 성능을 확인하기 위해 실제 사용환경과 비슷한 환경에서 테스트를 진행하는 것
- 이를 통해 응답시간 / 처리량 / 병목구간 확인 가능 
- 얻은 정보로 서비스나 서비스 시스템의 문제점을 확인하고 개선 및 보완 가능

## 성능 테스트의 목적
- 실제 내가 만든 기능이 최대 몇명의 인원까지 버틸 수 있는지
- 현재 디비가 버틸 수 있는 트랜잭션의 양이 어느정도 인지
- 현재 프레임워크에서 처리속도를 유지 할 수 있는 요청양은 어디까지인지
#### 부하 테스트
- 순간 트래픽 최대치, 한계치 탐색
#### 스트레스 테스트
- 장기간 부하 발생에 대한 한계치 탐색 / 예외 동작 상황 확인
- Graceful Shutdown 정상 동작 확인
- 데이터베이스 failover, 자동복구, 예외 동작 상황 확인
- 
## 성능 테스트의 유형
- Load 테스트 : 시스템의 성능을 벤치 마크하기 위한 테스트
    - 부하를 순차적으로 증가시키며 응답시간이 급격히 증가하거나 / 더는 처리량이 증가하지 않거나 / 시스템의 CPU, Memory 값이 기준 이상으로 증가하는 등 비정상 상태가 발생하는 임계점을 찾아내고 이를 바탕으로 성능 이슈 테스트 반복
- Stress 테스트 : 비정상적인 상황의 처리 상태 확인해 시스템의 최고 성능 한계 측정
    - 임계값 이상의 요청 등
    - 램업을 길게
- Spike 테스트 : 갑자기 사용자가 몰렸을 때 요청이 정상적으로 처리되는 지 / 해당 업무 부하가 줄어들었을 때 정상 작동하는 지 테스트
    - 램업을 짧게
- Stability / Soak 테스트 : 긴 시간동안 테스트 진행
    - 테스트 시간에 따른 메모리 증가, 성능 정보의 변화 확인

## 용어
- TPS : Transaction per second
    - 1초에 처리하는 단위 작업의 수
- RPS : Request per second
    - 1초에 처리하는 HTTP 요청의 수