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
