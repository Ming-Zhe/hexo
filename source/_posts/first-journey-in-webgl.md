title: First Journey In WebGL（一）
date: 2013-12-06 21:49:53
description: "由于一门课程的需要，要做一个三维可视化的presentation，想来想去，也就WebGL我还有点感兴趣，于是开始对这一领域的研究。这两天的时间也算是小有收获吧。在报告中我就是打算带着大家一起从零写一个WebGL程序的，让所有人也都感受一下这么一个创作的过程，再分享一下学习的经验。不过可能时间不够，就在这写吧。"
category: 所谓开发
tags: [three.js,WebGL,爱技术,博客]
---

由于一门课程的需要，要做一个三维可视化的presentation，想来想去，也就WebGL我还有点感兴趣，于是开始对这一领域的研究。这两天的时间也算是小有收获吧。在报告中我就是打算带着大家一起从零写一个WebGL程序的，让所有人也都感受一下这么一个创作的过程，再分享一下学习的经验。不过可能时间不够，就在这写吧。

## three.js

要说玩WebGL怎么可能不说three.js，这可是对WebGL的完美封装。[源码]()和[官网]()，要学three.js这两个地方是必去之处。我简单说一下three.js起步。

### 在进行开发之前

由于我们最终的结果是要呈现在web端，所有在在前端的展示框架要先搭建好，同时还要事先下载three.js文件，或者就像我这里一样，用three.js的官方cdn。


	<html>
		<head>
			<title>My first Three.js app</title>
			<style>canvas { width: 100%; height: 100% }</style>
		</head>
		<body>
			<script src="https://rawgithub.com/mrdoob/three.js/master/build/three.js"></script>
			<script>
				// Our Javascript will go here.
			</script>
		</body>
	</html>

### 创建场景(scene)

用three.js呈现的内容需要3样元算：scene, camera, renderer	. 

```
	var scene = new THREE.Scene();
	var camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );

	var renderer = new THREE.WebGLRenderer();
	renderer.setSize( window.innerWidth, window.innerHeight );
	document.body.appendChild( renderer.domElement );
```
解释一下，这里的camera，我们使用的是PerspectiveCamera(three.js还提供了很多其他类型的camera以供选择)，它有4个参数，第一个表示视野大小；第二个表示成像比例，一般都是按照浏览器窗口比例来进行设定；第三和第四个分别是最远和最近的camera作用范围。下面的renderer比较容易理解，需要注意的是在最后要append到document中。

下面创建一个物体出来。

```
var geometry = new THREE.CubeGeometry(1,1,1);
var material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
var cube = new THREE.Mesh( geometry, material );
scene.add( cube );

camera.position.z = 5;
```
这里CubeGeometry为了创建一个cube出来，由geometry变量存储。除此之外还有需要material给我们的cube着色(同样的three.js由很多material可供选择)。然后就是mesh了，这个过程可以抽象的理解为融合过程，有了geometry和material融合后就是我们需要的最终object了。最后把物体add进之前声明好的scene中去。

### 渲染(render)

```
function render() {
	requestAnimationFrame(render);
	renderer.render(scene, camera);
}
render();
```
如果要深入解释render的原理，这就是另外一本书的内容了。。。这里，浏览器接到指令后，大概会做的事情就是每秒对要显示的内容进行60次着色。

### 制作动画效果

如果渲染出的图像不会动，可能也看不出三维效果来。因为我们用的只是一个生成的简单cube，本身是没有办法呈现出立体感的。所以我们加点动态以展示出，我们做的确实一个三维的object。

```
cube.rotation.x += 0.1;
cube.rotation.y += 0.1;
```
### 最终代码展示

```
<html>
	<head>
		<title>My first Three.js app</title>
		<style>canvas { width: 100%; height: 100% }</style>
	</head>
	<body>
		<script src="three.min.js"></script>
		<script>
			var scene = new THREE.Scene();
			var camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);

			var renderer = new THREE.WebGLRenderer();
			renderer.setSize(window.innerWidth, window.innerHeight);
			document.body.appendChild(renderer.domElement);

			var geometry = new THREE.CubeGeometry(1,1,1);
			var material = new THREE.MeshBasicMaterial({color: 0x00ff00});
			var cube = new THREE.Mesh(geometry, material);
			scene.add(cube);

			camera.position.z = 5;

			var render = function () {
				requestAnimationFrame(render);

				cube.rotation.x += 0.1;
				cube.rotation.y += 0.1;

				renderer.render(scene, camera);
			};

			render();
		</script>
	</body>
</html>
```

这次先写这么多，下次介绍怎么再一个WebGL程序中load一个外部obj模型。哈哈，给力的实例来了。。。

<br/>

***

<br/>

爱生活，爱技术。
愿结识各路小伙伴。
找到我：

微博：http://weibo.com/afmz
Github：http://github.com/Ming-Zhe
E-mail：law.gravitys@gmail.com 















