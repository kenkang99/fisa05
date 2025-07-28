```sql
-- 비효율적 쿼리 → 모두 조회해서 파티션 사용 안함
EXPLAIN SELECT title FROM movies_p WHERE year(release_date) BETWEEN 2014 AND 2017;
-- SELECT title FROM movies_p WHERE year(release_date) BETWEEN 2014 AND 2017;
--  컬럼 그대로 비교 → 프루닝 성공
-- 이거는 filtered 11.11로 조금만 돌음, 왜 다를까? 위에는 sql함수, 윈도우 함수 year()를 사용하니 이게 먼저 전체를 실행하게돼서
EXPLAIN SELECT title FROM movies_p WHERE release_date >= '2014-01-01' AND release_date < '2018-01-01'; 
-- SELECT title FROM movies_p WHERE year(release_date) >= '2014-01-01' AND release_date < '2018-01-01' ;
```