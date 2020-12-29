# 접속 문제 Access denied for user
- mysql 접속
- use mysql;
- select host, user from user; 로 확인 -> %가 외부접속 허용된 것
- create user 'id'@'%' identified by 'password';

# 한글 insert 안될 때
```
create table `tbl_a` (
 `no_s` int,
 `name` varchar(50),
 primary key (`no_s`)
 )ENGINE=InnoDB DEFAULT CHARSET=utf8;  //테이블 생성시 설정

insert into `tbl_a` values(1,'안명성');

drop table tbl_a;
commit;

select * from tbl_a;
```

# timezon 에러 
```
SHOW VARIABLES WHERE Variable_Name LIKE "%dir"
->
-> ;

datadir 경로로 이동
my파일 열어서 default-time-zone='+9:00' 추가

관리자 권한으로 cmd 열고 mariaDB 폴더의 bin으로 이동
net stop mysql; 정지후
net start mysql; 
```
