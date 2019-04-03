---
layout: post
title:  "MyBatis"
subtitle:   "ForEach"
categories: dev
tags: dev data
comments: true

---

## foreach 사용법

<foreach collection="objList" item="obj" separator="," open="(" close=")">
	#{obj.objId} 
</foreach>

---

