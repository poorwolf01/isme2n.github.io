---
layout: post
title:  "MariaDB"
subtitle:   "MariaDB TimeZone"
categories: dev
tags: dev data
comments: true

---

## MariaDB TimeZone

현재 설정되어 있는 TimeZone 확인
SELECT @@system_time_zone;

TimeZone Query로 변경 (root 권한)
SET GLOBAL time_zone='timezone;

'timezone' 의 값은 아래의 여러 가지 포맷으로 줄 수 있으며, 소문자 구분이 없다(보다 자세한 사항은 MySQLkorea.com의 메뉴얼을 참고한다)
  - 'SYSTEM' 은 타임존이 시스템의 타임존과 동일하도록 설정할 때 쓴다
  - '+10:00' 또는 '-6:00' 과 같이 GMT/UTC기준 offset을 가리키는 문자열로 쓸 수도 있다
  - 'US/Eastern', 'Asia/Seoul' 과 같은 named timezone 형태의 값을 줄 수도 있다.


SELECT @@global.time_zone, @@session.time_zone;

SELECT now()

named timezone 의 경우 mysql Database 내에 
time_zone, time_zone_name, time_zone_transition, time_zone_transition_type, time_zone_leap_second 

같은 시스템 Table 의 값을 사용하는데 실제 값이 없으므로 값을 등록 해줘야 한다. 

mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -uroot -prootpassword mysql

을 실행 해주면 데이터가 등록된다. 

---

