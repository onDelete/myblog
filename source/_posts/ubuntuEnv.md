---
title: ubuntu下常用java开发环境的配置
date: 2016-10-23 12:14:18
tags: [maven,tomcat]
categories: linux
---
在使用ubuntu的时候会遇到各种开发环境的配置，由于ubuntu本身自带的源往往版本要比较陈旧，所以有些环境需要手动配置最新包。
<!--more-->
下面是关于java常用的开发环境的配置，在配置下面的这些环境时，请先安装[java](https://ondelete.github.io/2016/10/06/configJavaEnvVar/)：
**MAVEN**
maven是常用项目构建的工具，下面是ubuntu下安装maven开发环境的命令：
1.去官网下载最新的tar包，然后解压缩到到本地目录
2.配置maven的环境变量：
`vim ~/.bashrc`
`export M2_HOME=mave主目录`
`export PATH=$PATH:${M2_HOME}/bin`
`source /etc/profile`
3.测试：
`mvn -v`
**tomcat**
tomcat是javaweb开发最常用的容器，下面是ubuntu下安装tomcat容器的命令：
1.去官网下载最新的tar包，然后解压缩到到本地目录
2.配置tomcat，进入tomcat目录下的bin目录，修改其startup.sh，在其中添加
`JAVA_HOME=yourjdkdir`
`JRE_HOME=${JAVA_HOME}/jre`
`CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib`
`PATH=${JAVA_HOME}/bin:$PATH`
`TOMCAT_HOME=yourtomcatdir`
注意要添加在`exec "$PRGDIR"/"$EXECUTABLE" start "$@"`之前。相应的网shutdownup.sh中也做相同的添加。
3.修改manager，打开其conf目录下的tomcat-users.xml，在其中添加：
`<role rolename="manager-gui"/>`
`<user username="yourusername" password="yourpassword" roles="manager-gui"/>`
保存相关的修改即可。
4.启动tomcat
`./startup.sh`
当看到设立了shell中出现`tomcat started.`说明配置完成。可以打开浏览器，访问**localhost:8080**，当出现一个猫脸，你的tomcat就配置好了。