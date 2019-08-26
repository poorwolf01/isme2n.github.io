---
layout: post
title:  "MariaDB"
subtitle:   "MariaDB Character Set"
categories: dev
tags: dev data
comments: true

---

## MariaDB Charater SET

MariaDB(MySql) 은 DB, Database(schema), Table 의 각 Characterset 이 다를 수 있다. 

한글이 깨질 경우 이 Characterset을 먼저 확인해 보는 것이 좋은데

우선 DB 의 Characterset을 확인 하는 방법은 

show variables like 'c%';

Query를 실행하면 DB Server의 각 Characterset을 확인할 수 있다. 

아래 URL 을 참고로 설정파일을 수정하고 DB 를 재시작해주면 Characterset을 변경할 수 있다. 

https://mariadb.com/kb/en/mariadb/setting-character-sets-and-collations/

Database 의 경우는 Query로 확인 및 수정이 가능하다.

SELECT SCHEMA_NAME, DEFAULT_CHARACTER_SET_NAME, DEFAULT_COLLATION_NAME FROM information_schema.SCHEMATA;

ALTER DATABSET 데이터베이스명 DEFAULT CHARACTER SET utf8;

# TABLE 

SHOW FULL COLUMNS FROM 테이블명;

ALTER TABLE 테이블명 CONVERT TO CHARACTER SET utf8 COLLATE utf8_general_ci;

---

