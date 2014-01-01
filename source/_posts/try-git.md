title: Try Git
date: 2013-11-28 16:11:00
tags: [博客,Git,爱技术]
description: "I am an egotistical bastard, and I name all my projects after myself. First Linux, now git.林纳斯·托瓦兹自嘲地取了这个名字'git'。在英式英语中指一个愚笨或者不开心的人。"
category: 所谓开发
---


> I'm an egotistical bastard, and I name all my projects after myself. First Linux, now git.

[林纳斯·托瓦兹](http://zh.wikipedia.org/wiki/%E6%9E%97%E7%BA%B3%E6%96%AF%C2%B7%E6%89%98%E7%93%A6%E5%85%B9)自嘲地取了这个名字"git"。在英式英语中指一个愚笨或者不开心的人。

## Git
说到版本控制、软件项目管理等概念，在开源时代到来的今天，Git绝对是一个绕不开的软件。更多有关Git的官方解释，我也不赘述了。

* [Git维基百科](http://zh.wikipedia.org/wiki/Git)
* [Git参考手册](http://gitref.cyj.me/zh/)
* [Git源码](https://github.com/git/git)
* [Git内部构造](http://youngsterxyf.github.io/2013/09/28/learning-git-internals-by-example/)

## Try Git
重点来了
我们不是技术大牛，不可能上来就熟练使用git全部操作。我接触git不到1个月，用的也只是它的基本粗浅功能（在我的[Github](http://github.com/Ming-Zhe)就能体现出来）。不一定有什么指导意义，还是想分享一下整个Try Git的过程。

### 初吻
惊艳的第一次来自于Github的[Training](http://training.github.com/)，在小伙伴的怂恿下开始了git之旅。
### 用git管理Github
如果上一阶段是初吻，那这里就是XXX，应该说是比初吻要更强烈的快感。用git命令行来控制自己项目的各种操作，看上去就有种瞬间变身技术大牛的感觉（有点自欺欺人）。
这里第一步比较麻烦一些，生成SSH Keys，建立与操作电脑的链接。详细参见来自Github的[帮助](https://help.github.com/articles/generating-ssh-keys)
之后就是从Training学来的东西，反复的操作。
如果没有记牢的话，除了可以跟着Training再来一遍，[Code School](https://www.codeschool.com/courses/try-git)里也有相应的免费教程。

## Use Git
现在，我们可以用Git进行最基本的项目操作，保证你的Github每天都有动态。这个时候和你打交道的基本就是就是一下几条语句：

*  git add .    添加所有修改文件到缓存
*  git commit -m "xxx"    记录缓存内容的快照
*  git push origin master    推送你的master分支与数据到某个远端仓库的master分支
*  git pull    从远端仓库提取数据并尝试合并到当前分支

## 总结
耐下性子，按照步骤，一步一步来，很快就能熟练掌握git。说到底也没有多少东西。

<br/>

***

<br/>

爱生活，爱技术。
愿结识各路小伙伴。
找到我：

微博：http://weibo.com/afmz
Github：http://github.com/Ming-Zhe
E-mail：law.gravitys@gmail.com 