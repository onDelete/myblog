---
title: 取模
date: 2016-11-01 19:12:07
tags: tips
categories: JavaSE
---
最初开始学习的java时，学到取模`%`，取模的规则感觉学懂了，但是一转眼就会忘记，很多东西都是这么忘了的，而且一到口边再也想不出来是什么。
```java
public void delivery() {
	int a = 3, b = 6, c = 7;
    System.out.println(a % b);
    System.out.println(b % c);
    System.out.println(d % a);
}
//-3 0 1
```
基本规则：
1.`a < b ==> a % b = a;`对负数也适用。
2.`a = b ==> a % b = 0;`
3.`a > b ` `a % b`别告诉我这你不会。