title: 基于Node的下一代Web开发框架--koa
date: 2014-01-02 00:26:53
description: "由 Express 原班人马打造的 koa，致力于成为一个更小、更健壮、更富有表现力的 Web 框架。使用 koa 编写 web 应用，通过组合不同的 generator，可以免除重复繁琐的回调函数嵌套，并极大地提升常用错误处理效率。Koa 不在内核方法中绑定任何中间件，它仅仅提供了一个轻量优雅的函数库，使得编写 Web 应用变得得心应手。"
category: 所谓开发
tags: [koa,爱技术,博客,node.js]
---

由 Express 原班人马打造的 koa，致力于成为一个更小、更健壮、更富有表现力的 Web 框架。使用 koa 编写 web 应用，通过组合不同的 generator，可以免除重复繁琐的回调函数嵌套，并极大地提升常用错误处理效率。Koa 不在内核方法中绑定任何中间件，它仅仅提供了一个轻量优雅的函数库，使得编写 Web 应用变得得心应手。

<br/>

## 安装koa

koa 依赖支持 generator 的 Node 环境，也就是说，node的版本要在 `0.11.9` 或者更高，否则将无法执行。

用npm：


	$ npm install koa


或者，选择安装在全局：


	$ npm install -g koa


<br/>

## Example

这是一个koa的简单例子：


	var koa = require('koa');
	var app = koa();

	// logger

	app.use(function *(next){
	  var start = new Date;
	  yield next;
	  var ms = new Date - start;
	  console.log('%s %s - %s', this.method, this.url, ms);
	});

	// response

	app.use(function *(){
	  this.body = 'Hello World';
	});

	app.listen(3000);


与普通的 function 不同，generator functions 以 function* 声明。以这种关键词声明的函数支持 `yield`。在后面会讲到 `yield` 的用法和意义。

<br/>

## 执行koa

执行koa时需要在 `—-harmony` 模式下运行，为了方便可以将 `node` 设置为默认启动 `harmony` 模式的别名：


	alias node='node --harmony'


这样在执行相关js的时候就可以直接使用了。

<br/>

## Cascading

这是一个比较抽象的概念。Koa 中间件以一种非常传统的方式级联起来，也就是这里所谓的Cascading。

在以往的 Node 开发中，频繁使用回调不太便于展示复杂的代码逻辑，在 Koa 中，我们可以写出真正具有表现力的中间件。与 Connect 实现中间件的方法相对比，Koa 的做法不是简单的将控制权依次移交给一个又一个的中间件直到程序结束，Koa 执行代码的方式有点像回形针，用户请求通过中间件，遇到 `yield next` 关键字时，会被传递到下一个符合请求的路由（downstream），在 `yield next` 捕获不到下一个中间件时，逆序返回继续执行代码（upstream）。

下边这个例子展现了使用这一特殊方法书写的 Hello World 范例：一开始，用户的请求通过 x-response-time 中间件和 logging 中间件，这两个中间件记录了一些请求细节，然后「穿过」 response 中间件一次，最终结束请求，返回 「Hello World」。

当程序运行到 `yield next` 时，代码流会暂停执行这个中间件的剩余代码，转而切换到下一个被定义的中间件执行代码，这样切换控制权的方式，被称为 downstream，当没有下一个中间件执行 downstream 的时候，代码将会逆序执行。

	var koa = require('koa');
	var app = koa();

	// x-response-time
	app.use(function *(next){
	  // (1) 进入路由
	  var start = new Date;
	  yield next;
	  // (5) 再次进入 x-response-time 中间件，记录2次通过此中间件「穿越」的时间
	  var ms = new Date - start;
	  this.set('X-Response-Time', ms + 'ms');
	  // (6) 返回 this.body
	});

	// logger
	app.use(function *(next){
	  // (2) 进入 logger 中间件
	  var start = new Date;
	  yield next;
	  // (4) 再次进入 logger 中间件，记录2次通过此中间件「穿越」的时间
	  var ms = new Date - start;
	  console.log('%s %s - %s', this.method, this.url, ms);
	});

	// response
	app.use(function *(){
	  // (3) 进入 response 中间件，没有捕获到下一个符合条件的中间件，传递到 upstream
	  this.body = 'Hello World';
	});

	app.listen(3000);


在上方的范例代码中，中间件以此被执行的顺序已经在注释中标记出来。你也可以自己尝试运行一下这个范例，并打印记录下各个环节的输出与耗时。


	.middleware1 {
	  // (1) do some stuff
	  .middleware2 {
	    // (2) do some other stuff
	    .middleware3 {
	      // (3) NO next yield !
	      // this.body = 'hello world'
	    }
	    // (4) do some other stuff later
	  }
	  // (5) do some stuff lastest and return
	}

上方的伪代码中标注了中间件的执行顺序，看起来是不是有点像 ruby 执行代码块（block）时 yield 的表现了？也许这能帮助你更好的理解 koa 运作的方式。

<br/>

## 中间件（Middleware）

实际上，这些只是初步理解了koa基础，koa有很多第三方开发的中间件，这些中间件的熟练运用才是关键，github有很多这方面的库，每个都有详细的例子解释。以下是常用的一些：

- [koa-router](https://github.com/alexmingoia/koa-router)
- [trie-router](https://github.com/koajs/trie-router)
- [route](https://github.com/koajs/route)
- [basic-auth](https://github.com/koajs/basic-auth)
- [etag](https://github.com/koajs/etag)
- [compose](https://github.com/koajs/compose)
- [static](https://github.com/koajs/static)
- [static-cache](https://github.com/koajs/static-cache)
- [session](https://github.com/koajs/session)
- [compress](https://github.com/koajs/compress)
- [csrf](https://github.com/koajs/csrf)
- [logger](https://github.com/koajs/logger)
- [mount](https://github.com/koajs/mount)
- [send](https://github.com/koajs/send)
- [error](https://github.com/koajs/error)

<br/>

## 小结

这些天确实做了写有关koa的工作，集合开源的力量做了一个 [`中文文档`](http://koajs.cn/) 有更详细的API说明。

在这里要感谢 [`一米`](http://yimity.com/) 对网站建设的大力贡献，还要感谢 [`郭宇`](https://github.com/turingou) 对翻译工作的大力贡献，此中文文档遵循MIT协议，转载著名出处即可。


<br/>

***

<br/>

爱生活，爱技术。
愿结识各路小伙伴。
找到我：

微博：http://weibo.com/afmz
Github：http://github.com/Ming-Zhe
E-mail：law.gravitys@gmail.com 