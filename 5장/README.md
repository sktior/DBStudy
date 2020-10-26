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
