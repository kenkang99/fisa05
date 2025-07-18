# 3주차
## 5일차

###### 주석 종류
```sql

-- : 한줄 주석
# : 한줄 주석

/*
여러줄 주석
*/

```

Mysql은 소문자 대문자 구분없이 동일하게 적용할 수 있다. ( 옵션 설정 혹은 타 프로그램으로 구분할 수 있도록 할 수 있긴 함)

테이블(복수)은 데이터 튜플(단수)을 모아두는 장소 <- 그래서 테이블은 보통 복수
ex - Users, user


##### workbench의 단축키
-- Ctrl(cmd) + Enter : 쿼리 실행
-- Ctrl(cmd) + Shift + Enter : 쿼리 실행 후 결과창으로 이동
-- Ctrl + Shift + Enter : 커서가 있는 줄까지의 모든 쿼리 실행
-- Ctrl(cmd) + / : 주석 토글
-- Ctrl(cmd) + B : 쿼리 인덴트 정렬


##### 데이터베이스만들기,제거하기
CREATE database IF NOT EXISTS fisa;
DROP database IF EXISTS fisa;


##### 데이터베이스 이동하기( 클릭하는거 말고 )
USE mywork; -- 커서를 mywork에 위치시킴

##### 현재 들어와있는 데이터베이스와 외부 데이터베이스 사용할 때의 차이
상위폴더 명시하는 규칙과 유사
SELECT * FROM movies; # 현재사용중인 db 안에 있는 테이블 조회
select * from sakila.actor; # 내 사용중인 db 밖의 db를 조회