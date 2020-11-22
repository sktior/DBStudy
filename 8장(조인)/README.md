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

