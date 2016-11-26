---
title: 牛客网Java试题纠错总结
date: 2016-11-25 15:06:10
tags: 面试
categories: JavaSE
---
在牛客网上断断续续的刷JAVA的专项练习，最近刚好刷完，感觉不过瘾，所以将之前收集的一些的试题整理一下，加强记忆。
1.有关类方法的描述()：
* 在类方法中可用this来调用本类的类方法 F
* 在类方法中调用本类的类方法时可直接调用 T
* 在类方法中只能调用本类中的类方法 F
* 在类方法中绝对不能调用实例方法 F
<!--more-->
类方法是指static修饰的方法，属于类本身，不可以使用this来引用，this指的是当前正在使用的这个对象；在类方法中可以使用其它类的类方法，使用类名.静态方法名即可；想在类方法中调用实例方法需要在类方法中先实例化一个对象，才可以调用。
2.下列说法正确的有()：
* 环境变量可在编译source code时指定 T
* 在编译程序时，所能指定的环境变量不包括class path F
* javac一次可同时编译数个Java源文件 T
* javac.exe能指定编译结果要置于哪个目录（directory）T

a选项-d即可设置系统属性;c选项一次编译多个java文件用javac *.java. 即可编译当前目录下的所有java文件;d选项－s指定存放生成的源文件的位置；在编译java文件的时候必须指定类的路径。
3.下列程序的结果是：
```java
public class Test{
	public static void changeStr(String str)
     {
         str = "welcome";
     }
     public static void main(String[] args)
     {
         String str = "1234";
         changeStr(str);
         System.out.println(str);
     }
}```
输出的结果是str值为1234.
字符串在编译过程中，1234和welcome都被编译到了方法区常量池中，在调用changeStr(str)的过程中，首先将str的引用传递给了changeStr(保存在在栈里，对str的引用进行了一次复制，二者都指向'1234'），然后在这里面更改str的值，相当于在栈中将传递给changeStr的引用的值变化了，但是当changeStr结束之后，栈就清空了，因此原来的引用并没有发生变化。这是JAVA值传递的特点决定的，并没有引用传递。
4.能被java.exe成功运行的java class文件必须有main()方法 T
不包含main方法的JAVA程序可以被编译，但是要想使用java.exe直接执行一个java字节码文件，该类必须有一个main方法，即程序的入口。否则会发生：
```java
$java Hello 
错误: 在类 Hello 中找不到 main 方法, 请将 main 方法定义为:
   public static void main(String[] args)
否则 JavaFX 应用程序类必须扩展javafx.application.Application
```
5.假定str0,...,str4后序代码都是只读引用。Java 7中，以上述代码为基础，在发生过一次FullGC后，上述代码在Heap空间（不包括PermGen）保留的字符数为（）
```java
static String str0="0123456789";
static String str1="0123456789";
String str2=str1.substring(5);
String str3=new String(str2);
String str4=new String(str3.toCharArray());
str0=null;
```
保留的字符书为15，‘0123456789’为字符串常量，保存在方法区常量池中，属于持久代对象（PermGen）。其它的像substring实际是new，5字符str3和4也都是new，每个5字符，分别都会创建新的对象，总共15个字符。（存疑？）
6.关于JAVA的垃圾回收机制，下面哪些结论是正确？
* 程序可以任意指定释放内存的时间 F
* JAVA程序不能依赖于垃圾回收的时间或者顺序 T
* 程序可明确地标识某个局部变量的引用不再被使用 F
* 程序可以显式地立即释放对象占有的内存 F

java提供了一个系统级的线程，即垃圾回收器线程。用来对每一个分配出去的内存空间进行跟踪。当JVM空闲时，自动回收每块可能被回收的内存，GC是完全自动的，不能被强制执行。程序员最多只能用System.gc()来建议执行垃圾回收器回收内存，但是具体的回收时间，是不可知的。对于局部变量可以直接让其引用指向null，其实局部变量被保存在栈中，随着方法的结束就会自动被回收。
7.下面程序执行的结果：
```java
public class Test
{
    public static void main(String[] args)
    {
        int x = 0;
        int y = 0;
        int k = 0;
        for (int z = 0; z < 5; z++) {
            if ((++x > 2) && (++y > 2) && (k++ > 2))
            {
                x++;
                ++y;
                k++;
            }
        }
        //每一次循环xyk的值依次为 100->200->310->420->531,if判断块内的内容并没执行 
        System.out.println(x + ”” +y + ”” +k);//结果为531
    }
}
```
这里涉及到&&符号的短路作用，以及自增的特点。
8.Math.cos为计算弧度的余弦值，Math.toRadians函数讲角度转换为弧度。
9.suspend() 和 resume() 方法：两个方法配套使用，suspend()使得线程进入阻塞状态，并且不会自动恢复，必须其对应的 resume() 被调用，才能使得线程重新进入可执行状态。
10.判断对错。在java的多态调用中，new的是哪一个类就是调用的哪个类的方法。 F
java多态有两种情况：重载和覆写。在覆写中，运用的是动态单分配，是根据new的类型确定对象，从而确定调用的方法，而且当调用子类中包含而父类并不存在的方法时，就不能多态引用子类对象`Foo f = new Sub();`，对于父子类中都有的方法和域，上面的多态引用，引用方法则是引用子类的方法，而引用域则引用父类的域；在重载中，运用的是静态多分派，即根据静态类型确定对象，因此不是根据new的类型确定调用的方法。
