# 조인
- 두 개 이상의 테이블을 연결하여 하나의 테이블처럼 출력할 때 사용하는 방식

## 조인과 집합연산자의 차이
- 조인 : 두 개 이상의 테이블 데이터를 가로로 연결한 것
- 집합 연산자 : 두 개 이상 결과 값을 세로로 연결한 것

## 여러 테이블을 사용할 때 FROM절
```
SELECT FROM 테이블1, 테이블2 , ... 테이블N

SELECT * FROM EMP, DEPT ORDER BY EMPNO;

SELECT * FROM EMP, DEPT
WHERE EMP.DEPTNO = DEPT.DEPTNO
ORDER BY EMPNO;


SELECT * FROM EMP A, DEPT B
WHERE A.DEPTNO = B.DEPTNO
ORDER BY EMPNO;
```

# 조인 종류 ( DB에서 가장 중요하다고 생각하니까 매일봐! )

## 등가조인 ( 내부조인 (INNER JOIN) )
- 테이블을 연결한 후 출력 행을 각 테이블의 특정 열에 일치한 데이터를 기준으로 선정하는 방식
```
SELECT E.EMPNO, E.ENAME, D.DEPTNO, D.DNAME, D.LOC
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
ORDER BY D.DEPTNO, E.EMPNO;

SELECT E.EMPNO, E.ENAME, D.DEPTNO, D.DNAME, D.LOC
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
AND SAL >= 3000;
```

## 비등가조인 (NON-EQUI JOIN)
- '='를 사용하지 않는 조인
```
SELECT * 
FROM EMP E, SALGRADE S
WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL;
```

## 자체조인
- 동일 테이블의 다른 컬럼을 참조하는 컬럼을 이용해서 동일 테이블간 조인하는 방법
```
SELECT E1.EMPNO, E1.ENAME, E1.MGR,
       E2.EMPNO AS MGR_EMPNO,
       E2.ENAME AS MGR_ENAME
       FROM EMP E1, EMP E2
       WHERE E1.MGR = E2.EMPNO;
```
![image](https://user-images.githubusercontent.com/42050824/99894466-296c2780-2cc7-11eb-899a-175078485552.png)

## 외부조인
- 두 테이블간 조인 수행에서 조인 기준 열의 어느 한쪽이 NULL이어도 강제로 출력하는 방식
- 왼쪽 외부 조인 : WHERE TABLE1.COL1 = TABLE2.COL1(+)
```
SELECT E1.EMPNO, E1.ENAME, E1.MGR,
       E2.EMPNO AS MGR_EMPNO,
       E2.ENAME AS MGR_ENAME
       FROM EMP E1, EMP E2
       WHERE E1.MGR = E2.EMPNO(+)
       ORDER BY E1.EMPNO;
```
![image](https://user-images.githubusercontent.com/42050824/99894522-e52d5700-2cc7-11eb-8ba3-6b3ec4b73e6d.png)

- 오른쪽 외부 조인 : WHERE TABLE1.COL1(+) = TABLE2.COL1
```
SELECT E1.EMPNO, E1.ENAME, E1.MGR,
       E2.EMPNO AS MGR_EMPNO,
       E2.ENAME AS MGR_ENAME
       FROM EMP E1, EMP E2
       WHERE E1.MGR(+) = E2.EMPNO
       ORDER BY E1.EMPNO;
```
![image](https://user-images.githubusercontent.com/42050824/99894536-042be900-2cc8-11eb-9498-9f415df0c2d2.png)

## 자연조인 ( NATURAL JOIN ) 
- 등가 조인을 대신해 사용할 수 잇는 조인방식
- 조인 대상이 되는 두 테이블에 이름과 자료형이 같은 열을 찾은 후 그 열을 기준으로 등가 조인 하는 방식
```
SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE, E.SAL, E.COMM,
       DEPTNO, D.DNAME, D.LOC
       FROM EMP E NATURAL JOIN DEPT D
       ORDER BY DEPTNO, E.EMPNO;
```
![image](https://user-images.githubusercontent.com/42050824/99894594-b2379300-2cc8-11eb-941f-d35f12463818.png)
- EMP 테이블과 DEPT 테이블에는 공통으로 DEPTNO를 가지고 있다. -> NATURAL JOIN은 자동으로 DEPTNO 열을 기준으로 등가 조인
- 기준이되는 DEPTNO에는 이름을 붙이면 안된다.

## JOIN ~ USING
- 기존 등가 조인을 대신하는 조인방식
- NATURAL JOIN과 달리 USING 키워드는 조인 기준으로 사용할 열을 명시하고 사용
```
FROM TABLE1 JOIN TABLE2 USING (조인에 사용할 기준열)

SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE, E.SAL, E.COMM,
       DEPTNO, D.DNAME, D.LOC
       FROM EMP E JOIN DEPT D USING (DEPTNO)
       WHERE SAL >= 3000
       ORDER BY DEPTNO, E.EMPNO;
```

## JOIN ~ ON 
- 가장 범용성 있는 키워드
- 기존 WHERE절에 잇는 조인 조건식을 ON 키워드 옆에 작성
```
SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE, E.SAL, E.COMM,
       E.DEPTNO, D.DNAME, D.LOC
       FROM EMP E JOIN DEPT D ON (E.DEPTNO = D.DEPTNO)
       WHERE SAL <= 3000
       ORDER BY E.DEPTNO, EMPNO;
```

## OUTER JOIN
- 왼쪽 : 기존 -> WHERE TABLE1.COL1 = TABLE2.COL1(+)
         SQL-99 -> FROM TABLE1 LEFT OUTER JOIN TABLE2 ON (조인 조건식)
- 오른쪽, 전체 외부조인도 같은 방식
```
SELECT E1.EMPNO, E1.ENAME, E1.MGR,
       E2.EMPNO AS MGR_EMPNO,
       E2.ENAME AS MGR_ENAME
       FROM EMP E1 LEFT OUTER JOIN EMP E2 ON(E1.MGR = E2.EMPNO)
       ORDER BY E1.EMPNO;
```
- 3개 이상 테이블 조인
```
SELECT * 
FROM TABLE1, TABLE2, TABLE3
WHERE TABLE1.COL = TABLE2.COL
AND TABLE2.COL = TABLE3.COL;
```


# 연습문제
1.
![image](https://user-images.githubusercontent.com/42050824/99895553-0abc5f80-2ccc-11eb-8391-c2dd1f9ba84e.png)
```
SELECT DEPTNO, E1.DNAME, E2.EMPNO, E2.ENAME, E2.SAL
FROM DEPT E1 NATURAL JOIN EMP E2
WHERE E2.SAL > 2000
ORDER BY DEPTNO;

SELECT E1.DEPTNO, E1.DNAME, E2.EMPNO, E2.ENAME, E2.SAL
FROM DEPT E1, EMP E2
WHERE E1.DEPTNO = E2.DEPTNO
AND E2.SAL > 2000
ORDER BY E2.DEPTNO;
```

2.
![image](https://user-images.githubusercontent.com/42050824/99895760-38ee6f00-2ccd-11eb-8c55-f491071fe92f.png)
```
SELECT E1.DEPTNO, E1.DNAME, TRUNC(AVG(SAL)) AS AVG_SAL, MAX(SAL) AS MAX_SAL, MIN(SAL) AS MIN_SAL, COUNT(*) AS CNT
FROM DEPT E1, EMP E2
WHERE E1.DEPTNO = E2.DEPTNO
GROUP BY E1.DEPTNO, E1.DNAME;

SELECT DEPTNO, E1.DNAME, TRUNC(AVG(SAL)) AS AVG_SAL, MAX(SAL) AS MAX_SAL, MIN(SAL) AS MIN_SAL, COUNT(*) AS CNT
FROM DEPT E1 JOIN EMP E2 USING (DEPTNO)
GROUP BY DEPTNO, E1.DNAME;
```

3.
![image](https://user-images.githubusercontent.com/42050824/99895854-350f1c80-2cce-11eb-9fdd-8a02989e4911.png)
```
SELECT E1.DEPTNO, E1.DNAME, E2.EMPNO, E2.ENAME, E2.JOB, E2.SAL
FROM DEPT E1, EMP E2
WHERE E1.DEPTNO = E2.DEPTNO(+)
ORDER BY E1.DEPTNO, E1.DNAME;

SELECT E1.DEPTNO, E1.DNAME, E2.EMPNO, E2.ENAME, E2.JOB, E2.SAL
FROM DEPT E1 LEFT OUTER JOIN EMP E2 ON (E1.DEPTNO = E2.DEPTNO)
ORDER BY E1.DEPTNO;
```
