---
layout: post
title:  "BigDecimal"
subtitle:   "BigDecimal 연산"
categories: dev
tags: dev BigDecimal
comments: true

---

## BigDecimal 의 함수 정리

더하기   BigDecimal1.add(BigDecimal2)
빼기     BigDecimal1.substract(BigDecimal2)   
곱하기   BigDecimal1.multiply(BigDecimal2)
나누기   BigDecimal1.divide(BigDecimal2)

나누기의 경우 함수가 2가지가 더 있다. 

BigDecimal1.divide(BigDecimal2, BigDecimal.ROUND_UP);

BigDecimal1.divide(BigDecimal2, int, BigDecimal.ROUND_UP);

첫 번째는 BigDecimal1 을 BigDecimal2로 나누는데 ROUND_UP 을 하겠다는 뜻이다.
이 때 값의 표시되는 소수점은 BigDecimal1 의 소수점 자리수를 따라 간다.

두 번째는 BigDecimal1 을 BigDecimal2로 나누는데 ROUND_UP 을 하겠다는 뜻이며,
계산된 값의 표시되는 자리수는 int 값을 따라 간다. 

BigDecimal.ROUND_UP : 올림
BigDecimal.ROUND_DOWN : 버림
BigDecimal.ROUND_HALF_UP : 반올림
BigDecimal.ROUND_HALF_DOWN : 반내림

비교     BigDecimal1.compareTo(BigDecimal2)

리턴값이
1 인 경우 BigDecimal1 이 BigDecimal2 보다 크다.
0 인 경우 BigDecimal1 이 BigDecimal2 과 같다.
-1 인 경우 BigDecimal1 이 BigDecimal2 보다 작다.


---

