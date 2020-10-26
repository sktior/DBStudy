# SELECT
- 데이터 조회

# DISDINCT
- 데이터 중복 제거
```
select distinct deptno from emp;
select all job,deptno from emp;
DEFAULT는 all이다.
```

# AS
- 별칭 설정
```
select ename,sal,sal*12+comm as 연봉,comm from emp;
```

# ORDER BY
- 오름차순 내림차순
- DEFAULT는 ASC 오름차순
```
select * from emp order by sal; /* default asc(오름차순) */ 
select * from emp order by sal desc; 
select * from emp order by deptno asc, sal desc;
```
- 꼭 필요한 경우가 아니면 사용하지 않는 것이 좋다
- 많은 자원(비용)을 소모한다. -> 시간이 많이 걸림
