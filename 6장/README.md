# 오라클 함수

## 오라클 함수의 종류
- 사용자 정의 함수 : 사용자가 필요에 의해 직접 정의한 사용자 정의 함수
- 내장 함수 : 오라클에서 기본적으로 제공하는 함수

## 내장 함수의 종류
- 단일행 함수 : 데이터가 한 행씩 입력되고 입력된 한 행당 결과가 하나씩 나오는 함수
- 다중행 함수 : 여러 행이 입력되어 하나의 행으로 결과가 반환되는 함수

## 문자 데이터를 가공하는 문자 함수
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
