##### 윈도우 함수

```sql
SELECT WINDOW_FUNCTION(ARGUMENTS) 
OVER([PARTITION BY 컬럼] [ORDER BY 컬럼] [WINDOWING 절])
FROM 테이블명;
```

###### rank

```sql
/*
1
2 2 -- 동등순위가 있을 때 이렇게 처리하고 다음 등수는 3이 아니라 4로 반영되게끔해준다
4
*/
-- ORDER BY를 포함한 쿼리문에서 특정 컬럼의 순위를 구하는 함수
-- PARTITION 내에서 순위를 구할 수도 있고 전체 데이터에 대한 순위를 구할 수도 있다. 
-- 동일한 값에 대해서는 같은 순위를 부여하며 중간 순위를 비운다.
```

cume_dist와 percent_rank의 차이?

##### 변수

##### 프로시저
프로시저 시작과 끝 사이에는 ( delimiter 사이) 주석을 붙이지말것 
<- 메모리 최소화 및 에러발생가능성( 문자열 활용할 때 주석까지 사용해버림 )

##### 트리거
트리거는 테이블에 부착하는 개념이다