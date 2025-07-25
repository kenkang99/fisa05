
drop시에는 자료형을 명시하지 않는다

##### 테이블 복제하기

```sql
-- 데이터 복제해서 새로 생성

create table emp01 select empno,ename,deptno from emp;
select * from emp01;

-- empno, ename, deptno emp 테이블을 가지고오되 구조만 가져와 보세요.

create table emp02 select empno, sal, hiredate, ename from emp where 1=0; -- emp 데이터 말고 구조만 가져오는 방법
```

```sql

desc emp;

# 출력결과
field   type    null   default  extra 
empno	int	NO	PRI		auto_increment
ename	varchar(20)	YES			
job	varchar(20)	YES			
mgr	smallint	YES			
hiredate	date	YES			
sal	decimal(7,2)	YES			
comm	decimal(7,2)	YES			
deptno	int	YES	MUL		

desc emp01;
# 출력결과
empno	int	NO		0	
ename	varchar(20)	YES			
deptno	int	YES			
```

! 복사를 하지만 칼럼과 자료형만 복사하고 제약조건은 옮겨오지 못함 ( 따로 설정해줘야함)

##### 조건으로 행 삭제
```sql
delete from emp01 where empno=0;
```

##### 테이블 행값 변경

```sql
-- 1. 테이블의 모든 행 변경 UPDATE 테이블명 SET 컬럼명=값
UPDATE emp01 SET deptno=50;

UPDATE emp01 SET deptno=deptno+1;

UPDATE emp01 SET deptno=60 WHERE deptno=51;

UPDATE emp01 SET deptno=60, empno=100 WHERE deptno=51;
```

##### start transaction, rollback, savepoint, commit

```sql
start transaction;
select * from emp01;
update emp01 set empno =3 where empno=100;
savepoint s1;
rollback; -- 위에 실행문들 취소
rollback to savepoint s1; -- savepoint도 사라져서 rollback이 안된다
update emp01 set empno =3 where empno=1; -- 다시 실행
savepoint s2;
commit; -- commit 후에는 rollback 적용 x 
```

##### drop table과 truncate table과 delete from table의 차이
- truncate는 데이터값만을 모두 삭제(구조는 남음), 모든 행 바로 삭제, 실행 시 바로 적용
- delete from은 데이터값만을 모두 삭제(구조는 남음), 한행 한행 삭제해서 느림 but commit시 반영이 돼 안정적
- drop은 table 자체를 모두 삭제, 실행 시 바로 적용

##### Subquery returns more than 1 row error 일 때

=을 쓰지 않고 in 을 사용하자

```sql
update emp01 set sal=sal+1000 where ename = (select e.ename from emp e, dept d where e.deptno=d.deptno and d.loc='dallas')	

# Error Code: 1242. Subquery returns more than 1 row	0.000 sec

update emp01 set sal=sal+1000 where ename in (select e.ename from emp e, dept d where e.deptno=d.deptno and d.loc='dallas'); -- 정상 실행

```

##### update join set 
```sql
-- JOIN 테이블명 ON 공통키컬럼
-- emp / emp01 : empno / ename 
-- emp / dept : deptno 
SELECT * FROM emp e JOIN emp01 e01 ON e.empno=e01.empno
					JOIN dept d ON d.deptno=e.deptno;

-- UPDATE 구문에서는 JOIN이 UPDATE 테이블명 JOIN 중복컬럼명 SET 컬럼=변경할값  
UPDATE emp01 e01 JOIN emp e ON e.empno=e01.empno
				JOIN dept d ON d.deptno=e.deptno
SET e01.sal = e01.sal+1000;
```

##### alter table


##### 인덱스 생성하기

인덱스 활용 시 btree로 이진탐색이 더 수월해진다?
```sql
create table emp01 as select * from fisa.emp;
desc emp01;

/*
empno	int	NO		0	
ename	varchar(20)	YES			
job	varchar(20)	YES			
mgr	smallint	YES			
hiredate	date	YES			
sal	decimal(7,2)	YES			
comm	decimal(7,2)	YES			
deptno	int	YES			
*/

-- empno로 검색시에 빠른 검색이 가능하게 색인 기능 적용
# empno의 MUL: 비고유 인덱스의 일부임을 나타냅니다. 비고유 인덱스는 중복된 값을 허용하며, 검색 성능을 향상시키기 위해 사용
#  검색, 정렬, 범위 검색, 조인 등의 성능을 향상
create index idx_emp01_empno on emp01(empno);
desc emp01;
SELECT * FROM emp01;

/*

*/

```

###### 자동커밋해제

```sql

select @@autocommit; -- 기본값은 1
set @@autocommit = 0;
select @@autocommit; -- 이걸 1에서 0으로 바꾸면 commit을 직접할 때까지 변경이 안됨

UPDATE emp01 SET deptno=60, empno=100 WHERE deptno=51;

commit;
```
autocommit=0 으로 해놔도 변경이 돼있어서 소용없는 걸로 보이지만 db 재접속하면 commit을 직접 하기 전까지 변경이 안돼있는 모습을 확인할 수 있다


#### 제약조건들
외래키는 다른 테이블에 데이터를 공유를 받는 키

a references table2.b - a가 자식의 칼럼, table2.b가 부모의 칼럼
```sql
# dept가 부모, emp가 자식
ALTER TABLE emp 
ADD CONSTRAINT fk_emp_dept FOREIGN KEY ( deptno ) REFERENCES dept( deptno ) 
ON DELETE NO ACTION ON UPDATE NO ACTION; 

```



-- NO ACTION 참조하고 있는 값이 있다면 부모테이블에서도 변경 불가하도록 하는 것 :  회원 탈퇴 (회원 / 주문내역), 회원 / 상품 
-- SET NULL 참조하고 있는 값이 있다면 부모테이블은 변경되고, 자식테이블에서 해당 값을 NULL로 바꿔버리는 것 : 상품 카테고리 관리 (상품 / 상품의 대분류 카테고리), 일시적 이벤트 글 / 일시적 이벤트에 관한 부가적 로그 / 대출상품 - 권유직원  
-- CASCADE 참조가 있는 값이 있다면 부모테이블도, 자식테이블도 해당 값을 변경사항으로 바꿔버리는 것 : 주문 취소 (주문내역 / 주문상품들) / 글-첨부파일 / 활성화 컬럼에 T/F


1. 회원 탈퇴(회원 / 주문내역) no action
2. 주문 취소(주문내역 / 주문상품들) cascade
3. 상품 카테고리 관리 (상품 / 상품의 대분류 카테고리) set null


-- 외래키에 추가하는 옵션
-- no action 참조하고 있는 값이 있다면 변경 불가하도록 하는 것
-- set null 참조하고 있는 값이 있다면 해당 값을 null로 바꿔버리는 것
-- cascade 참조가 있는 값이 있다면 해당 값을 부모 테이블의 변경사항으로 바꿔버리는 것


```sql

-- MODIFY 자기 자신 컬럼에만 영향을 미치는 조건들에 주로 사용합니다
ALTER TABLE emp01 MODIFY empno VARCHAR(10); -- 이미 있는 무언가를 변경하면 부가적은 효과가 따라오기 마련입니다.

-- 처음부터 테이블설계를 잘 하는 것이 중요합니다.
DESC emp01;
INSERT INTO emp01 VALUES (NULL, "12312312412541535");
ALTER TABLE emp01 MODIFY empno VARCHAR(40); -- 자료형까지는 
SELECT * FROM emp01;
DESC emp01;
ALTER TABLE emp01 MODIFY ename VARCHAR(10) NOT NULL; 
-- 이미들어있는 데이터에 변경사항과 위배되는 조건이 있다면 변경이 쉽지 않습니다.
```

기본적으로 조인 뷰는 무결성 제약이 하나도 없어도 MySQL에서는 두 개 이상의 기본 테이블을 동시에 INSERT/UPDATE/DELETE하는 것을 허용하지 않습니다
