---
title:  githubpage搭建
tags: [githubpage,hexo]
toc: true
---

## 背景
[GitHub Pages](https://pages.github.com)，可以根据github
资源仓库中的文件，建立对应的静态网站。

## 资源库搭建
### 建立资源库
在Github上新建一个与用户名相关的资源库，`username.github.io`,
username是用户名（或者组织的名称）。

### clone项目
```
git clone https://github.com/username/username.github.io
```
记得修改自己的用户名

### 编辑index.html
新建一个index.html页面，并进行编辑

### 提交
```
git add --all
git commit -m "Initial commit"
git push -u origin master
```
当然，上述操作需要配置好本机的git。可以通过
`
git config --list
`
查看当前的配置。如果需要进行配置，可以通过
```
git config --global user.name "Your name"
git config --global user.email "Your email"
```
一般配置文件在~目录下的.gitconfig中可以看到。      
如果需要配置ssh key,可以参考[这里](https://help.github.com/articles/generating-an-ssh-key/)。   
```
ls -al ~/.ssh
# 列出所有存在的sshkey
```

上述内容主要来自[GitHub Pages](https://pages.github.com)，比较简单。

## hexo

### 介绍
[hexo](http://hexo.io)是一个静态博客的生成器，可以方便的与github pages联系起来。
hexo是基于nodejs的，因此，首先要安装nodejs。

### 安装

#### nodejs
采用dnf安装
```
$ dnf install nodejs
$ dnf install npm 
```
安装完成后
```
$ node --version
```
![结果](http://img0.ph.126.net/LZvVEzDRZon5j4T6Hg7_gQ==/6631892399419889046.png)


### hexo 安装
```
$ npm install hexo-cli -g
```
如果发现网络连接很慢，可以考虑换成国内的资源
```
$ npm install hexo-cli -g cnpm --registry=https://registry.npm.taobao.org
```
然后初始化一个目录

### 初始化hexo
```
$ cd /home/li/workspace/blog
$ hexo init
```