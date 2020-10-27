# 오라클 함수

## 오라클 함수의 종류
- 사용자 정의 함수 : 사용자가 필요에 의해 직접 정의한 사용자 정의 함수
- 내장 함수 : 오라클에서 기본적으로 제공하는 함수

## 내장 함수의 종류
- 단일행 함수 : 데이터가 한 행씩 입력되고 입력된 한 행당 결과가 하나씩 나오는 함수
- 다중행 함수 : 여러 행이 입력되어 하나의 행으로 결과가 반환되는 함수

# 문자 데이터를 가공하는 문자 함수
- 문자 데이터를 가공하거나 문자 데이터로부터 특정 결과를 얻고자 할 때 사용하는 함수
- 대 - 소문자를 바꿔주는 UPPER, LOWER, INITCAP 함수
  - UPPER(문자열) : 괄호 안 문자 데이터를 모두 대문자로 변환
  - LOWER(문자열) : 괄호 안 문자 데이터를 모두 소문자로 변환
  - INITCAP(문자열) : 괄호 안 문자 데이터 중 첫 글자는 대문자, 나머지 문자를 소문자로 변환
  ```
  SELECT ENAME, UPPER(ENAME), LOWER(ENAME), INITCAP(ENAME) FROM EMP;
  ```
  - 사용 예제 (대, 소문자 상관ㅇ벗이 검색 단어와 일치한 문자열 데이터 찾기)
  ```
  SELECT * FROM EMP
  WHERE UPPER(ENAME) = UPPER('scott');

  SELECT * FROM EMP
  WHERE UPPER(ENAME) LIKE UPPER('%scott%');
  ```
  
- 문자열 길이를 구하는 LENGTH 함수
  - 특정 문자열의 길이를 구할 때 사용
  ```
  SELECT ENAME, LENGTH(ENAME) FROM EMP;
  SELECT ENAME, LENGTH(ENAME) FROM EMP WHERE LENGTH(ENAME) >= 5;
  ```
  - LENGTHB 함수는 바이트를 출력 EX) length('한글') -> 2 // lengthb('한글') -> 4

- 문자열 일부를 추출하는 SUBSTR 함수
  - 필요한 부분만 추출하는 함수
  - SUBSTR(문자열데이터, 시작위치, 추출길이) : 문자열의 시작 위치부터 추출 길이만큼 추출, 시작 위치가 음수일 경우 마지막 위치부터 거슬러 올라간 위치에서 시작
  - SUBSTR(문자열데이터, 시작 위치) : 문자열 데이터의 시작 위치부터 문자열 데이터 끝까지 추출, 시작 위치가 음수면 마지막 위치부터 거슬러 올라간 위치에서 끝까지 추출
  ```
  SELECT JOB, SUBSTR(JOB,1,2), SUBSTR(JOB,3,2), SUBSTR(JOB,5) FROM EMP;
  ```
  
- 문자열 데이터 안에서 특정 문자 위치를 찾는 INSTR 함수
  - 문자열 데이터 안에 특정 문자나 문자열이 어디에 포함되어 있는지를 알고자 할 때 INSTR 함수 사용
  - 총 4개의 입력 값을 지정가능 하며, 최소 두 개의 입력 값(원본 문자열, 찾으려는 문자열)을 반드시 지정해야 한다.
  ```
  INSTR([대상 문자열 데이터(필수)],
        [위치를 찾으려는 부분 문자(필수)],
        [위치 찾기를 시작할 대상 문자열 데이터 위치(선택, 기본값은 1)],
        [시작 위치에서 찾으려는 문자가 몇 번째인지 지정(선택, 기본값은 1)]
  ```
  ```
  SELECT INSTR('HELLO, ORACLE!', 'L') AS INSTR_1,
       INSTR('HELLO, ORACLE!', 'L', 5) AS INSTR_2,
       INSTR('HELLO, ORACLE!', 'L', 2, 2) AS INSTR_3
       FROM DUAL;
  ```
  - 예제
  ```
  // 사원 이름중 s가 있는 행 구하기
  SELECT * FROM EMP
  WHERE INSTR(ENAME, 's') > 0;
  ```

- 특정 문자를 다른 문자로 바꾸는 REPLACE 함수
  - 특정 문자열 데이터에 포함된 문자를 다른 문자로 대체할 경우 사용
  - REPLACE([문자열 데이터 or 열 이름(필수)], [찾을 문자(필수)], [대체할 문자(선택)])
  - 만약 대체할 문자를 입력하지 않는다면 찾는 문자로 지정한 문자는 문자열 데이터에서 삭제됨
  ```
  SELECT '010-1234-5678' AS REPLACE_BEFORE,
    REPLACE('010-1234-5678', '-', ' ') AS REPLACE_1,
    REPLACE('010-1234-5678', '-') AS REPLACE_2
    FROM DUAL;
    -> 010-1234-5678 , 010 1234 5678 , 01012345678
  ```
  
- 데이터의 빈 공간을 특정 문자로 채우는 LPAD, RPAD 함수
  - 데이터와 자릿수를 지정한 후 데이터 길이가 지정한 자릿수보다 작을 경우에 나머지 공간을 특정 문자로 채우는 함수
  - LPAD는 왼쪽, RPAD는 오른쪽
  - LPAD,RPAD([문자열 데이터 or 열이름(필수)], [데이터의 자릿수(필수)], [빈 공간에 채울 문자(선택])
  ```
  SELECT 'Oracle',
    LPAD('Oracle', 10, '#') AS LPAD_1,
    RPAD('Oracle', 10, '*') AS RPAD_1,
    LPAD('Oracle', 10) AS LPAD_2,
    RPAD('Oracle', 10) AS RPAD_2
    FROM DUAL;
    -> ####Oracle, Oracle****,     Oracle, Oracle    결과값
  ```

- 두 문자열 데이터를 합치는 CONCAT 함수
  - 두 개의 문자열 데이터를 하나의 데이터로 연결하는 함수
  - 두 개의 입력 데이터 지정을 하고 열이나 문자열 데이터 모두 지정 가능
  ```
  SELECT CONCAT(EMPNO, ENAME),
    CONCAT(EMPNO, CONCAT(' : ', ENAME))
    FROM EMP
    WHERE ENAME = 'scott';
    -> 7788scott , 7788 : scott
  ```
  - ||로 concat을 대신할 수 있다.
  ```
  SELECT EMPNO || ENAME,
    EMPNO || ' : ' || ENAME
    FROM EMP;
  ```
  
- 특정 문자를 지우는 TRIM, LTRIM, RTRIM
  - 문자열 데이터 내에서 특정 문자를 지우기 위해 사용
  - TRIM([삭제할 옵션(선택)][삭제할 문자(선택)] FROM [원본 문자열 데이터(필수)])
  - 삭제할 문자가 없으면 공백이 제거됨
  ```
  // 삭제할 문자 없을 때
  SELECT '[' || TRIM(' _ _Oracle_ _ ') || ']' AS TRIM,
       '[' || TRIM(LEADING FROM ' _ _Oracle_ _ ') || ']' AS TRIM_LEADING,
       '[' || TRIM(TRAILING FROM ' _ _ Oracle_ _ ') || ']' AS TRIM_TRAILING,
       '[' || TRIM(BOTH FROM ' _ _Oracle_ _ ') || ']' AS TRIM_BOTH
       FROM DUAL;
  // 삭제할 문자 있을 때
  SELECT '[' || TRIM('_' FROM '_ _Oracle_ _') || ']' AS TRIM,
       '[' || TRIM(LEADING '_' FROM '_ _Oracle_ _') || ']' AS TRIM_LEADING,
       '[' || TRIM(TRAILING '_' FROM '_ _ Oracle_ _') || ']' AS TRIM_TRAILING,
       '[' || TRIM(BOTH '_' FROM '_ _Oracle_ _') || ']' AS TRIM_BOTH
       FROM DUAL;
  ```

# 숫자 데이터를 연산하고 수치를 조정하는 숫자 함수
- ROUND : 지정된 숫자의 특정 위치에서 반올림한 값 반환
- TRUNC : 지정된 숫자의 특정 위치에서 버림한 값 반환
- CEIL : 지정된 숫자보다 큰 정수 중 가장 작은 정수 반환
- FLOOR : 지정된 숫자보다 작은 정수 중 가장 큰 정수를 반환
- MOD : 지정된 숫자를 나눈 나머지 값 반환

## ROUND
- ROUND([숫자(필수)],[반올림 위치(선택)])
- 특정 숫자를 반올림한 결과 출력, 반올림 위치를 지정하지 않으면 소수점 첫 번째 자리에서 반올림
```
SELECT ROUND(1234.5678) AS ROUND,
       ROUND(1234.5678, 0) AS ROUND_0,
       ROUND(1234.5678, 1) AS ROUND_1,
       ROUND(1234.5678, 2) AS ROUND_2,
       ROUND(1234.5678, -1) AS ROUND_MINUS1,
       ROUND(1234.5678, -2) AS ROUND_MINUS2
       FROM DUAL;
-> 1235, 1235, 1234.6, 1234.57, 1230, 1200
```

## TRUNC
- 지정된 자리에서 숫자를 버림 처리하는 함수
- 반올림 위치를 지정하지 않으면 소수점 첫째자리에서 버림
- TRUNC([숫자(필수)],[버림 위치(선택)]
```
SELECT TRUNC(1234.5678) AS TRUNC,
       TRUNC(1234.5678, 0) AS TRUNC_0,
       TRUNC(1234.5678, 1) AS TRUNC_1,
       TRUNC(1234.5678, 2) AS TRUNC_2,
       TRUNC(1234.5678, -1) AS TRUNC_MINUS1,
       TRUNC(1234.5678, -2) AS TRUNC_MINUS2
       FROM DUAL;
-> 1234,	1234,	1234.5,	1234.56,	1230,	1200
```

## CEIL, FLOOR
- CEIL 함수와 FLOOR 함수는 각각 입력된 숫자와 가까운 큰 정수, 작은 정수를 반환하는 함수
- CEIL([숫자(필수)] , FLOOR([숫자(필수)])
```
SELECT CEIL(3.14),
       FLOOR(3.14),
       CEIL(-3.14),
       FLOOR(-3.14)
       FROM DUAL;
-> 4,	3,	-3,	-4
```

## MOD
- 나머지 구하기
- MOD([나눗셈 될 숫자(필수)], [나눌 숫자(필수)])
```
SELECT MOD(15,6),
       MOD(10,2),
       MOD(11,2)
       FROM DUAL;
-> 3,0,1
```

# 날짜 데이터를 다루는 날짜 함수
- 날짜 데이터 + 숫자 : 날짜 데이터보다 숫자만큼 일수 이후 날짜
- 날짜 데이터 - 숫자 : 날짜 데이터보다 숫자만큼 일수 이전 날짜
- 날짜 데이터 - 날짜 데이터 : 두 날짜 데이터 간의 일수 차이
- 날짜 데이터 + 날짜 데이터 : 연산 불가 (지원X)

## SYSDATE
- 날짜 함수중 가장 내표적인 함수
- 현재 날짜와 시간 보여줌
```
SELECT SYSDATE AS NOW,
       SYSDATE-1 AS YESTERDAY,
       SYSDATE+1 AS TOMORROW
       FROM DUAL;
```

## ADD_MONTHS
- 몇 개월 이후 날짜를 구하는 함수
- ADD_MONTHS([날짜 데이터(필수)], [더할 개월 수(정수)(필수)])
```
SELECT SYSDATE,
       ADD_MONTHS(SYSDATE, 3)
       FROM DUAL;
-> 오늘날짜 , 오늘날짜에 + 3개월
```

## MONTHS_BETWEEN
- 두 날짜 데이터간 개월 수 차이
- MONTHS_BETWEEN([날짜 데이터1(필수)],[날짜 데이터2(필수)]
```
SELECT EMPNO, ENAME, HIREDATE, SYSDATE,
       MONTHS_BETWEEN(HIREDATE, SYSDATE) AS MONTHS1,
       MONTHS_BETWEEN(SYSDATE, HIREDATE) AS MONTHS2,
       TRUNC(MONTHS_BETWEEN(SYSDATE, HIREDATE)) AS MONTHS3
       FROM EMP;
-> 7369	smith	80/12/17	20/10/27	-478.350956914575866188769414575866188769	478.350956914575866188769414575866188769	478
```

## 등등 필요시 찾아보기

# 자료형을 변환하는 형 변환 함수
- TO_CHAR : 숫자 또는 날짜 데이터를 문자 데이터로 변환
- TO_NUMBER : 문자 데이터를 숫자 데이터로 변환
- TO_DATE : 문자 데이터를 날짜 데이터로 변환

## TO_CHAR
- TO_CHAR([날짜데이터(필수)], '[출력되길 원하는 문자 형태(필수)]')
```
SELECT TO_CHAR(SYSDATE, 'YYYY/MM/DD HH24:MI:SS') AS 현재날짜시간 FROM DUAL;
```
- 형식 : P159

## TO_NUMBER
- 문자 데이터를 숫자 데이터로 변환
- TO_NUMBER('[문자열 데이터(필수)]', '[인식될 숫자형태(필수)]')
```
SELECT TO_NUMBER('1,300', '999,999') - TO_NUMBER('1,500', '999,999') FROM DUAL;
```

## TO_DATE
- 문자 데이터를 날짜 데이터로 변환
- TO_DATE('[문자열 데이터(필수)]', '[인식될 날짜형태(필수)]')
```
SELECT TO_DATE('2018-07-14', 'YYYY-MM-DD') AS TODATE1,
       TO_DATE('20180714', 'YYYY-MM-DD') AS TODATE2
       FROM DUAL;
       
SELECT * FROM EMP
       WHERE HIREDATE > TO_DATE('1981/06/01', 'YYYY/MM/DD');
```

# NULL 처리 함수

## NVL 함수의 기본 사용법
- NVL([NULL 인지 여부를 검사할 데이터 or 열(필수)], [앞의 데이터가 NULL일 경우 반환할 데이터](필수))
```
SELECT EMPNO, ENAME, SAL, COMM, SAL+COMM,
       NVL(COMM,0), SAL+NVL(COMM,0)
       FROM EMP;
만약 COMM이 NULL인사람은 0으로 변경, 두번째는 COMM이 NULL이면 0으로 바꾸고 SAL과 덧셈
```

## NVL2 함수의 기본 사용법
- NVL 함수와 비슷하지만 데이터가 NULL이 아닐 때 반환할 데이터를 추가로 지정 가능
- NVL2([NULL인지 여부를 검사할 데이터 or 열(필수)], [앞 데이터가 NULL이 아니면 반환할 데이터 or 계산식(필수)], [앞 데이터가 NULL일 경우 반환할 데이터 or 계산식(필수)])
```
SELECT EMPNO, ENAME, COMM,
       NVL2(COMM, '0', 'X'),
       NVL2(COMM, SAL*12+COMM, SAL*12) AS ANNSAL
       FROM EMP;
만약 COMM이 널이아니면 '0', 널이면 'X' , 두번째는 COMM이 널이아니면 SAL * 12와 COMM을 더하고, 널이면 SAL * 12만 반환
```

