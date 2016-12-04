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
11.What is displayed when the following is executed:
```java
double d1=-0.5;
System.out.println("Ceil d1="+Math.ceil(d1));
System.out.println("floor d1="+Math.floor(d1));
//Ceil d1=0.0
//floor d1=-1.0
```
ceil方法，大于等于参数，并且与它最接近的整数。注释：
>If the argument value is less than zero but greater than -1.0, then the result is negative zero

如果参数小于0且大于-1.0，结果为-0。ceil和floor方法（小于等于参数，且与之最接近的整数。）都有注释：
>If the argument is NaN or an infinity or positive zero or negative zero, then the result is the same as  the argument

意思为：如果参数是NaN、无穷、正0、负0，那么结果与参数相同，如果是-0.0，那么其结果是-0.0。
12.下面的输出结果是什么？
```java
public class Demo {
  public static void main(String args[])
  {  {
    String str=new String("hello");
    if(str=="hello")
    {
      System.out.println("true");
    } else {
      System.out.println("false");
    }
  }
}
//false
```
用双等号来判断两个变量是否相等时，如果两个变量是基本类型变量，且都是数值类型(不要求数据类型严格相同)，则只要两个变量的值相等，就返回true；对于两个引用类型变量，必须指向同一个对象，双等号才会返回true。
java中使用new String("hello")时，jvm会先使用常量池来贮存"hello"常量，再调用String类的构造器创建一个新的String对象，新创建的对象被保存在堆内存中；str变量指向保存在堆中的这个“hello”对象，而不是直接直接指向常量池中的“hello”，故二者不相等。
13.忽略内部接口的情况，能够修饰接口的只有public和abstract，即使不写修饰符，也不是默认修饰符，而是public。
14.以下代码将打印出:
```java
public static void main (String[] args) {
    String classFile = "com.jd.". replaceA11(".", "/") + "MyClass.class";
    System.out.println(classFile);
}
//com/jd/MyClass.class
```
由于replaceAll方法的第一个参数是一个**正则表达式**，而"."在正则表达式中表示任何字符，所以会把前面字符串的所有字符都替换成"/"。如果想替换的只是"."，那么久要写成"[.]"。
15.在Jdk1.7中，下述说法中抽象类与接口的区别正确的有哪些？
a.抽象类中可以有普通成员变量，接口中没有普通成员变量。 T
b.抽象类和接口中都可以包含静态成员常量。 T
c.一个类可以实现多个接口，但只能继承一个抽象类 T
d.抽象类中可以包含非抽象的普通方法，接口中的方法必须是抽象的，不能有非抽象的普通方法。 T
**接口（interface）**可以说成是抽象类的一种特例，接口中的所有方法都必须是抽象的。接口中的方法定义默认为public abstract类型，接口中的成员变量类型默认为public static final。另外，接口和抽象类在方法上有区别：
* 抽象类可以有构造方法，接口中不能有构造方法。
* 抽象类中可以包含非抽象的普通方法，接口中的所有方法必须都是抽象的，不能有非抽象的普通方法。
* 抽象类中可以有普通成员变量，接口中没有普通成员变量。
* 抽象类中的抽象方法的访问类型可以是public，protected和默认类型。
* 抽象类中可以包含静态方法，接口中不能包含静态方法。
* 抽象类和接口中都可以包含静态成员变量，抽象类中的静态成员变量的访问类型可以任意，但接口中定义的变量只能是public static final类型，并且默认即为public static final类型。
* 一个类可以实现多个接口，但只能继承一个抽象类。二者在应用方面也有一定的区别：接口更多的是在系统架构设计方法发接口更多的是在系统架构设计方法发挥作用挥作用，主要用于定义模块之间的通信契约。而抽象类在代码实现方面发挥作用，可以实现代码的重用，
* java8中在接口中可以有实现了的默认方法，用default修饰。

16.list是一个ArrayList的对象，哪个选项的代码填到//todo delete处，可以在Iterator遍历的过程中正确并安全的删除一个list中保存的对象？
```java
Iterator it = list.iterator();
int index = 0;
while (it.hasNext())
{
    Object obj = it.next();
    if (needDelete(obj))  //needDelete返回boolean，决定是否要删除
    {
        //todo delete
    }
    index ++;
}
```
a.it.remove(); T
b.list.remove(obj);
c.list.remove(index);
d.list.remove(obj,index);
**Iterator**支持从源集合中安全地删除对象，只需在 Iterator 上调用 remove() 即可。这样做的好处是可以避免 ConcurrentModifiedException(如循环过程中list.size()的大小变化了，就导致了错误。) ，当打开 Iterator 迭代集合时，同时又在对集合进行修改。有些集合不允许在迭代时删除或添加元素，但是调用 Iterator 的remove() 方法是个安全的做法。
17.问这个程序的输出结果。
```java
class Base
{
    public void method()
    {
        System.out.println("Base");
    } 
}
class Son extends Base
{
    public void method()
    {
        System.out.println("Son");
    }
    public void methodB()
    {
        System.out.println("SonB");
    }
}
public class Test01
{
    public static void main(String[] args)
    {
        Base base = new Son();
        base.method();
        base.methodB();
    }
}
```
a.Base SonB
b.Son SonB
c.Base Son SonB
d.编译不通过 T
由于使用父类的引用来引用子类的对象，而Son含有Base不包含的方法methodB()，所以此时就不能使用父类来引用子类方法。可能有些说我在运行的时候动态绑定到子类的methodB()方法不就好了，这是编译器层面进行的，没有动态绑定，在编译时就无法通过。
18.Java程序的种类有（ ）
a.类（Class）
b.Applet T
c.Application T
d.Servlet T
Java程序的种类有：
* 内嵌于Web文件中，由浏览器来观看的 **Applet**
* 可独立运行的 **Application**
* 服务器端的 **Servlets**

19.如下Java语句
```java
double x= 3.0;
int y=5;
x/=--y;
```
执行后， x的值是（）
**你们猜是几？**(答案在最后，我不信都会，哈哈。)
20.下面代码的输出是什么？
```java
public class Base
{
    private String baseName = "base";
    public Base()
    {
        callName();
    }
    public void callName()
    {
        System. out. println(baseName);
    }
    static class Sub extends Base
    {
        private String baseName = "sub";
        public void callName()
        {
            System. out. println (baseName) ;
        }
    }
    public static void main(String[] args)
    {
        Base b = new Sub();
    }
    //null
}
```
最后是有关对象创建以及代码执行顺序的题目，作为压轴菜。
new Sub();在创造派生类的过程中首先创建基类对象，然后才能创建派生类。创建基类即默认调用Base()方法，在方法中调用callName()方法，由于派生类中存在此方法，则被调用的callName（）方法是派生类中的方法，此时派生类还未构造，所以变量baseName的值为null。
让我们来回忆一下对象创建的步骤，当执行new操作时，先执行父类的静态代码块，然后子类静态代码块，然后父类构造器，然后子类构造器。这里还涉及到运行时动态绑定，即父类引用子类对象。

<hr>
<h2>慢慢消化吧 未完待续。。。</h2>
<h6>哦，答案是0.75</h6>