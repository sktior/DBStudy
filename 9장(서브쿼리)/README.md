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

## 비교할 열이 여러개인 다중열 서브쿼리
- SELECT절에 비교할 데이터를 여러 개 지정하는 방식
```
SELECT * FROM EMP
WHERE (DEPTNO, SAL) IN (SELECT DEPTNO, MAX(SAL)
                        FROM EMP
                        GROUP BY DEPTNO);
```

## FROM절에 사용하는 서브쿼리와 WITH절
- FROM절에 사용하는 서브쿼리를 인라인 뷰 라고 부른다.
```
SELECT E10.EMPNO, E10.ENAME, E10.DEPTNO, D.DNAME, D.LOC
FROM (SELECT * FROM EMP WHERE DEPTNO = 10) E10,
     (SELECT * FROM DEPT) D
WHERE E10.DEPTNO = D.DEPTNO;
```
- FROM절에 너무 많은 서브쿼리를 지정하면 가독성과 성능이 떨어질 수 있기 때문에 WITH절 사용
```
WITH
[별칭1] AS (SELECT문 1),
[별칭2] AS (SELECT문 2),
...
[별칭N) AS (SELECT문 N)
SELECT
 FROM 별칭1, 별칭2, 별칭3
 ..
 
WITH
E10 AS (SELECT * FROM EMP WHERE DEPTNO = 10),
D   AS (SELECT * FROM DEPT)
SELECT E10.EMPNO, E10.ENAME, E10.DEPTNO, D.DNAME, D.LOC
FROM E10, D
WHERE E10.DEPTNO = D.DEPTNO;
```

## SELECT절에 사용하는 서브쿼리
- 스칼라 서브쿼리라고 부르며, SELECT절에 하나의 열 영역으로서 결과를 출력할 수 있다.
```
SELECT EMPNO, ENAME, JOB, SAL,
        (SELECT GRADE
        FROM SALGRADE
        WHERE E.SAL BETWEEN LOSAL AND HISAL) AS SALGRADE,
        DEPTNO,
        (SELECT DNAME
        FROM DEPT
        WHERE E.DEPTNO = DEPT.DEPTNO) AS DNAME
        FROM EMP E;
```

# 연습문제
1.
![image](https://user-images.githubusercontent.com/42050824/99928632-4a974b80-2d8d-11eb-8b31-c79f3b55da8b.png)
```
SELECT E.JOB, E.EMPNO, E.ENAME, E.SAL, E.DEPTNO, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
AND JOB = (SELECT JOB FROM EMP WHERE ENAME = 'ALLEN');
```

2.
![image](https://user-images.githubusercontent.com/42050824/99928864-58999c00-2d8e-11eb-8064-2fa645ef9927.png)
```
SELECT E.EMPNO, E.ENAME, D.DNAME, E.HIREDATE, D.LOC, E.SAL, S.GRADE
FROM EMP E, DEPT D, SALGRADE S
WHERE E.DEPTNO = D.DEPTNO
AND E.SAL BETWEEN S.LOSAL AND S.HISAL
AND E.SAL > (SELECT AVG(SAL) FROM EMP)
ORDER BY E.SAL DESC, E.EMPNO;
```

3.
10번 부서에 근무하는 사원 중 30번 부서에는 존재하지 않는 직책을 가진 사원들의 사원 정보, 부서정보 출력
```
SELECT E.EMPNO, E.ENAME, E.JOB, E.DEPTNO, D.DNAME, D.LOC
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
AND E.DEPTNO = 10
AND E.JOB NOT IN (SELECT DISTINCT JOB FROM EMP WHERE DEPTNO = 30);
```

4. 
직책이 SALESMAN인 사람들의 최고 급여보다 높은 급여를 받는 사원들의 사원정보, 급여등급 정보 출력
```
SELECT E.EMPNO, E.ENAME, E.SAL,S.GRADE
FROM EMP E, SALGRADE S
WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL
AND E.SAL > (SELECT MAX(SAL) FROM EMP WHERE JOB = 'SALESMAN')
ORDER BY E.EMPNO ASC;

SELECT E.EMPNO, E.ENAME, E.SAL,S.GRADE
FROM EMP E, SALGRADE S
WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL
AND E.SAL > ALL (SELECT MAX(SAL) FROM EMP WHERE JOB = 'SALESMAN')
ORDER BY E.EMPNO ASC;
```
