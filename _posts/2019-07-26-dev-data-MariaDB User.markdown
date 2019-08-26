---
layout: post
title:  "MariaDB"
subtitle:   "MariaDB USER"
categories: dev
tags: dev data
comments: true

---

## MariaDB USER

MariaDB(MySql)를 사용하기 위해서 USER 를 생성 하고 권한을 부여해야 한다. 

먼저 root 계정으로 접속 후

Database 를 생성 한다. 

CREATE DATABASE 데이터베이스명;

생성된 Database 는 

SHOW DATABASES; 

로 확인 할 수 있다. 

#이제 USER 를 생성하자. 

CREATE USER '아이디'@'접속위치' IDENTIFIED BY '비밀번호';

접속위치 => '%' 외부, 'localhost' 내부, 또는 IP 를 직접 지정 가능하다. 

삭제시)
DROP USER '아이디'@'접속위치';

확인)
SELECT HOST, USER, PASSWORD FROM USER;

#권한부여

GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, ALTER ON Database명.테이블 TO '아이디'@'접속위치';

전체 권한)
GRANT ALL PRIVILEGES ON *.* TO '아이디'@'접속위치'; 

권한 확인)
SHOW GRANTS FOR '아이디'@'접속위치';

권한 적용)
FLUSH PRIVILEGES;

권한 회수)
REVOKE ALL ON Database명.테이블 FROM '아이디'@'접속위치';


이제 해당 계정으로 접속하면, 

CREATE TABLE IF NOT EXISTS 데이터베이스명.테이블 (
	TEST_SEQ		BIGINT		NOT NULL
	, TEST_NAME		VARCHAR(30) NOT NULL
	, REG_DT		DATATIME	NOT NULL)
ENGINE = InnoDB;

위와 같이 Table 생성이 가능하다. 

---

