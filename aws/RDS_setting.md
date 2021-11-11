# RDS 를 생성할 때 세팅할 것 들을 정리

## 0. DB 접근 위한 보안 그룹 생성

- EC2 들어가서 보안그룹 새로 생성한다.
- 인바운드(DB 사용 포트) IPv4, IPv6 다 열어두기
  - mysql : 3306
  - postgrel : 5432
- 아웃바운드 모든 트래픽 IPv4 열어두기

## 1. RDS 생성

- 사용 DB만 맞춘다. (postgrel 같은 경우엔 버전 12 이하여야 프리티어 사용가능)
- 퍼블릭 액세스 "예" 체크

## 2. 파라미터 그룹 생성

1. DB 버전 맞추기
2. 파라미터 그룹 생성확인 및 클릭
3. 파라미터 검색 및 편집 -> 저장
   - character_set_client : utf8
   - character_set_connection : utf8
   - character_set_database : utf8
   - character_set_filesystem : utf8
   - character_set_server : utf8
   - collation_connection : utf8_general_ci
   - collation_server : utf8_general_ci
4. 적용될 디비 인스턴스 클릭 -> 수정
5. 보안그룹 / 파라미터 그룹적용
