---
layout: post
title:  "Spring"
subtitle:   "Jackson"
categories: dev
tags: dev JAVA
comments: true

---

## Spring Json 객체

Spring 을 사용할 때 @ResponseBody 를 사용하여 Ajax 로 Data 를 주고 받을 때 

"nested exception is java.lang.IllegalArgumentException: No converter found for return value of type: class java.util.ArrayList"

와 같은 Error 가 발생한다.

이는 ArrayList(또는 HashMap)를 JSON 형태로 변형해 줄 Message Converter 가 존재 하지 않아서 발생하는 것으로 

<script src="https://gist.github.com/poorwolf01/4476b5ddf1671e80f2eb83e85ff36a98.js"></script>

와 같이 Jackson Library 를 추가 해주고 


<script src="https://gist.github.com/poorwolf01/78b52c087e985eaab57a3a8aaddd34e9.js"></script>

Message Converter 를 등록 해 주면 된다. 


---

