# 全景虚拟漫游实现

全景虚拟漫游其实看到很多例子，比如地图上的全景，校园的全景，之前在朋友圈流行转发的公司全景。但真的想起来要去研究或者实现一下，是前几日说工作上可能会有这样的一个需求。觉悟来得太晚，好奇心也不够重，这么好玩新奇的东西怎么一开始没想到去尝试呢？

由于对这方面的了解是一篇空白的，于是直接在google里搜，发现资料很少，说的稍微全一些的就属《打造H5里的“3D全景漫游”秘籍 》。几种方案中，打算决定采用ThreeJS，因为之前也稍微看过一点，也打算学习，这正好是一个机会。但是看到这篇里面的教程其实说的比较含糊，关键步骤应该都展示出来了，但是没有完整的代码还是不能让我有个很好的概念的。然后，我就愣住了。。。直到我问了一下公司的同事，他告诉我ThreeJS官网有现成的例子！！！ 一下子又被自己的智商惊呆了。。。还是没有养成什么都先去官网看看的习惯。

本文以官网的例子为例，具体分析全景虚拟漫游的原理和实现细节。

## 基本概念
用自己的话来说，ThreeJS是一个3D的JS库，封装了WebGL的功能。而WebGL（Web Graphic Library）又是什么呢，简而言之，就是在浏览器端开发3D图形相关的程序的一个库或者说一个标准。

开发3D程序的过程好比拍照或者录制视频，首先要有场景，然后场景中可能有几个物体，接着把相机放置在某处，并且摆放好位置对准某个地方，这样就能记录下来了。想要在浏览器中显示出来，还需要一个渲染器。至于渲染器到底做了什么或者原理是什么，我们可先不要管。总的来说，相机，场景，渲染器是开启一个3D程序必须的三个要素。三者的关系如下图所示（虽然我对这张图不是特别得满意，但没有找到其他合适的，将就着用）。

![img](../images/three3D/three_flow.jpg)

## 入门例子
直接上个简单例子。
```
//创建场景
var scene = new THREE.Scene();
//创建透视相机
var camera = new THREE.PerspectiveCamera(75, window.innerWidth/window,innerHeight, 1, 1000);
//创建一个渲染器，添加到body元素中
var renderer = new THREE.WebGLRenderer();
document.body.appendChild(renderer.domElement);
//创建一个物体，几何+材质 =》三维物体
var geometry = new THREE.CubeGeometry(1,1,1);
var material = new THREE.MeshBasicMaterial({color: 0x00ff00});
var cube = new THREE.Mesh(geometry, material);
//将物体添加到场景中
scene.add(cube);
 //定义渲染函数    
function render(){
     requestAnimationFrame(render);
     cube.rotation.x += 0.1;
     cube.rotation.y += 0.1;
     renderer.render(scene, camera);
}
//实时渲染
render();
```

这样就创建了一个渲染的立方体的3D程序，是不是挺简单的。

其中需要稍微说一下的是，相机创建的时候带的那些参数。相机分为正交相机和透视相机。透视相机下的世界是真实的效果，有着远小近大的关系。如下图，你很容易看出二者的区别。

![img](../images/three3D/three_projection.png)

透视相机的参数如下
```
PerspectiveCamera( fov, aspect, near, far )
fov — Camera frustum（截面椎体） vertical field of view.
aspect — Camera frustum aspect ratio.
near — Camera frustum near plane.
far — Camera frustum far plane.
```

用张图直观地表示。

![img](../images/three3D/three_camera.png)
