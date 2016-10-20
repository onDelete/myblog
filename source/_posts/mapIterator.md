---
title: 关于map的迭代方式
date: 2016-10-10 03:39:04
tags: [map,iterator]
categories: JavaSE
---
最近复习java集合框架，正好看到github上有关于java中map的迭代，本来以为熟悉的点，没想到看过去就忘记了。随即又重新打开eclipse，一边看源码，一边重新敲了一下代码。
具体的代码如下：（正好可以测试一下hexo的语法高亮）
<!--more-->
```java
//学生类
public class Student {
	
	private String name;
	private int age;
	private String school;
	
	public Student(){}
	
	public Student(String name, int age, String school) {
		super();
		this.name = name;
		this.age = age;
		this.school = school;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public String getSchool() {
		return school;
	}	
}
```

```java
Map<String, Student> students = new HashMap<>();
		students.put("001", new Student("Kobe", 25, "SouthWest"));
		students.put("002", new Student("Allen", 23, "SouthWest"));
		students.put("003", new Student("James", 20, "SouthWest"));

		// 使用foreach迭代
		Set<Map.Entry<String, Student>> sets = students.entrySet();
		for (Map.Entry<String, Student> entry : sets) {
			System.out.println("编号：" + entry.getKey() + " 学生姓名：" + entry.getValue().getName());
		}
		//分别迭代value和key
		Set<String> keys = students.keySet();
		Collection<Student> collection = students.values();
		for(String key:keys){
			System.out.println("编号：" + key);
		}
		for(Student student:collection){
			System.out.println("student name:" + student.getName() + " student age:" + student.getAge() + " school:" + student.getSchool());
		}
		//使用iterator进行迭代
		Iterator<Map.Entry<String, Student>> iterator = sets.iterator();
		while(iterator.hasNext()){
			Map.Entry<String, Student> student = iterator.next();
			System.out.println("编号：" + student.getKey() + " 学生姓名：" +student.getValue().getName());

```
在迭代的过程中要涉及到泛型的一些用法，还好有IDE。

记于九九重阳。