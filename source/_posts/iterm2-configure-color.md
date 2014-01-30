title: iTerm2 配色攻略
date: 2014-01-12 22:31:12
description: "终于把 iTerm 的配色搞完了，之前因时间不够半途而废了。网上有几个 iTerm 的配色教程，跟着试了试，都不行。终于找到了解决方案。"
category: 所谓开发
tags: [爱技术,博客,iTerm]
---


终于把 iTerm 的配色搞完了，之前因时间不够半途而废了。网上有几个 iTerm 的配色教程，跟着试了试，都不行。终于找到了解决方案。

<br/>

## 预览

![](http://farm6.staticflickr.com/5487/11906135643_b25552b7dc_b.jpg)

这是我的配色结果，还有多种配色方案可供选择。

<br/>

## 一步一步来

首先打开home目录下的 `.bash_profile` 文件

	$ vim ~/.bash_profile

粘贴进去一下内容：

	CLICOLOR=1
	LSCOLORS=gxfxcxdxbxegedabagacad
	exportPS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;36m\]\w\[\033[00m\]\$ '

保存退出后。

接下来，打开 iterm 确认一下在 Preferences->Profiles->Terminal 标签下的 Terminal Emulation 选择的是 `xterm-new`，这个一定要确定（有可能需要重启 iTerm ），网上有的版本说的是 `xterm-256color`，亲测结果不对，一定要是 `xterm-new` 才可以。

之后在 Preferences->Profiles->Colors 标签，点击 `Load Preset` 列表中的 `Import` 进行导入，然后选择一种即可。



<br/>

***

<br/>

爱生活，爱技术。
愿结识各路小伙伴。
找到我：

微博：http://weibo.com/afmz

Github：http://github.com/Ming-Zhe

E-mail：law.gravitys@gmail.com 
