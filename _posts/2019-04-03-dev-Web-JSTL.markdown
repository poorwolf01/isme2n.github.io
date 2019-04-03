---
layout: post
title:  "JSTL"
subtitle:   "JSTL"
categories: dev
tags: dev Web
comments: true

---

## 스크립틀릿 JSTL
	
	<%
	String java = "value";
	pageContext.setAttribute("java", java);
	%>

	<c:set var="jstl" value="${pageScope.java}" />

	<c:if test="${jstl eq 'value'}">
		
	</c:if>
	
	</<script type="text/javascript">
	
	var javascript = '<%=java %>';
	var jstl = '${jstl}';

	</script>	

---

