---
title:  Fedora安装
tags: Fedora
toc: true
---

## 操作系统安装
下载操作系统，安装，不再赘述。

## 终端快捷键
设置->键盘->自定义快捷键
名称为:terminal   
命令：/usr/bin/gnome-terminal   
![新建自定义快捷键](http://img1.ph.126.net/r0aDTFg7DtQNTtpDPQVJLQ==/6631568043492828073.png)
![设置快捷键1](http://img2.ph.126.net/R_kosqiRtiJY-yW2syPLDQ==/6631518565469583956.png)   
然后点击'禁用'   
![设置快捷键2](http://img1.ph.126.net/E0sPsa4ULp1Gu13tbRUDvQ==/6632025440326887643.png)   
随便按下快捷键(ctrl+alt+t)   
![设置快捷键3](http://img1.ph.126.net/veOi2kABWK1LGCI8-hDLgg==/6632211257791991827.png)

## 卸载自带jdk
### 查看现有jdk
命令
```
rpm -aq|grep jdk
```
得到结果
```
$rpm -aq|grep jdk
copy-jdk-configs-1.1-4.fc24.noarch
java-1.8.0-openjdk-headless-1.8.0.91-7.b14.fc24.x86_64
```

### 卸载
```
dnf remove java-1.8.0-openjdk-headless-1.8.0.91-7.b14.fc24.x86_64

```

### 安装
下载jdk，到/usr/local/java中   
```
tar -zxvf jdk-8u111-linux-x64.tar.gz 
```
### 修改配置
```
vi /etc/profile
```
输入
```
export JAVA_HOME=/usr/local/java/jdk1.8.0_111
export JAVA_BIN=$JAVA_HOME/bin
export JAVA_LIB=$JAVA_HOME/lib
export CLASSPATH=.:JAVA_LIB/tools.jar:$JAVA_LIB/dt.jar
export PATH=$JAVA_BIN:$PATH
```
刷新配置
```
source /etc/profile
```
查看
```
$java -version
java version "1.8.0_111"
Java(TM) SE Runtime Environment (build 1.8.0_111-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.111-b14, mixed mode)
```

## 安装chrome
参考[这里](http://www.cnblogs.com/zhangyin6985/p/5635846.html)
### 添加chrome源
```
$cd /etc/yum.repos.d/
$wget  http://repo.fdzh.org/chrome/google-chrome-mirrors.repo 
$dnf install -y google-chrome-stable
```



## git安装

### 安装
```
dnf install git
```
### 查看
```
$ git --version
git version 2.7.4
```

## mysql安装

### 软件包安装
来自[这里](https://www.if-not-true-then-false.com/2010/install-mysql-on-fedora-centos-red-hat-rhel/)

#### 资源库安装
```
dnf install https://dev.mysql.com/get/mysql57-community-release-fc24-8.noarch.rpm
dnf install https://dev.mysql.com/get/mysql57-community-release-fc23-8.noarch.rpm
dnf install https://dev.mysql.com/get/mysql57-community-release-fc22-8.noarch.rpm
```

#### 安装   
```
dnf install mysql-community-server
```

### tar文件安装
由于网络原因，dnf安装一直失败。因此采用tar文件安装。[参考](http://www.jb51.net/article/90317.htm)

#### 下载
[下载](http://dev.mysql.com/downloads/mysql/)下的Linux Generic里的`mysql-5.7.16-linux-glibc2.5-x86_64.tar`。

#### 增加用户和用户组
```
groupadd mysql
```
   
```
useradd -r -g mysql mysql
```
为什么要增加用户和用户组呢，原因可以参考[这里](http://www.kryptosx.info/archives/456.html)，引用如下
>为什么要用这个mysql用户
一开始有点奇怪，安装软件为何还需要申请一个用户。原来这是为了使用linux的安全机制——“任意的访问控制”DAC。   
 每个文件对应所属用户，组用户，其它用户三个类型，对于三种类型用户可以分别设置读，写，执行的权限，root用户始终有最高权限。
如果mysql以mysql用户启动，那么它的权限将仅限于允许mysql用户操作的文件。然后，把数据库的用户归属设置为mysql，那样mysql正常的数据库操作就没有问题了。   
原因：这样一来，即使mysqld被黑客控制，黑客也只能得到mysql用户的权限，不是root权限。账号的shell使用/sbin/nologin，此时用户即使有密码也无法登录系统。   
缺陷：如果运行多个数据库实例在同个mysql用户下，一旦启动一个mysqld被攻破，其它实例的数据库可能会被泄密。   
注意：文件所属用户仅仅是限定用户对此文件操作的权限。文件执行时所属的用户和文件所属用户是无关的。 mysql推荐的是用mysqld_safe启动，mysqld_safe脚本在root权限下运行，进行一些需要root权限的配置，然后降权，以mysql用户权限启动mysqld，并监视其运行。



#### 解压
解压到`usr/local/mysql`。`mysql-5.7.16-linux-glibc2.5-x86_64.tar`解压得到
```
mysql-5.7.16-linux-glibc2.5-x86_64.tar.gz
mysql-test-5.7.16-linux-glibc2.5-x86_64.tar.gz
```
再次解压
```
mysql-5.7.16-linux-glibc2.5-x86_64.tar.gz
```
得到程序目录。
```
$pwd
/usr/local/mysql
$ls
bin  COPYING  docs  include  lib  man  README  share  support-files
```
#### 给用户权限
```
$chown -R mysql .
$chgrp -R mysql .
```

#### 添加环境变量
/etc/profile
```
export PATH=$PATH:/usr/local/mysql/bin
```
#### 初始化

```
mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
```
记住给出的密码：cOuIuVezj1#d。   
```
bin/mysql_ssl_rsa_setup --datadir=/usr/local/mysql/data
```

#### 增加配置文件
首先在/etc/下增加my.cnf文件，内容如下
```
[client]
port=3306
socket=/usr/local/mysql/mysql.sock

[mysqld]

basedir = /usr/local/mysql
datadir = /usr/local/mysql/data
port = 3306
socket = /usr/local/mysql/mysql.sock
user = mysql
bind-address = 0.0.0.0
server-id = 1
#sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES,这种配置会有1055错误
sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'
```
然后配置到/etc/init.d/目录
```
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
```
需要修改
```
basedir = /usr/local/mysql
datadir = /usr/local/mysql/data
```
#### 启动mysql
```
bin/mysqld_safe --user=mysql &
```
登录
```
bin/mysql --user=root –p
```
输入记住的密码：cOuIuVezj1#d   
然后修改密码
```
mysql> set password=password('XXX');
```
```
mysql>grant all privileges on *.* to root@'%' identified by 'A123456';
```

```
mysql> flush privileges
```
查看用户信息
```
mysql> use mysql;
mysql> select host,user from user;
```

#### mysql自启动

```
chmod 755 /etc/init.d/mysql
chkconfig --add mysql
chkconfig --level 345 mysql on
chkconfig --list
```

#### 遇到的问题

这时，如果用`mysql -version`，会报错
```
mysql: error while loading shared libraries:
```
执行
```
dnf install libncurses.so.5
```
继续错误
```
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)

```
原因是/tmp/mysql.sock没有权限。根本原因在于没有指定对应的配置。

#### mysql 配置文件读取顺序

## navicat使用
### 下载
点击[这里](https://www.navicat.com/download/navicat-premium)下载试用版。

### 解压
在～/usr/下，解压（tar -zxvf）
### 启动
```
./start_navicat 
```
### 解决乱码
#### navicat无法输入中文
在navicat的工作区内，输入汉字，会变成方框。
修改Tools->Options>Apperence->font的字体，不起作用。最后发现，在start_navicat文件中，将语言设置为了英文。
```
export LANG="zh_CN.UTF-8"
```
改成上述即可。

#### 查询结果乱码
在处理navicat无法输入中文的问题时，不断修改Edit Connection中的属性，Advanced中的Encoding修改成了UTF8。
这是，改为Auto，发现查询结果可以正常显示中文。
