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
deploy	: install 수행 후, 원격저장소 배포 수행
site 	: target/site에 문서 사이트 생성

이때 맞지 않은 Goal 을 입력 시 

Unknown lifecycle phase “build”. You must specify a valid lifecycle phase or a goal in the format

와 같은 오류가 발생할 수 있다.

Maven 과 관련된 설정은 
setting.xml 을 작성하여,  MAVEN_HOME/conf/ 아래에 둘 수 있다. ( *MAVEN_HOME은 환경변수에 설정한 경로)


pom.xml 은 (Project Object Model) Project 내의 Maven 으로 Build 하는 옵션들을 설정할 수 있다.

<script src="https://gist.github.com/poorwolf01/c46d7fb592a455779b34c3b72dd9c940.js"></script>

위의 내용은 전자정부 3.8 의 기본 pom.xml 이다.

<modelVersion>	: 4.0.0 은 Maven pom.xml 의 모델 버전으로 xml 의 버전이라고 볼 수 있다.
<groupId>		: 해당 프로젝트의 그룹명으로 보통 URL 의 역순을 쓴다.
<artifactId> 	: 해당 프로젝으틔 기본 아티팩트 명이다.
<packaging>		: jar, war, ear, pom 등 패키지 유형을 나타낸다.
<version>		: 애플리케이션의 버전을 나타낸다. 접미사로 -SNAPSHOT이 붙으면 아직 개발단계라는 의미이며, 메이븐에서 라이브러리를 관리하는 방식이 다르다고 한다.
<name> 			: 프로젝트 명
<description> 	: 프로젝트 설명
<url> 			: 프로젝트를 찾을 수 있는 URL
<properties> 	: pom.xml에서 중복해서 사용되는 설정(상수) 값들을 지정해놓는 부분. 다른 위치에서 ${...}로 표기해서 사용할 수 있다. (egovframework.rte.version에 3.8.0을 적용하고 다른 위치에서 ${egovframework.rte.version}이라고 쓰면 "3.8.0"이라고 쓴 것과 같다.
<profiles>		: dev, uat, prod 이런식으로 개발과 릴리즈할 때를 나눠야할 필요가 있는 설정 값은 profiles로 설정할 수 있다. maven goal 부분에 -P 옵션으로 프로파일을 선택할 수 있다. ex)compile -P prod

<dependencies>	: 의존성 Library 정보들을 기입한다. 최소한 groupId, artifactId, version 정보가 필요하며, 기입한 Library 가 다른 Library 를 필요로 한다면 자동으로 가져온다.
<dependency>	: 각 Library 들을 기입한다. 이 중 특이한 Tag 만 설명하면, 
<scope>의 경우 compile, runtime, provided, test, system 등이 올 수 있는데 해당 라이브러리가 언제 필요한지, 언제 제외되는지를 나타내는 tag 이다.
	compile		: 기본값, 모든 classpath에 추가, 컴파일 및 배포 때 같이 제공
	provided	: 실행 시 외부에서 제공, 예를들면 WAS에서 제공되어 지므로 컴파일 시에는 필요하지만, 배포시에는 빠지는 라이브러리들
	runtime		: 컴파일 시 참조되지 않고 실행때 참조
	test 		: 테스트 때만
	system 		: 저장소에서 관리하지 않고 직접 관리하는 jar 파일을 지정

<exclusion>은 선언한 각 Library 에서 동일한 Library 를 사용하나 버전이 달라 충돌이 날 때 해당 Library 를 제외 해주는 tag 이다.

<repositories>	: 빌드할 때 접근할 Library 저장소의 위치로 기본적으로 메이븐 중앙 저장소인 http://repo1.maven.org/maven2로 지정되어 있으며, 전자정부 관련은 http://www.egovframe.go.kr/maven/ 가 있다.

<build>			: Maven Build와 관련된 설정을 기입할 수 있다. 
<finalName> 	: 빌드 결과물(ex .jar) 이름 설정
<resources> 	: 리소스(각종 설정 파일)의 위치를 지정할 수 있다. - <resource> : 없으면 기본으로 "src/main/resources"
<testResources> : 테스트 리소스의 위치를 지정할 수 있다. - <testResource> : 없으면 기본으로 "src/test/resources"
<outputDirectory> : 컴파일한 결과물 위치 값 지정, 기본 "target/classes"
<testOutputDirectory> : 테스트 소스를 컴파일한 결과물 위치 값 지정, 기본 "target/test-classes"
<plugin> 		: Build 시에 특정 액션하나를 담당하는 것으로 각 plugin 마다 옵션이 다르다. 
- <executions> : 플러그인 goal과 관련된 실행에 대한 설정
- <configuration> : 플러그인에서 필요한 설정 값 지정



---

