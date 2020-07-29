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

> 시퀀스 생성 및 활용
```
-- board.bnum에 대한 시퀀스 생성
create SEQUENCE board_bnum_seq
start with 1
minvalue 0;
-- 삭제
drop sequence board_bnum_seq;

-- 현재 시퀀스 번호
select board_bnum_seq.currval from dual;
-- 시퀀스번호 생성
select board_bnum_seq.nextval from dual;

-- 시퀀스 수정(시작 값은 수정 불가 , 1씩 증가하게 만듦)
alter sequence board_bnum_seq
increment by 1;
-- min값은 이미 증가된 상태에서는 불가
alter sequence board_bnum_seq
increment by 1
minvalue 10;
-- 현재값보다 큰값 지정 불가
alter sequence board_bnum_seq
increment by 1
minvalue 63;

alter sequence board_bnum_seq
increment by 1
minvalue 10
maxvalue 20
cache 3
cycle;

desc board;

-- 시퀀스 삽입
insert into board(BNUM,BCATEGORY,BTITLE) values(board_bnum_seq.nextval, 1001, '테스트');
commit;
select * from board;

delete from board;
commit;
```
