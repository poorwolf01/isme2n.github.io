---
layout: post
title:  "Linux Time"
subtitle:   "Linux Time"
categories: dev
tags: dev linux
comments: true

---

## Linux Time

Linux (CentOS)에는 두가지 타입의 시간이 있다.
1. 마더보드(CMOS) 시간 = H/W Time
2. 서버계산 시간 = System Time

현재 시간 확인
# clock
-> H/W Time 확인

# date
-> System Time 확인


수동으로 시간 설정-일회성
# date 130812002019.00
-> date MMDDhhmmYYYY.ss
-> System Time 설정

# clock -w
-> System Time 값을 읽어 H/W Time을 설정 한다.


타임서버를 이용한 시간 설정-일회성
# rdate -p time.bora.net 
-> 타임서버(time.bora.net)의 현재시간 확인

# rdate -s time.bora.net 
-> 타임서버(time.bora.net)의 시간을 System Time 으로 설정


부팅될때 마다 타임서버와 동기화 설정
# echo "rdate -s time.bora.net" >> /etc/rc.d/rc.local   


localtime 을 서울로 설정
# ls -al /etc/localtime
-rw-r--r-- 1 root root 3519 Feb 26  2006 /etc/localtime

# ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime

# ls -al /etc/localtime
lrwxrwxrwx 1 root root 30 May 15 01:02 /etc/localtime -> /usr/share/zoneinfo/Asia/Seoul

# mv /etc/localtime /etc/localtime_org # ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime


clock, date, rdate, ntpdate - 시스템 시간정보 관리

 

시스템의 시간을 정확히 설정하는 것은 보안 측면뿐만 아니라 기본 서비스 측면에서도 중요하다. 시간 오류로 인한 문제로 DBMS에서 DB정보가 비정상적으로 처리될 수 있으며, 시스템 Log 정보가 부정확해져 분석이 불가능하게 될수 있다.

 

우선, CMOS의 시각과 시스템 시각을 관리자가 직접 설정할 수 있는 두개의 명령어(clock(hwclock), date)에 대해 배우고, 주기적으로 시간을 체크하고 자동 설정하는 방법(rdate, ntpdate)에 대해서도 알아보겠다.

 

# clock - CMOS의 시간 설정

clock은 CMOS의 시각을 설정할 수 있는 명령어이다. CMOS의 시각을 가져와 시스템운영체제 시각으로 설정할 수 있으며, 반대로 시스템 운영체제 시각을 CMOS의 시각으로 설정할 수 있다. date명령어는 단순히 시스템 즉, 시스템 운영체제의 시각을 설정하는 것이며 서버의 CMOS시각을 설정할 수는 없다. 시스템이 부팅될 때에는 CMOS의 시각이 시스템 운영체제시각에 넘겨져서 부팅 시 초기시간으로 설정된다. 따라서 부팅시마다 지속적으로 정확한 시각설정이 필요하다면 clock명령어로 CMOS의 시각을 정확하게 설정해 두어야 한다.

 

clock애서 사용되는 옵션은 아래와 같다.

-u : CMOS의 시각을 국제시각으로 조정한다.
-r : CMOS의 시각을 출력한다.
-w : 리눅스시스템 시각으로 CMOS시각을 조정한다.
-s : CMOS의 시각으로 리눅스시스템시각을 조정한다.
-a : CMOS의 시각으로 리눅스시스템시각으로 조정하고 다시 CMOS에 조정한다.

아래는 CMOS의 시간과 운영체제의 시간에 대해 비교설명한 것이다.
    구분	                           CMOS 시간	       운영체제시간
    의미	 메인보드에 있는 ROM-BIOS에서 인식하고 있는 시간.
 즉, 하드웨어적인 의미의 시간. 	 운영체제에서 인식하는 시간
 리눅스에서 인식하는 시간
 커널에서 인식하는 시간
    구분	 하드웨어적인 의미의 시간	 소프트웨어적인 의미의 시간
    관계	 운영체제(리눅스)가 부팅시 마다 CMOS의 시간을 가져와서 운영체제에 적용함.
 따라서 운영체제의 시간이 CMOS시간에 다소 종속됨.
 변경방법	clock(hwclock)명령어로 CMOS시간을 변경할 수 있음.	 data명령어로 운영체제의 
 시간을 변경할 수 있음.
 

* CMOS의 시각과 시스템 시각을 가각 확인하기

CMOS의 시각을 확인하고자 할때에는 clock명령어에 -r옵셕을 사용한다.

[root@inter-devel ~]# clock -r
2009년 10월 14일 (수) 오후 11시 09분 45초  -0.091976 seconds
[root@inter-devel ~]#
 

리눅스서버의 시스템(운영체제)시각을 확인하고자 한다면 date 명령어를 사용한다.

[root@inter-devel ~]# date
2009. 10. 14. (수) 23:11:33 KST
[root@inter-devel ~]#
 

* 리눅스시스템시각으로 CMOS시각 설정하기

대부분의 리눅스시스템의 시각은 cron에서 주기적으로 맞추는 설정을 해두게 되므로 리눅스시스템의 시각이 CMOS의 시각보다는 정확하다고 생각하므로 리눅스시스템의 시각으로 CMOS의 시각을 설정하기 위하여 "clock -w"로 CMOS 시각을 수정하였다.

 [root@inter-devel ~]# clock -w
 

* CMOS의 시각으로 리눅스시스템 시각을 설정한다.

이번에는 반대로 CMOS시각으로 리눅스시스템의 시각을 설정한 것이다.

 [root@inter-devel ~]# clock -s
 

* CMOS로 시스템시각설정 후 다시 CMOS시각을 설정하기

clock명령어에서  -a옵션을 사용하면 /etc/adjtime파일에서 3가지의 숫자를 읽어서 그 값을 참조하여 CMOS의 시각으로 리눅스시스템의 시각을 설정한 후에 다시 CMOS의 시각을 설정한다. 

 [root@inter-devel ~]# clock -a
 

 

# date - 시스템 날짜, 시간을 확인하고 설정

date명령어는 현재 서버의 날짜와시간을 확인하거나 설정할 수 있는 명령어이다. 가끔씩 서버의 날짜와 시간이 조금씩 틀리게되어 있는 경우가 있다. 이런경우에 이 명령어를 이용하여 현재서버의 날짜와시간을 확인하고 정확하게 설정하는 작업을 하기위해 사용한다.  이 명령어는 rdate와 비교를 하시면 좀 더 정확한 이해를 할 수 있다.

리눅스 서버는 새로 부팅을 하거나 재부팅을 할 때에는 서버의 CMOS에서 시간정보를 가져와서 사용한다. 따라서 CMOS의 시간이 틀릴 경우에 date명령어로 시간정보를 맞추었다 하더라도 시스템을 재부팅하면 시간정보가 틀리게 된다. 즉, data명령어에서 무엇보다 중요한 것은 clock명령어와의 정확한 구분을 할 수 있어야 한다.

 

date로 날짜와 시간을 새롭게 설정할 때에는  "date MMDDhhmmYY" 와 같은 형식을 사용하면 된다. 그리고 각 인수들은 아래와 같은 의미를 가지고 있다. 서버의 날짜와 시간을 새롭게 설정할 수 있는 권한은 오로지 root만이 할 수 있다.

       MM     월
       DD      월 중 일
       hh       시
       mm      분
       CC      연도의 처음 두 숫자 (선택적)
       YY      연도의 나중 두 숫자 (선택적)
       ss       초 (선택적)
 

또한 date는 지정된 형식에 맞는 출력을 할 수 있다. 즉 형식에 맞는 출력을 하고자 한다면 "%"지시자를 사용하여 아래 설정된 문자를 앞에 붙여 사용한다.

 시간 필드:
       %H     시 (00..23)
       %I      시 (01..12)
       %k     시 ( 0..23)
       %l      시 ( 1..12)
       %M    분 (00..59)
       %p     로케일의 AM 또는 PM
       %r     시간, 12-시간제 (hh:mm:ss [AP]M)
       %s     1970-01-01 00:00:00 UTC (비표준 확장기능)로 부터 경과된 초
       %S     초 (00..61)
       %T     시간, 24-시간 (hh:mm:ss)
       %X     로케일에서 정의한 시간 표현(%H:%M:%S)
       %Z     시간대 (에, EDT), 시간대를 결정할 수 없는 때는 아무 값도 출력하지 않는다.

        

 날짜 필드:

       %a     로케일의 약식 요일 이름 (Sun..Sat)
       %A     로케일의 완전한 요일 이름, 가변 길이 (Sunday..Saturday)
       %b     로케일의 약식 월 이름 (Jan..Dec)
       %B     로케일의 완전한 월 이름, 가변 길이 (January..December)
       %c     로케일의 날짜와 시간 (Sat Nov 04 12:02:33 EST 1989)
       %d     월 중 일 (01..31)
       %D     날짜 (mm/dd/yy)
       %h     %b 와 동일
       %j     연 중 일 (001..366)
       %m     월 (01..12)
       %U     연 중 주 번호, 일요일을 주의 첫번째 날로 생각 (00..53)
       %w     요일 번호 (0..6), 0 은 일요일
       %W     연 중 주 번호, 월요일을 주의 첫번째 날로 생각 (00..53)
       %x     로케일의 날짜 표현식 (mm/dd/yy)
       %y     연 중 일의 마지막 두 숫자 (00..99)
       %Y     연 (1970...)

 

또한 date는 다음과 같은 옵션을 이용하여 다양한 출력형식을 표현할 수 있다.

       -d datestr, --date datestr

위의 옵션은 datestr에 지정된 형식대로 출력하게 된다.

 

 

쉬운 예부터 보도록 하겠다.

아래의 예는 현재 서버의 날짜와 시간을 확인한 것이다.

[root@inter-devel ~]# date
2009. 10. 14. (수) 23:11:33 KST
[root@inter-devel ~]#
 

다음은 현재 서버의 날짜와 시간을 새롭게 설정한 것이다.

[root@host1 root]# date 050601012003
2003. 05. 06. (화) 01:01:00 KST

[[root@host1 root]# date
2003. 05. 06. (화) 01:01:06 KST
[root@host1 root]#

위의 예에서 새롭게 설정한 날짜와 시간은 2003년 5월 6일 01시 01초입니다. 그리고 date라는 명령어로 새롭게 설정된 날짜와 시간을 확인한 것이다.

 

아래의 예는 지금으로 부터 5일전의 날짜와 시간정보를 확인하고할때 사용하는 옵션이다.

[root@host1 root]# date --date '5 days ago'
2003. 09. 04. (목) 17:02:51 KST
[root@host1 root]#
 

다음의 예는 5개월하고 3일 후의 날짜와 시간정보를 출력하는 예이다.

[root@host1 root]# date --date '5 months 3 day'
2004. 02. 12. (목) 17:03:48 KST
[root@host1 root]#
 

올해 크리스마스 날짜를 출력하는 예입니다.

[root@host1 root]# date --date '25 Dec' +%j
359
[root@host1 root]#
 

 

--------------------------------------------------------------------------

 

지금까지 관리자가 직접 시간을 설정하는 것을 알아보았고. 이제는 시스템 시간을 주기적으로 동기화하는 작업에 대해서 알아보겠다.

 

# rdate - 원격타임서버로 부터 날짜시간정보 가져오기 I

rdate는 지정한 원격지의 타임서버로 부터 날짜시간정보를 받아와 보여주거나 날짜시간설정을 하는 명령어이다.

레드햇계열 및 여타 배포판에서도 별도의 설치과정 필요없이 사용할 수 있다.

 

사용형식

 rdate [-p][-s][-u][-l]  [타임서버]
 

* 원격타임서버의 시간정보 확인하기

-p옵션을 사용하면 지정한 원격지의 타임서버(아래의 예에서는 time.bora.net)에서 시간정보를 가져와서 보여준다.

[root@host1 root]# rdate -p time.bora.net
rdate: [time.bora.net]  Tue Sep  9 11:44:25 2003
즉, 위의 예는 원격 타임서버인 time.bora.net서버에서 현재 날짜시간정보를 가져와서 확인한 예이다.

 

* 원격타임서버의 시간정보 가져와서 현재로컬서버의 시간 맞추기

현재 시스템의 날짜시간정보가 틀리다는 것을 확인하고 원격지의 타임서버에서 날짜시간정보를 가져와서 현재 시스템에 적용을 한 것이다.  날짜시간설정을 하려면 -s옵션을 사용해야한다.

 [root@host1 root]# date                                (현재시스템의 날짜시간정보 확인)
2003. 09. 07. (일) 01:30:01 KST
[root@host1 root]# 
[root@host1 root]# rdate -s time.bora.net   (타임서버에서 날짜시간정보를 가져와 적용함)
[root@host1 root]# 
[root@host1 root]# date                                 (현재시스템의 변경적용된 날짜시간정보 확인)
2003. 09. 09. (화) 11:45:40 KST
[root@host1 root]#
 

참고로 사용할 수 있는 타임서버(Time Server)의 종류로는 다음과 같은 것들이 있다.
time.bora.net (보라넷)
time.kriss.re.kr (한국표준과학연구원)

 

* 재부팅시 자동 날짜시간정보 사용하기

서버관리자는 매번 이런 시간을 직접 맞추어야하는 번거로움이 있을 것이다. 이런 경우에는 다음과 같이 /etc/rc.d/rc.local 파일에 명령어를 넣어 두거나 주기적인 시간설정을 위해 cron에 넣어두기도 한다.

 

서버가 재부팅할 때마다 위의 시간설정을 하고자 한다면 /etc/rc.d/rc.local 파일에 다음과 같은 설정을 넣어두면 된다.

## Set the date & time ## 
/usr/bin/rdate -s time.kriss.re.kr
/sbin/clock -w
 

위의 설정에 대해 설명하면

/usr/bin/rdate -s time.kriss.re.kr

위의 rdate는 time.kriss.re.kr이라는 타임서버(time server)의 시각을 가져와서 현재 서버의 시각을 설정한다.

 

/sbin/clock -w

위의 "clock -w"는 현재 시스템의 시각정보를 CMOS의 시각을 재설정한다.

 

이와 같이 해두면 CMOS의 시각정보를 재설정하였기 때문에 재부팅을 하더라도 현재 재설정된 시각정보를 그대로 유지하게 된다.

 

* 주기적으로 cron에 의한 날짜시간정보 적용

이번에는 cron설정으로 날짜시간정보를 주기적으로 설정하는 예이다.

cron설정은 서버의 계정사용자라면 누구나 사용할 수 있지만 서버의 전체적인 날짜시간정보를 적용하려면 반드시 root권한으로 root의 cron설정을 해야한다. 그냥 root권한으로 "corntab -e"하면 vi모드가 열리면서 root의 cron파일을 편집할 수 있다.

  00 01 * * * su - root /usr/bin/rdate -s time.kriss.re.kr && /sbin/clock -w
참고로 root를 포함한 개별사용자의 cron파일의 보관장소는 /var/spool/cron이다.

 

 

# ntpdate - 원격타임서버로 부터 날짜시간정보 가져오기 II

NTP는 Network Time Protocol 약자로서 rdate와 비슷한 기능을 제공한다. ntpdate는 rdate를 이용한 방법보다 시간을 0.01초 이하의 오차로 맞출 수 있다.

 

ntpdate는 rdate와 다르게 별도 설치 작업이 필요하다.

# rpm -qa | grep ntp 명령으로 "ntp-버젼"이 출력되지 않으면

 

# yum install ntp   또는 
   http://rpmfind.net 에서 "ntp" 로 검색해서 배포판에 해당하는 패키지를 설치한다.

 

ex1) OS : Fedora Core 3 
#wget ftp://rpmfind.net/linux/fedora/core/3/i386/os/Fedora/RPMS/ntp-4.2.0.a.20040617-4.i386.rpm

#rpm -Uvh ntp-4.2.0.a.20040617-4.i386.rpm

 

ex2) #rpm -Uvh ftp://rpmfind.net/linux/fedora/core/3/i386/os/Fedora/RPMS/ntp-4.2.0.a.20040617-4.i386.rpm

 

동기화하는 방법은 rdate와 같으며, rdate 명령부분에 아래와 같이 입력하면 된다.

 # ntpdate time.kriss.re.kr
 

* 재부팅시 자동 날짜시간정보 사용하기

## Set the date & time ## 
/usr/sbin/ntpdate time.kriss.re.kr

/sbin/clock -w

 

* 주기적으로 cron에 의한 날짜시간정보 적용

  00 01 * * * su - root /usr/sbin/ntpdate time.kriss.re.kr && /sbin/clock -w
 

어느 것을 사용하시든 시간이 동기화 되어 local time 이 바뀌게 된다.

 

** 서버의 정확한 시간을 위한 설정시 /etc/rc.d/rc.local 파일에 등록하는 것보다는 가능한 root의 crontab에 등록하는 것이 좋다.


---

