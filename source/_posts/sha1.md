---
title: 如何得到一个字符串SHA1值
date: 2016-10-15 17:43:06
tags: sha1
categories: JavaSE
---

今天饶有兴致地想去尝试一下微信公众号的开发，遇到这样一个问题，接入开发模式需要生成一个SHA1的消息摘要。下面是在stack owerflow上看到的实现：
```java
String str = "Hello";
MessageDigest md = MessageDigest.getInstance("SHA");//此处可以指定的值为SHA、MD5
md.update(str.getBytes());
byte[] bs = md.digest();
BigInteger bigInteger = new BigInteger(1,bs);
String hashtext = bigInteger.toString(16);
System.out.println(hashtext);
```