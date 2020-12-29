# 접속 문제 Access denied for user
- mysql 접속
- use mysql;
- select host, user from user; 로 확인 -> %가 외부접속 허용된 것
- create user 'id'@'%' identified by 'password';
