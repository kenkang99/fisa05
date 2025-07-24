- **utf8_bin (or utf8mb4_bin) :** 바이너리 저장 값 그대로 정렬합니다.(아스키 문자가 높은 순서대로 정렬) <- 기본정렬방식
    
    ![Untitled](https://paperdoll.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F385eb2ad-fb48-44b4-b4fa-bf9e5c42c911%2FUntitled.png?table=block&id=17defd73-3489-8196-8bd5-dae73790a697&spaceId=d1a988f4-e527-4f37-84f2-5cf57ae3d75a&width=2000&userId=&cache=v2)

    
- **utf8_general_ci (or utf8mb4_general_ci):** 텍스트 정렬할 때 a 다음에 b 가 나타나야 한다는 생각으로 나온 정렬방식. 문자열 자체의 오름차순을 사용합니다.
    
    ![Untitled](https://paperdoll.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F385eb2ad-fb48-44b4-b4fa-bf9e5c42c911%2FUntitled.png?table=block&id=17defd73-3489-8196-8bd5-dae73790a697&spaceId=d1a988f4-e527-4f37-84f2-5cf57ae3d75a&width=160&userId=&cache=v2)
    
- **utf8_unicode_ci (or utf8mb4_unicode_ci) :** general_ci 보다 조금 더 사람의 기준에 맞게 여러 특수문자를 정렬합니다.

    ![Untitled](https://paperdoll.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F041f8996-570f-4a85-83a2-fedaacb0109a%2FUntitled.png?table=block&id=17defd73-3489-815a-b54a-f8d995b4c074&spaceId=d1a988f4-e527-4f37-84f2-5cf57ae3d75a&width=440&userId=&cache=v2)




비교연산자(==) 대신 그냥 대입연산자로 사용한다(=)
AND && 
OR || 
NOT !

-- workbench는 리턴되는 행의 개수가 최대 1000개까지가 디폴트임 <- preferences에서 setting 바꿔주면됨

-- movies 테이블은 몇 개의 장르 영화를 다루고 있고, rating을 어떻게 구분되어 있을까요?
select distinct genre from movies; -- unique 값만 조회
select distinct count(genre) from movies; -- 순서를 잘못 지정하여 실행하는 경우 오류
select count(distinct genre) from movies; -- 동작순서를 잘 고려한 경우

select count(distinct genre) as '개수'from movies; -- 동작순서를 잘 고려한 경우+ 칼럼명 임의 설정
select count(distinct genre) 개수 from movies; -- 동작순서를 잘 고려한 경우+ 칼럼명 임의 설정

select distinct rating from movies where country = NULL; -- NULL은 없는 것이니 이런식으로 비교연산자로 구별할 수 없음
select distinct rating from movies where country is NULL; -- NULL은 없는 것이니 이런식으로 비교연산자로 구별할 수 없음 -> is null로 명시해야함

SELECT year(release_date), title, director, production, screens, revenue, audience FROM movies 
WHERE (year(release_date) = 2024) AND (production RLIKE '롯데|쇼박스|스튜디오앤뉴|씨제이') -- MYSQL 의 OR은 ||이지만 RLIKE는 정규표현식이므로 |로 or를 나타낸다
    ORDER BY screens DESC;



select month(release_date),  quarter(release_date) -- groupby를 기준으로 select할 수 있기 때문에 quarter를 나타낼 수 없음 so groupby 절에 적혀있는 행들 또는 집계함수만 새로운 select 절로 사용가능
from movies 
where quarter(release_date)=1 and audience>=100000
group by month(release_date);


/* 2. 1) 2019년 개봉 영화 중 2) 매출액이 천만 원 이상인 영화의 월별(MONTH), 
	영화 유형별 관객 소계를 구하되
	7월 1일 전에 개봉한 영화이면 상반기,
	7월 1일 이후에 개봉한 영화이면 하반기라고 함께 출력하는 쿼리 */

select month(release_date) as 월, movie_type as 타입, sum(audience) as 총관객 ,
if(month(release_date) <=6, '상반기', '하반기') as 반기 -- groupby 기준으로 넣어야 select로 표현이 가능해짐
from movies 
where 
year(release_date) = 2019
and revenue >= 10000000
group by month(release_date) , movie_type, 반기;

select month(release_date) as 월, movie_type as 타입, sum(audience) as 총관객 ,
if(month(release_date) <=6, '상반기', '하반기') as 반기 -- groupby 기준으로 넣어야 select로 표현이 가능해짐
from movies 
where 
year(release_date) = 2019
and revenue >= 10000000
group by month(release_date) , movie_type, 반기
having 반기='상반기' # 새롭게 만들어진 기준으로 확인할 부분을 조건절로 확인하고 싶어
order by 월;

select month(release_date) as 월, movie_type as 타입, sum(audience) as 총관객 ,
if(month(release_date) <=6, '상반기', '하반기') as 반기 -- groupby 기준으로 넣어야 select로 표현이 가능해짐
from movies 
where 
year(release_date) = 2019
and revenue >= 10000000
group by month(release_date) , movie_type, 반기
having 반기='상반기' # 새롭게 만들어진 기준으로 확인할 부분을 조건절로 확인하고 싶어
order by 월
limit 2 offset 3; -- 2개만 보여줘, 근데 앞에 3개는 제치고말야

/* 3. 부제가 있는 영화 찾기 ':' 2015년 이후의 개봉영화 중에서 
부제가 달려있는 영화의 개수 세어보기 */
-- 전체 영화 중에 부제가 달려있는 영화의 비율
SELECT count(*) as 전체, sum(if(title LIKE '%:%',1,0)) as 부제 -- count 가 이미 집계이니 2번째도 집계형태로 맞춰줘야함
, round(sum(if(title LIKE '%:%',1,0)) / count(*) ,2) as 비율 -- 이거 round(전체/부제 ,2) 로 실행하면 실행이 안됨
FROM
    movies
WHERE
     YEAR(release_date) >= 2015
ORDER BY release_date;

#### inner join
```sql
# JOIN ~ ON 키워드를 직접 사용하는 방법과
select *
from emp  join dept on emp.deptno = dept.deptno;

# where 절에 테이블명.컬럼명 = 테이블2명.컬럼명 으로 사용하는 방법
select *
from emp, dept  
where emp.deptno = dept.deptno;
```
\* 로 사용하여 select하면 deptno칼럼이 두번 찍혀서 select 칼럼명을 명시하는 것이 좋다

```sql
-- 2. SMITH 의 이름(ename), 사번(empno), 근무지역(부서위치)(loc) 정보를 검색
select emp.ename as name, emp.empno as no, dept.loc as loc
from emp, dept
where emp.deptno=dept.deptno and emp.ename = 'SMITH';


-- 아래처럼 작성하면 더 빠르게 작성할 수 있다
select e.ename as name, e.empno as no, dept.loc as loc
from emp as e , dept as d -- 축약어로 정의했으면 축약어로만 사용해야함(아니면 에러남) 
where e.deptno=d.deptno and e.ename = 'SMITH'; 
```


#### not-equi 조인
```sql
/* 	2. not-equi 조인
		: 100% 일치하지 않고 특정 범위내의 데이터 조인시에 사용
		: between ~ and(비교 연산자) */
        
select e.ename, e.empno, e.sal, s.losal, s.hisal, s.grade
from emp e, salgrade s
where e.sal between s.losal and s.hisal;

select e.ename, e.empno, e.sal, s.losal, s.hisal, s.grade
from emp e
join salgrade s on e.sal between s.losal and s.hisal;
```

#### self 조인

```sql
/*
	3. self 조인 
		: 동일 테이블 내에서 진행되는 조인
		: 동일 테이블 내에서 상이한 칼럼 참조
			emp의 empno[사번]과 mgr[사번] 관계
*/


select e.empno, e.ename, e.mgr, mg.ename
from emp as e , emp as mg
where e.mgr=mg.empno;

```

##### (부가) natural 조인
```sql
SELECT 
  D.DNAME, D.LOC, E.COMM, E.ENAME
FROM
  EMP E NATURAL JOIN DEPT D -- 자기가 알아서 중복되는 칼럼명이 있다면 해당 컬럼명 기준으로 join
WHERE E.COMM IS NOT NULL;
```

### subquery
서브쿼리가 실행속도면에서 빠르다

#### 스칼라 서브쿼리(SELECT 절에서)

```sql
-- 가장 emp 테이블에서 월급을 조금 받는 사람 
select ename from emp where sal = min(emp.sal); -- 이 문법이 안됨
select ename from emp where sal = (select min(sal) from emp); -- 이 문법은 됨

select ename from emp where sal = (select sal from emp limit 1); -- 이 문법은 됨
-- 꼭 스칼라 값이 아니어도 서브쿼리의 결과를 메인쿼리에 사용할 수 있다 (튜플 형식으로)
select ename from emp where (ename,sal) = (select ename, sal from emp limit 1); -- 이 문법은 됨

```
#### 파생 테이블 서브쿼리(FROM절에서)

```sql
-- 서브쿼리 안에서는 메인쿼리의 칼럼 사용가능
select i.ename, i.deptno, d.dname, d.loc -- i꺼에서는 ename,deptno만 사용가능
from (select ename,deptno from emp where (sal+ifnull(comm,0))>2000) i, dept d
where d.deptno=i.deptno;
```


#### WHERE 절에서의 서브쿼리
조인보다는 필터조건으로 사용함

```sql
select * from emp e
where e.sal>= (SELECT e.sal FROM emp e WHERE e.deptno=20) ; -- 서브쿼리에 many row일 때 all 로 모두를 만족할 수 있는 하나의 값으로 변환해줌

select * from emp e
where e.sal>= ALL (SELECT e.sal FROM emp e WHERE e.deptno=20) ; -- 서브쿼리에 many row일 때 all 로 모두를 만족할 수 있는 하나의 값으로 변환해줌

```



desc movies; -- 테이블의 구조를 묘사하는 명령어
show databases; -- db 스키마명을 확인할 수 있는 명령어
use box_office;
show tables; -- use 한 곳의 table명을 확인할 수 있는 명령어

