> 시퀀스
- 자동으로 순차적으로 증가하는 순번을 반환하는 데이터베이스 객체
- PK값에 중복값을 방지하기 위해 사용

```
CREATE SEQUENC sequence_name
[INCREMENT BY n] // 시퀀스 번호의 증가 값으로 기본값 1
[START WITH n] // 시퀀스 시작번호로 기본값 1
[MAXVALUE n | NOMAXVALUE] //생성 가능한 시퀀스 최대값
[MINVALUE n | NOMINVALUE] //CYCLE일 경우 새로 시작되는 값과 감소하는 시퀀스일 경우 최소 값
[CYCLE | NOCYCLE] //시퀀스 번호를 순환 사용할 것인지 지정
[CACHE n | NOCACHE] //시퀀스 생성속도를 개선하기 위해 캐싱 여부 지정
```
