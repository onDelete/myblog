---
title: 配置java环境变量
date: 2016-10-06 12:51:22
tags: java环境变量
categories: linux
---
## Linux下的java环境变量的配置
在windows下操作，由于有图形化界面的帮助，环境变量的配置很人性化，尤其是在win10以后，环境变量的配置甚至都不需要在原来的特别长的path变量中添加“;“，简单几步即可完成。当我第一次使用ubuntu时，想在其中安装个java，配置环境变量都无从下手，linux下学习安装软件是一个比较复杂的过程，有很多依赖关系需要搞明白，但是当学习一段时间之后，发现在命令行下的操作也是相当方便的。
<!--more-->
**linux下java环境变量的配置**
1.去oracle官网上下载相应的jdk的包，Ubuntu下对应的下载.tar.gz的包，将其解压，解压可以直接使用Ubuntu的图形化界面工具进行操作也可以使用命令行，然后将其移动到指定的文件夹下。
`sudo mkdir /usr/local/java`
`sudo tar -zxvf yourJDKDirName -C /usr/local/java/`
2.配置环境变量
`sudo vim ~/.bashrc`
`export JAVA_HOME=/usr/local/java/jdk-...`
`export JRE_HOME=${JAVA_HOME}/jre`
`export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib`
`export PATH=${JAVA_HOME}/bin:$PATH`
这里在配置时也可以使用其它文本编辑器，也可以学习一些vim的简单操作。
3.`source /etc/profile`
4.测试
在终端输入：`java` `javac`

至此环境变量就配置好了。
