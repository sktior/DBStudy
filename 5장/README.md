# WHERE
- 조건문

# AND , OR
- 조건 연산
```
SELECT * FROM EMP WHERE DEPTNO = 30 AND JOB = 'salesman';
select * from emp where deptno = 30 or job = 'clerk';
```

# 산술 연산자
- 더하기, 빽;, 곱하기, 나누기 등을
- WHERE 절에도 조건식에도 산술 연산자를 사용할 수 있다.
```
SELECT * FROM EMP WHERE SAL*12 = 36000;
```
# 비교 연산자
- 비교
```
SELECT * FROM EMP WHERE SAL >= 3000;
```

# 등가 비교 연산자
- 연산자 양쪽 항목이 같은 값인지 검사 등
- = : 값이 같으면 TRUE, 다르면 FALSE
- != , <> , ^= : 값이 다르면 TRUE, 같으면 FALSE  
```
SELECT * FROM EMP WHERE SAL = 800;

SELECT * FROM EMP WHERE SAL != 800;
SELECT * FROM EMP WHERE SAL <> 800;
SELECT * FROM EMP WHERE SAL ^= 800;
```

# 논리 부정 연산자
- NOT
```
SELECT * FROM EMP WHERE NOT SAL = 3000;
```

# IN 연산자
- OR 연산자 여러개를 간단하게 가능
- 특정 열에 해당하는 조건을 여러 개 지정 가능
```
SELECT * FROM EMP WHERE JOB IN ('manager','salesman','clerk');
```

# NOT IN 연산자
- 여러개의 부정 AND 사용
```
SELECT * FROM EMP WHERE JOB NOT IN ('manager','salesman','clerk');
```
- 직업이 manager,salesman,clerk가 아닌애들만 출력

# BETWEEN A AND B
- A이상~B이하 출력
```
SELECT * FROM EMP WHERE SAL BETWEEN 2000 AND 3000;
SELECT * FROM EMP WHERE SAL NOT BETWEEN 2000 AND 3000;
```

# LIKE 연산자
- 일부 문자열이 포함된 데이터 조회
- _ : 어떤 값이든 상관없이 한 개의 문자 데이터를 의미
- % : 길이와 상관없이(문자 없는 경우 포함) 모든 문자 데이터 의미
```
SELECT * FROM EMP WHERE ENAME LIKE 's%';  // s로 시작하는 데이터 조회
SELECT * FROM EMP WHERE ENAME LIKE '_l%'; // 두번째 글자가 l인 사원 조회
SELECT * FROM EMP WHERE ENAME LIKE '%le%'; // 이름에 le가 포함되어 있는 데이터 조회
SELECT * FROM EMP WHERE ENAME NOT LIKE '%le%'; // 이름에 le가 포함되어 있지 않은 데이터 조회

```

# IS NULL
- 특정 열 또는 연산의 결과 값이 NULL 인지 여부를 확인
```
select * from emp where mgr is null;
```

# 집합 연산자
- UNION
  - 연결된 SELECT 문의 결과 값을 합집합으로 묶어 준다. 중복은 제거됨
  - SELECT 문이 출력하려는 열 개수와 각 열의 자료형이 순서별로 일치해야 한다.
  ```
    SELECT empno, ename, sal, deptno
    from emp
    where deptno = 10
    union
    select empno, ename, sal, deptno
    from emp
    where deptno = 20;
  ```
- UNION ALL
  - 연결된 SELECT문의 결과 값을 합집합으로 묶어 준다. 중복된 결과 값도 모두 출력
  ```
  SELECT empno, ename, sal, deptno
    from emp
    where deptno = 10
    union all
    select empno, ename, sal, deptno
    from emp
    where deptno = 20;
  ```
- MINUS
  - 먼저 작성한 SELECT문의 결과 값에서 다음 SELECT문의 결과 값을 차집합 처리
  - 먼저 작성한 SELECT문의 결과 값 중 다음 SELECT문에 존재하지 않는 데이터만 출력
  ```
    SELECT empno, ename, sal, deptno
    from emp
    minus
    SELECT empno, ename, sal, deptno
    from emp
    where deptno = 10;
  ```
- INTERSECT
  - 먼저 작성한 SELECT문과 다음 SELECT문의 결과 값이 같은 데이터만 출력 -> 교집합
  ```
  SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
    INTERSECT
  SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
    WHERE DEPTNO = 10;
  ```
  
