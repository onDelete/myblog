---
title: Use Hexo
date: 2016-10-05 12:25:08
tags: hexo
categories: 博客构建
---
以下试用ubuntu下利用github pages搭建hexo博客，windows下流程与此类似，可以作参考。
## 环境搭建
* ### git
首先需要在系统中安装git：`sudo apt install git`
可以先检查是否安装git：`git --version`
<!--more-->
然后是对git的一些基本配置：
`git config --global user.name "username"`
`git config --global user.email "youremail"`
对于使用hexo你不需要太多的git技巧，还有要用到的git命令：
`git clone repo_url`（这里的仓库地址有两种形式，一种是https，一种是ssh，推荐使用ssh，ssh的速度更快，并且避免了重复验证用户名和密码的麻烦）
这里推荐<a href="http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000">廖雪峰的git教程</a>

* ### nodejs
安装：`sudo apt install nodejs`
还需安装npm(nodejs的包管理工具)：`sudo apt insatll npm`
上面两个在windows下可以直接绑定安装
npm管理工具的使用：
升级新版npm：`sudo npm install npm -g`
使用npm安装模块：`npm install module_name`
**安装hexo包：`npm install hexo-cli -g`**
查看所有全局安装的npm模块：`npm ls -g`
模块的卸载：`npm uninstall module_name`
更新模块：`npm update module_name`
但是使用npm安装模块时速度太慢，可以使用淘宝镜像或者请科学上网。<a href="https://github.com/getlantern/lantern">lantern</a>
* ### github
1.注册一个github账户，成为轮子哥口中所谓的（re）poer。
2.打开个人的profile，点击右上角的**+**，new一个repository（新建一个仓库），注意，要将你的repository name设置为[username.github.io]，这一步很重要。然后其它的不用管，直接create repository。
3.配置ssh keys，在settings中打开SSH and GPG keys，new一个ssh key。在下面的输入框中填写的你本机的ssh公钥，使用`ssh-keygen -t rsa -C "youremail"`，在你的用户目录下找到.ssh目录中的id_rsa.pub的内容，也就是你本机ssh公钥，然后点击添加。可以使用`cat id_rsa.pub`查看其中的内容。
4.验证：`ssh -T git@github.com`，看到successfully就说明配置完成了。


## 常用指令

* **新建博客项目，默认为指定的folder文件夹;**
`hexo init [folder]`
* **新建文章，总共有post、draft、page三种layout，文章以你指定的title名创建，title中如果有空格请使用“”括起来;**
`hexo new [layout] <title>`
* **生成静态文件，下面的代码为简写，可以添加-w参数监视文件的变动;**
`hexo generate`
`hexo g`
* **启动本地服务器，可以添加参数-p指定服务器的端口，默认在端口4000**
`hexo server`
`hexo s`
* **博客项目部署**
`hexo deploy`
`hexo d`
* **一键静态文件生成与部署**
`hexo g -d`
`hexo d -g`
* **清楚缓存和生成的静态文件，对应于db.json和public目录下的文件**
`hexo clean`
`hexo cl`
* **列出博客文件树**
`hexo list route`

## 补充
1.安装nodejs和npm的时候，最好使用源码包安装，ubuntu源中的安装包一般版本较旧，安装好之后可以使用npm淘宝镜像，在npmrc文件中加入`registry=https://registry.npm.taobao.org`
2.重新安装的时候要记得安装部署插件**hexo-deployer-git**(因为我是部署在github上的)。
3.安装hexo时，请使用超级用户权限，否则无法正确写入文件，要是在安装无法完成请先使用`npm cache clean`清理npm缓存。