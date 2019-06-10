---
layout: post
title:  "Spring Maven"
subtitle:   "Maven Goal"
categories: dev
tags: dev JAVA Maven
comments: true

---

Maven Goal List


clean 	: 컴파일 결과물인 target 디렉토리 삭제
compile : 모든 소스코드 컴파일, 리소스파일을 target/classes 디렉토리에 복사
package : compile 수행 후, 테스트 수행, pom.xml 의 <packaging> 정보에 따라 패키징 수행
install : package 수행 후, local repo에 install 수행
deploy	: install 수행 후, 배포 수행


이때 맞지 않은 Goal 을 입력 시 

Unknown lifecycle phase “build”. You must specify a valid lifecycle phase or a goal in the format

와 같은 오류가 발생할 수 있다.

Maven 과 관련된 설정은 
setting.xml 을 작성하여,  MAVEN_HOME/conf/ 아래에 둘 수 있다. ( *MAVEN_HOME은 환경변수에 설정한 경로)


pom.xml 은 (Project Object Model) Project 내의 Maven 으로 Build 하는 옵션들을 설정할 수 있다.

<script src="https://gist.github.com/poorwolf01/c46d7fb592a455779b34c3b72dd9c940.js"></script>


---

