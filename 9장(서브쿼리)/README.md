# 서브쿼리란?
- 어떤 상황이나 조건에 따라 변할 수 있는 데이터 값을 비교하거나 근거로 하기 위해 SQL문 안에 작성하는 작은 SELECT 문
- SQL문을 실행하는데 필요한 데이터를 추가로 조회하기 위해 SQL 내부에서 사용하는 SELECT 문
```
SELECT []
 FROM []
 WHERE [조건식] ( SELECT []
                    FROM []
                       WHERE [])
```

## 예시
```
SELECT SAL FROM EMP WHERE ENAME = 'JONES';   // 2975

SELECT ENAME, SAL
FROM EMP
WHERE SAL > (SELECT SAL FROM EMP WHERE ENAME = 'JONES');  //2975보다 높은 사람 출력
```

## 실행 결과가 하나인 단일행 서브쿼리
- 실행 결과가 단 하나의 행으로 나오는 서브쿼리를 말한다.
- 서브쿼리에서 출력되는 결과가 하나이므로 메인 쿼리와 서브 쿼리 결과를 단일행 연산자를 사용하여 비교
- 단일행 연산자
  - > , >= , = , <= , < , <> , ^= , !=
 
## 단일행 서브쿼리와 날짜형 데이터
```
SELECT * FROM EMP
WHERE HIREDATE < (SELECT HIREDATE FROM EMP WHERE ENAME = 'SCOTT');
```
- SCOTT보다 빨리 입사한 사람 찾기

## 단일형 서브쿼리와 함수
```
SELECT E.EMPNO, E.ENAME, E.JOB, E.SAL, D.DEPTNO, D.DNAME, D.LOC
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
AND E.DEPTNO = 20
AND E.SAL > (SELECT AVG(SAL) FROM EMP);
```
- 20번 부서 사원중에 전체 사원의 평균 급여보다 높은 사람 찾기

## 실행 결과가 여러 개인 다중행 서브쿼리
- 실행 결과 행이 여러 개로 나오는 서브쿼리를 말한다.
- 서브쿼리 결과가 여러 개이므로 단일행 연산자를 사용할 수 없고, 다중행 연산자를 사용해야 메인 쿼리와 비교 가능하다.
- 다중행 연산자
  - IN (메인쿼리의 데이터가 서브쿼리의 결과 중 하나라도 일치한 데이터가 있다면 TRUE)
  - ANY & SOME (둘다 메인쿼리의 조건식을 만족하는 서브쿼리의 결과가 하나 이상이면 TRUE)
  - ALL(메인쿼리의 조건식을 서브쿼리의 결과 모두가 만족시 TRUE)
  - EXISTS(서브쿼리의 결과가 존재하면 TRUE)
  
## IN 연산자
```
SELECT * FROM EMP WHERE DEPTNO IN (20, 30);
```
- 부서 번호가 20이거나 30인 사람

```
SELECT * FROM EMP WHERE SAL IN (SELECT MAX(SAL) FROM EMP GROUP BY DEPTNO) ORDER BY DEPTNO;
```
- 각 부서별 최고 급여와 동일한 급여를 받고 있는 사람

## ANY, SOME 연산자
- 서브쿼리가 반환한 여러 결과 값 중 메인쿼리와 조건식을 사용한 결과가 하나라도 TRUE라면 메인쿼리 조건식을 TRUE로 반환해 주는 연산자
```
SELECT * FROM EMP
WHERE SAL = SOME (SELECT MAX(SAL) 
                    FROM EMP
                    GROUP BY DEPTNO);
                    

SELECT * FROM EMP
WHERE SAL = ANY (SELECT MAX(SAL) 
                    FROM EMP
                    GROUP BY DEPTNO);
```
```
SELECT * FROM EMP
WHERE SAL < ANY (SELECT SAL 
                    FROM EMP
                    WHERE DEPTNO = 30)
ORDER BY SAL, DEPTNO;
```
- 이 경우에는 30번 부서의 급여가 950, 1600, 1250, ~~ 2850까지 여러개가 나오지만 하나라도 만족하면 TRUE이므로 2850보다 작은 급여인 사람 모두 출력

## ALL 연산자
- ALL연산자는 서브쿼리의 결과가 모두 TRUE가 되어야 하는 연산자
```
SELECT * FROM EMP WHERE SAL < ALL ( SELECT SAL
                                    FROM EMP
                                    WHERE DEPTNO = 30);
```
- 30번 부서의 급여 목록이 950, 1600, 1250, ~~ 2850까지 여러개가 나오지만 950보다 작은 사원만 이 모든 목록보다 급여가 작은 사원만 출력

## EXISTS 연산자
- 서브쿼리에 결과 값이 하나 이상 존재하면 조건식이 모두 TRUE, 존재하지 않으면 모두 FALSE가 되는 연산자
- TRUE가 되는 경우
```
SELECT * FROM EMP
WHERE EXISTS ( SELECT DNAME
                FROM DEPT
                WHERE DEPTNO = 30);      // 하나이상이 TRUE이므로  SELECT * FROM EMP; 와 같은 결과값을 출력                
                
SELECT * FROM EMP
WHERE EXISTS ( SELECT DNAME
                FROM DEPT
                WHERE DEPTNO = 50);      // 50번의 부서는 없으므로 아무것도 출력 하지 않음
```
