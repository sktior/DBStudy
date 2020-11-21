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
