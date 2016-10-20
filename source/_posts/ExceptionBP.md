---
title: 异常和try...catch...finally最佳实践
date: 2016-10-10 04:26:20
tags: [Exception]
categories: JavaSE
---
有关异常的几点总结：
<!--more-->
1.Throwable是Exception和Error的父类，我们能够处理的只有Exception类，一般不要使用Throwable。
2.Exception定义的异常必须被处理，RuntimeException可以选择性地处理。RuntimeException包含有算术异常、空指针异常、类型转换异常等。
3.try...catch...finally使用的标准格式：
```java
public class ExceptionStanfordStyle {

	//在定义方法时throws异常，我们必须使用try...catch来处理异常，在处理时将异常throw到调用处，让调用处进行处理。
	public static void div(int a,int b) throws Exception{
		int result = 0;
		System.out.println("除法计算开始");
		try {
			result = a / b;
		} catch (Exception e) {
			throw e;
		} finally {
			System.out.println("除法计算结束");
		}
	}

	public static void main(String[] args) {
		try {
			ExceptionStanfordStyle.div(10, 0);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```
4.finally不被调用的情况
* 使用System.exit();
* 其他线程干扰了当前线程（通过interrupt方法）;
* jvm崩溃（crash）;