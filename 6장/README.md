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
