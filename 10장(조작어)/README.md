## 테이블 생성
- DEPT 테이블 복사해서 DEPT_TEMP 테이블 생성
```
CREATE TABLE DEPT_TEMP AS SELECT * FROM DEPT;
```

## 삭제
DROP TABLE 테이블명;

## 추가
- 생략함 ( 이거 모르면 개발자 안하지;; )

## 급하게 테이블 복사
```
CREATE TABLE EMP_TEMP
AS SELECT * FROM EMP
WHERE 1 <> 1;
```
- 조건절에 1<>1이므로 결과가 항상 FASLE가 된다 -> 입력값은 아무것도 없는 빈 테이블

## 테이블에 날짜 데이터 입력하기
- 날짜 사이에 '/' OR '-' 붙이기
EX) '2001/01/01'  ,   '2001-01-01'

- TO_DATE 함수 사용
  - INSERT INTO EMP_TEMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
  - VALUES (2111,'이순신','MANAGER',9999,TO_DATE('07/01/2001', 'DD/MM/YYYY'),4000,NULL,20);
- sysdate 사용
- 서브쿼리를 사용해서 한 번에 여러 데이터 추가
```
// SALGRADE 테이블을 참조하여 급여 등급이 1인 사원만들 EMP_TEMP 테이블에 넣고 싶을 때

INSERT INTO EMP_TEMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
    SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE, E.SAL, E.COMM, E.DEPTNO
    FROM EMP E, SALGRADE S
    WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL
    AND S.GRADE = 1;
    
// INSERT 하는 테이블의 속성 수와 SELECT 요소 맞춰주기
```
