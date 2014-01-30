title: Set Up Nginx On Vps
date: 2013-11-28 16:41:36
description: "经过一个晚上的研究，终于在我的VPS服务器上（Centos）搭建Nginx。网上的很多博客都太老了，已经起不到指导作用了。"
category: VPS
tags: [VPS,nginx,linux,博客,爱生活,爱技术]
---

先说一下，这篇文章是给那些需要在自己的VPS服务器上装Nginx的人看的。VPS版本是CentOS 6，Nginx使用是比较稳定的1.4.4版本。

<br/>

## Why Nginx

事实上，我是受怂恿的。。。因为本身，我的博客不论是Jekyll还是Hexo都是不需要这种代理服务器的。但是作为一个出色轻量级web服务器，Nginx是在是太优秀了：

* 超轻量级，占用更少的资源，对于我们这种自己掏腰包买VPS的小R玩家，每一分资源都颇显珍贵，选择Nginx而非Apache还是很有必要的。
* 并发处理，Nginx处理请求是异步非阻塞的，（这点和node.js有点不谋而合的意思。。。）
* 社区活跃，这点相信就不用我多说了，每个程序员恐怕最看重这点了。

考虑到这些，加上为了长远考虑，说不定以后要在服务器上放点App的服务什么的，我还是装了吧。

<br/>

## 强势吐槽

不知道是我搜索的方法不对还是怎么，搜到的资料实在有失水准，如果现在看到这篇文章的你和我遇到的情况是一样的，按照我的解决方案应该是没问题了。在此我先写表上日期，现在是2013年11月26日，自己判断本博客是否已经过时。有什么反馈或者问题可以通过下面的联系方式找到我。

<br/>

## 言归正传

本人的VPS是CentOS系统，Ubuntu或者其他用户仅供参考，自己斟酌。

<br/>

#### 第一步，请确定，你的系统已经安装GCC等基础yum包

<br/>

#### 第二步，建立软件源配置文件

在`/etc/yum.repos.d/`目录下建立一个`nginx.repo`软件源配置文件。命令如下： 
    # cd /etc/yum.repos.d/ 
    # vim

然后填写如下文件内容：

    [nginx] 
    name=nginx repo 
    baseurl=http://nginx.org/packages/centos/$releasever/$basearch/ 
    gpgcheck=0 
    enabled=1

执行vim命令保存文件为`nginx.repo`完整路径是`/etc/yum.repos.d/nginx.repo`

    :w nginx.repo

<br/>

#### 第三步，运行YUM

执行yum命令安装nginx

    yum install nginx 

在过程中会让你输入一个Y，回车即可。

<br/>

#### 第四步，设置反向代理

在`/etc/nginx/conf.d/`找到`default.conf`，修改`listen、server_name、proxy_pass`内容：

    server {
    listen       80;
    server_name  example.com;
    location / {
        #    root   /usr/share/nginx/html;
        #    index  index.html index.htm;
	proxy_pass   http://127.0.0.1:4000/; ##这里我用Hexo搭建的博客
    }
    }

<br/>

#### 第五步，启动Nginx

且看一下指令：

    # service nginx start
    # service nginx stop
    # service nginx restart
    # service nginx status
    # service nginx reload

<br/>

## 小结

现在回头来看，装个Nginx还是挺简单的，主要是反代理Hexo网站的时候走了些弯路。接下来我会写一些使用VPS的心得，以及在VPS上搭建git服务。

<br/>

<br/>

***

<br/>

爱生活，爱技术。
愿结识各路小伙伴。
找到我：

微博：http://weibo.com/afmz

Github：http://github.com/Ming-Zhe

E-mail：law.gravitys@gmail.com 