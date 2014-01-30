title: First Journey In WebGL（二）
date: 2013-12-09 10:56:39
description: "由于一门课程的需要，要做一个三维可视化的presentation，想来想去，也就WebGL我还有点感兴趣，于是开始对这一领域的研究。这两天的时间也算是小有收获吧。在报告中我就是打算带着大家一起从零写一个WebGL程序的，让所有人也都感受一下这么一个创作的过程，再分享一下学习的经验。不过可能时间不够，就在这写吧。"
category: 所谓开发
tags: [three.js,WebGL,爱技术,博客]
---

承接上次讲述，这次我们要实现一个功能。在程序中引入一个obj文件，在页面中以三维形式展现出来。并且，实现互动式的三维旋转，即随着鼠标的移动，图像在三维立体空间里进行相应的动作。

<br/>

### 引入相关js文件

	<body>
	     <script src="Three.js"></script>
	     <script src="OBJLoader.js"></script>
	     <script src="Detector.js"></script>
	     <script src="RequestAnimationFrame.js"></script>//最大可能的兼容老版本浏览器
	     <script>
	     // your code here
	     </script>
	</body>

所有的js文件在源码中都能找到，在OBJLoader.js在examples/js/loaders/中。

<br/>

### OBJLoader.js

这个是我们主要用的一个js模块，它实现了我们所要的引入obj文件的功能。

	var loader = new THREE.OBJLoader();
	      loader.load( "1.obj", function ( object ) {
	      object.position.y = -30;
	      scene.add( object );
	      } );

代码浅显易懂，基本没什么好说的。

<br/>

### 设定环境光和直接光源

	var ambient = new THREE.AmbientLight( 0x101030 );
	scene.add( ambient );

	var directionalLight = new THREE.DirectionalLight( 0xffeedd );
	directionalLight.position.set( 0, 0, 1 ).normalize();
	scene.add( directionalLight );

three.js提供了多种环境光和光源可供选择。从官网或者github下载原文件后，里面的doc有全部的解释，其实玩javascript是一个体力活，相信你也发现了。

<br/>

### 设置动画效果

	function animate() {

	    requestAnimationFrame( animate );
	    render();

	    }

	function render() {

	    camera.position.x += ( mouseX - camera.position.x ) * .05;
	    camera.position.y += ( - mouseY - camera.position.y ) * .05;
	    camera.lookAt( scene.position );
	    renderer.render( scene, camera );

	    }

requestAnimationFrame()的主要作用是，当此时页面所在浏览器的tap或者window不可见时，就停止渲染，这样更加节省系统资源和电量。在render()中，有两个全局变量mouseX和mouseY，需要添加监听器进行监听，随时改变camera的位置，从而实现动态图像的动态显示。下面来添加监听器。

<br/>

### 添加监听器

	document.addEventListener( 'mousemove', onDocumentMouseMove, false );
	window.addEventListener( 'resize', onWindowResize, false );

相应的响应函数如下：

	function onWindowResize() {

	      windowHalfX = window.innerWidth / 2;
	      windowHalfY = window.innerHeight / 2;

	      camera.aspect = window.innerWidth / window.innerHeight;
	      camera.updateProjectionMatrix();

	      renderer.setSize( window.innerWidth, window.innerHeight );

	}

	function onDocumentMouseMove( event ) {

	      mouseX = ( event.clientX - windowHalfX ) / 2;
	      mouseY = ( event.clientY - windowHalfY ) / 2;

	}

这里还添加了另外一个监听器，是用来相应窗口变化，这也是为了适应多种显示器比例不同，以及在浏览过程中窗口大小变化，物体总是呈现在窗口的正中间。

<br/>

### 最后，全部代码

	<!doctype html>
	<html lang="en">
	    <head>
	        <title>three.js webgl - loaders - OBJ loader</title>
	        <meta charset="utf-8">
	        <meta name="viewport" content="width=device-width, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
	        <style>
	            body {
	                font-family: Monospace;
	                background-color: #000;
	                color: #fff;
	                margin: 0px;
	                overflow: hidden;
	            }
	        </style>
	    </head>
	    <body>
	        <script src="Three.js"></script>
	        <script src="OBJLoader.js"></script>
	        <script src="Detector.js"></script>
	        <script src="RequestAnimationFrame.js"></script>
	        <script>
	            var container;
	            var camera, scene, renderer;
	            var mouseX = 0, mouseY = 0;
	            var windowHalfX = window.innerWidth / 2;
	            var windowHalfY = window.innerHeight / 2;

	            init();
	            animate();

	            function init() {
	                container = document.createElement( 'div' );
	                document.body.appendChild( container );
	                scene = new THREE.Scene();
	                camera = new THREE.PerspectiveCamera( 65, window.innerWidth / window.innerHeight, 1, 2000 );
	                camera.position.z = 100;
	                scene.add( camera );
	                var ambient = new THREE.AmbientLight( 0x101030 );
	                scene.add( ambient );
	                var directionalLight = new THREE.DirectionalLight( 0xffeedd );
	                directionalLight.position.set( 0, 0, 1 ).normalize();
	                scene.add( directionalLight );
	                var loader = new THREE.OBJLoader();
	                loader.load( "1.obj", function ( object ) {
	                    object.position.y = -30;
	                    scene.add( object );
	                } );

	                // RENDERER
	                renderer = new THREE.WebGLRenderer();
	                renderer.setSize( window.innerWidth, window.innerHeight );
	                container.appendChild( renderer.domElement );

	                document.addEventListener( 'mousemove', onDocumentMouseMove, false );
	                window.addEventListener( 'resize', onWindowResize, false );
	            }

	            function onWindowResize() {
	                windowHalfX = window.innerWidth / 2;
	                windowHalfY = window.innerHeight / 2;
	                camera.aspect = window.innerWidth / window.innerHeight;
	                camera.updateProjectionMatrix();
	                renderer.setSize( window.innerWidth, window.innerHeight );
	            }

	            function onDocumentMouseMove( event ) {
	                mouseX = ( event.clientX - windowHalfX ) / 2;
	                mouseY = ( event.clientY - windowHalfY ) / 2;
	            }

	            function animate() {
	                requestAnimationFrame( animate );
	                render();
	            }

	            function render() {
	                camera.position.x += ( mouseX - camera.position.x ) * .05;
	                camera.position.y += ( - mouseY - camera.position.y ) * .05;
	                camera.lookAt( scene.position );
	                renderer.render( scene, camera );
	            }
	        </script>
	    </body>
	</html>

<br/>

## 注意

如果只写一个html文件并把所有的js文件引入进来，依然是无法在浏览器中显示的，这是许多浏览器为了安全性考虑，对于本地的html显示有所限制，具体原因还没有深究，有两种解决方案，一种是改变浏览器的配置，另一种是搭建一个本地服务器(推荐)，这是用python搭建的方法。

	$ python -m SimpleHTTPServer 8080

用node.js：

	(sudo) npm install http-server -g

然后在任意目录下

	http-server [-p] [端口号]

然后访问localhost:8080，会进入到到自己搭建的本地服务器，就可以实现obj的引入了。

<br/>

### 小结

WebGL还是内容还是相当丰富的，javascript更是如此，再多的教程也都只能作为抛砖引玉，真正的老师是谷歌，还有一个强大的资料库github，善用这两个资源，再到几个社区活跃一下，学技术很简单了。。。

<br/>

***

<br/>

爱生活，爱技术。
愿结识各路小伙伴。
找到我：

微博：http://weibo.com/afmz

Github：http://github.com/Ming-Zhe

E-mail：law.gravitys@gmail.com 





