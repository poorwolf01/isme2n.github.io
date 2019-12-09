---
layout: post
title:  "Spring"
subtitle:   "Exception Handler"
categories: dev
tags: dev JAVA
comments: true

---

## Spring Exception 처리

Spring 에서 Exception 을 처리하기 위해서는 여러가지 방법이 있겠지만 대표적으로 2가지가 있다.

Excetpion Resolver를 이용하는 방법과 ControllerAdvice 를 이용하는 방법이 되겠다. 

둘다 Dispatcher-Servlet 내의 Excpetion에 대해서 전역으로 처리가 가능하다.
하지만 그 이외의 영역에서 발생하는 Exception에 대해서 Resolver 는 

<script src="https://gist.github.com/poorwolf01/5592f08c12aba8110caca3d92bf28286.js"></script>

와 같은 인터페이스를 제공하는데 해당 인터페이스를 구현할 경우 Dispatcher-Servlet 외의 영역에서 발생한 Exception에 대해서도 처리가 가능하다.

1. Exception Resolver

Xml (Context) 추가
<script src="https://gist.github.com/poorwolf01/928c2be89b131556b98e6c87b90bf7a3.js"></script>

또는 Java Config 추가
<script src="https://gist.github.com/poorwolf01/1446f8f422354f4f6e943342a55a2015.js"></script>

Exception Resolver 기능 구현 
<script src="https://gist.github.com/poorwolf01/c989666ef54a089ad9d17dc5dae268cb.js"></script>

2. ControllerAdvice

<script src="https://gist.github.com/poorwolf01/a456f673c1d7f4b491fc28076db944ae.js"></script>

<script src="https://gist.github.com/poorwolf01/ccb5e9f2217cdbdaa4f2c39393723078.js"></script>

참조:  
https://jaehun2841.github.io/2018/08/30/2018-08-25-spring-mvc-handle-exception
https://plposer.tistory.com/68
https://springboot.tistory.com/25

---

