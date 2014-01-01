title: High Time Function
date: 2013-11-28 16:36:26
description: "这是一个比较有意思的JavaScript功能，很方便添加到你的博客中，快进来看看源码。"
category: 所谓开发
tags: [JS,博客,爱技术]
---

请注意我主页右上角的HighTime功能，昨天在看小伙伴的博客的时候发现的，现在全盘供出源码。

    <li> <a title="HighTime" href='javascript:(function() {
	function c() {
		var e = document.createElement("link");
		e.setAttribute("type", "text/css");
		e.setAttribute("rel", "stylesheet");
		e.setAttribute("href", f);
		e.setAttribute("class", l);
		document.body.appendChild(e)
	}
 
	function h() {
		var e = document.getElementsByClassName(l);
		for (var t = 0; t < e.length; t++) {
			document.body.removeChild(e[t])
		}
	}
 
	function p() {
		var e = document.createElement("div");
		e.setAttribute("class", a);
		document.body.appendChild(e);
		setTimeout(function() {
			document.body.removeChild(e)
		}, 100)
	}
 
	function d(e) {
		return {
			height : e.offsetHeight,
			width : e.offsetWidth
		}
	}
 
	function v(i) {
		var s = d(i);
		return s.height > e && s.height < n && s.width > t && s.width < r
	}
 
	function m(e) {
		var t = e;
		var n = 0;
		while (!!t) {
			n += t.offsetTop;
			t = t.offsetParent
		}
		return n
	}
 
	function g() {
		var e = document.documentElement;
		if (!!window.innerWidth) {
			return window.innerHeight
		} else if (e && !isNaN(e.clientHeight)) {
			return e.clientHeight
		}
		return 0
	}
 
	function y() {
		if (window.pageYOffset) {
			return window.pageYOffset
		}
		return Math.max(document.documentElement.scrollTop, document.body.scrollTop)
	}
 
	function E(e) {
		var t = m(e);
		return t >= w && t <= b + w
	}
 
	function S() {
		var e = document.createElement("audio");
		e.setAttribute("class", l);
		e.src = i;
		e.loop = false;
		e.addEventListener("canplay", function() {
			setTimeout(function() {
				x(k)
			}, 500);
			setTimeout(function() {
				N();
				p();
				for (var e = 0; e < O.length; e++) {
					T(O[e])
				}
			}, 15500)
		}, true);
		e.addEventListener("ended", function() {
			N();
			h()
		}, true);
		e.innerHTML = " <p>If you are reading this, it is because your browser does not support the audio element. We recommend that you get a new browser.</p> <p>";
		document.body.appendChild(e);
		e.play()
	}
 
	function x(e) {
		e.className += " " + s + " " + o
	}
 
	function T(e) {
		e.className += " " + s + " " + u[Math.floor(Math.random() * u.length)]
	}
 
	function N() {
		var e = document.getElementsByClassName(s);
		var t = new RegExp("\\b" + s + "\\b");
		for (var n = 0; n < e.length; ) {
			e[n].className = e[n].className.replace(t, "")
		}
	}
 
	var e = 30;
	var t = 30;
	var n = 350;
	var r = 350;
	var i = "//s3.amazonaws.com/moovweb-marketing/playground/harlem-shake.mp3";
	var s = "mw-harlem_shake_me";
	var o = "im_first";
	var u = ["im_drunk", "im_baked", "im_trippin", "im_blown"];
	var a = "mw-strobe_light";
	var f = "//s3.amazonaws.com/moovweb-marketing/playground/harlem-shake-style.css";
	var l = "mw_added_css";
	var b = g();
	var w = y();
	var C = document.getElementsByTagName("*");
	var k = null;
	for (var L = 0; L < C.length; L++) {
		var A = C[L];
		if (v(A)) {
			if (E(A)) {
				k = A;
				break
			}
		}
	}
	if (A === null) {
		console.warn("Could not find a node of the right size. Please try a different page.");
		return
	}
	c();
	S();
	var O = [];
	for (var L = 0; L < C.length; L++) {
		var A = C[L];
		if (v(A)) {
			O.push(A)
		}
	}
    })()    '>High一下</a> </li>

使用方法很简单，插进相应的HTML标签中即可。懂得JavaScript的朋友还可以另存到一个JS文件再引进来试试，我这里就仅供参考了。

> P.S. 代码从[这里](http://zipperary.com/2013/11/19/give-it-a-high/)来。再推荐下moxie的博客。

<br/>

***

<br/>

爱生活，爱技术。
愿结识各路小伙伴。
找到我：

微博：http://weibo.com/afmz
Github：http://github.com/Ming-Zhe
E-mail：law.gravitys@gmail.com 