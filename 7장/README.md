# 다중행 함수와 데이터 그룹화

## SUM
- ENAME과 SUM(SAL) 같은 실수만 안하면됨
- SUM(DISTINCT SAL) 가능 -> COUNT,AVG도 가능

## 그외 SUM,COUNT,MAX,MIN,AVG
-  MAX(HIREDATE) // 가장 최근 입사일  // MIN도 동일

## GROUP BY
- 그룹화
```
GROUP BY[그룹화 할 열을 지정 (여러개 가능)]

SELECT ROUND(AVG(SAL),3), DEPTNO
FROM EMP
GROUP BY DEPTNO;


SELECT DEPTNO,JOB,AVG(SAL)
FROM EMP
GROUP BY DEPTNO, JOB
ORDER BY DEPTNO, JOB;
```

## HAVING
- GROUP BY 사용시 조건


## ROLLUP, CUBE, GROUPING SETS 함수
- GROUP BY절에 지정할 수 있는 특수 함수
- ROLLUP, CUBE
  - 그룹화 데이터의 합계를 출력시 사용
  ```
  GROUP BY ROLLUP[그룹화 열 지정 (여러개 가능)]
  GROUP BY CUBE[그룹화 열 지정 (여러개 가능)]
  ```

## ROLLUP
```
SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL)
FROM EMP
GROUP BY ROLLUP (DEPTNO,JOB)
ORDER BY DEPTNO,JOB;
```
![image](https://user-images.githubusercontent.com/42050824/99871774-7c3ed400-2c20-11eb-88b4-dd4cbbe977c4.png)

- 그룹별 결과를 출력하고 마지막에 테이블 전체 대상으로 한 결과를 출력한다.

## CUBE
```
SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL)
FROM EMP
GROUP BY CUBE (DEPTNO,JOB)
ORDER BY DEPTNO,JOB;
```
![image](https://user-images.githubusercontent.com/42050824/99871815-c4f68d00-2c20-11eb-8daf-21c2f894c584.png)

- ROLLUP과 다르게 CUBE는 지정한 모든 열에서 가능한 조합의 결과를 모두 출력

## GROUPING SETS
- 같은 수준의 그룹화 열이 여러 개일 때 각 열별 그룹화를 통해 결과 값을 출력
```
SELECT DEPTNO, JOB, COUNT(*)
FROM EMP
GROUP BY GROUPING SETS(DEPTNO, JOB)
ORDER BY DEPTNO, JOB;
```
![image](https://user-images.githubusercontent.com/42050824/99871918-536b0e80-2c21-11eb-8867-de644bec503d.png)

- 부서별 인원수, 직책별 인원수의 결과 값을 하나의 결과로 출력 가능
- 즉, 지정한 모든 열을 각각 대그룹으로 처리하여 출력

## 그룹화 함수 
- 그룹화 데이터의 식별이 쉽고 가독성을 높이기 위한 목적으로 사용

## GROUPING 함수
- ROLLUP이나 CUBE 함수를 사용한 GROUP BY 절에 그룹화 대상으로 지정한 열이 그룹화된 상태로 결과가 집계되었는지 확인
```
SELECT ~ ,GROUPING [GROUP BY절에 ROLLUP 또는 CUBE에 명시한 그룹화 할 열 이름]
  FROM~
  
SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL), GROUPING(DEPTNO), GROUPING(JOB)
FROM EMP
GROUP BY CUBE(DEPTNO, JOB)
ORDER BY DEPTNO, JOB;
```
![image](https://user-images.githubusercontent.com/42050824/99872023-13585b80-2c22-11eb-8305-259caa661541.png)
- 0과1로 출력이되며 0은 그룹화 되었음을 의미, 1은 그룹화 되지 않았음을 의미

## 그외 그룹화 함수 P.202
