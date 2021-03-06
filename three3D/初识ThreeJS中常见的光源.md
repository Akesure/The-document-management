# 初识ThreeJS中常见的光源

现实环境中，人们之所以能看得到物体，是因为有光，物体的材质反射光到人眼中。在ThreeJS中有几种光源，去模拟现实环境。最常见的四种为：

- 环境光( AmbientLight )：笼罩在整个空间无处不在的光
- 点光源( PointLight )：向四面八方发射的单点光源
- 聚光灯( SpotLight )：发射出锥形状的光， 模拟手电筒，台灯等光源
- 平行光( DirectinalLight )：平行的一束光，模拟从很远处照射的太阳光

还有半球光光源( HemisphereLight )，平面光光源( AreaLight )，这里暂不做介绍。下面以一只高跟鞋为例来演示这几种光源。

![img](../images/three3D/shoes_1.png)

图1 白色环境光 + 白色平行光的鞋子效果

P.S. 此处的坐标轴是为了辅助创建光源，更好地放置光源的位置。其中红色线表示X轴，绿色线表示Y轴，蓝色线表示Z轴，很好记，红绿蓝，RGB-XYZ。有时候为了搞错，可以用右手法则去验证一下。代码也非常简单。

```
var axis = new THREE.AxisHelper(3); // 3表示轴的长度
sscene.add(axis);
```

## 环境光(AmbientLight)

环境光可以说是场景的整体基调。如何创建环境光呢？代码如下。

```
var ambient = new THREE.AmbientLight(0xffffff);
scene.add(ambient); //将环境光添加到场景中
```

API也很简单，只有color和intensity参数。

```
AmbientLight( color, intensity )

color — 光的颜色值，十六进制，默认值为0xffffff.
intensity — 光的强度，默认值为1.
```

图1示例中的环境光为白光。我们将颜色设置为红色，看看什么效果。结果是整只鞋子都变成了红色。

![img](../images/three3D/shoes_2.png)

图2 红色环境光 + 白色平行光的鞋子效果

需要注意的是，由于环境光无处不在，也就是说它是没有方向的，当然不能产生阴影。而且，它也不能作为环境中唯一的光源。我们来看一下只有环境光的效果。

![img](../images/three3D/shoes_3.png)

图3 仅有红色环境光的鞋子效果

![img](../images/three3D/shoes_4.png)

图4 仅有白色环境光光的鞋子效果

显然，只有环境光的场景是不真实的。那环境光什么作用呢？据说是弱化阴影或者给场景添加一些颜色。

## 点光源

可以将点光源想象成萤火虫一样发出的光。由于它的光线也发射到四面八方，在ThreeJS中它也是不能产生阴影的。创建的方法如下：

```
PointLight( color, intensity, distance, decay )

color — 光的颜色值，十六进制，默认值为0xffffff.
intensity — 光的强度，默认值为1.   
distance — 光照距离，默认为0，表示无穷远都能照到.
decay — 随着光的距离，强度衰减的程度，默认为1，为模拟真实效果，建议设置为2
```

我们给最初的那个场景图4创建一个坐标为(3, 3, 3)，强度为0.2，照射距离为100的点光源。我们分别将光照强度改为1，颜色调成红色，光照距离调成1，进行对比。

```
var pointLight = new THREE.PointLight(0xffffff, 0.2, 100);
pointLight.position.set(3,3,3);
scene.add(pointLight);
```

![img](../images/three3D/shoes_5.png)

图5 白色环境光 + 强度为0.2，距离为100的白色点光源的鞋子效果

```
var pointLight = new THREE.PointLight(0xffffff, 1, 100);
```

![img](../images/three3D/shoes_6.png)

图6 白色环境光 + 强度为1，距离为100的白色点光源的鞋子效果

```
var pointLight = new THREE.PointLight(0xffffff, 1, 10);
```

![img](../images/three3D/shoes_7.png)

图7 白色环境光 + 强度为1，距离为10的白色点光源的鞋子效果

```
var pointLight = new THREE.PointLight(0xff0000, 1, 100);
```

![img](../images/three3D/shoes_8.png)

图8 白色环境光 + 强度为1，距离为100的红色点光源的鞋子效果

## 聚光灯( SpotLight )

聚光灯是比较常见的光源，特别是当你想产生阴影的时候。聚光灯的属性不同版本的threeJS改动还有点大。以官网最新的版本8.3为准。

```
SpotLight( color, intensity, distance, angle, penumbra, decay )

color — 光的颜色值，十六进制，默认值为0xffffff.
intensity — 光的强度，默认值为1.   

distance — 光照距离，默认为0，表示无穷远都能照到.
angle —  圆椎体的半顶角角度，最大不超过90度，默认为最大值。
penumbra — 光照边缘的模糊化程度，范围0-1，默认为0，不模糊
decay — 随着光的距离，强度衰减的程度，默认为1，为模拟真实效果，建议设置为2
```

我们给图4的场景添加位置为( 0, 4, 0 )，强度为1，距离为100的白光聚光灯。对于颜色，距离，强度的效果已经在点光源处实践过。我们来看下角度angle，边缘模糊程度penumbra，衰减decay的效果。代码如下。

```
var spotLight = new THREE.SpotLight( 0xFFFFFF, 1, 100);
spotLight.position.set(0,4,0);  
scene.add(spotLight);
```

![img](../images/three3D/shoes_9.png)

图9 白色环境光 + 强度为1，距离为100的白色聚光灯的鞋子效果

```
var spotLight = new THREE.SpotLight( 0xFFFFFF, 1, 100, 0.06);
```

![img](../images/three3D/shoes_10.png)

图10 白色环境光 + 强度为1，距离为100，角度为0.06（弧度）的白色聚光灯的鞋子效果

```
var spotLight = new THREE.SpotLight( 0xFFFFFF, 1, 100, 0.06, 0.5);
```

![img](../images/three3D/shoes_11.png)

图11 白色环境光 + 强度为1，距离为100，角度为0.06（弧度），边缘模糊为0.5的白色聚光灯的鞋子效果

```
var spotLight = new THREE.SpotLight( 0xFFFFFF, 1, 100, 0.06, 0.5, 10);
```

![img](../images/three3D/shoes_12.png)

图12 白色环境光 + 强度为1，距离为100，角度为0.06（弧度），边缘模糊为0.5，衰减为10的白色聚光灯的鞋子效果

附带说一句，聚光灯还可以限制所能产生阴影的区域，但本文不涉及具体的阴影设置。

## 平行光( DirectionalLight )

平行光或者说方向光可以看成是另类的聚光灯，距离太远以至于光线基本平行了，就像太阳对于我们来说一样。它与聚光灯不同的一点是，它在任何地方的强度都是一样的。当然它也是可以产生阴影的。创建平行光的接口与环境光一致。

```
DirectionalLight( color, intensity )

color — 光的颜色值，十六进制，默认值为0xffffff.
intensity — 光的强度，默认值为1.  
```

同样就创建一个坐标为( 0, 4, 0)，强度为1的白色平行光。效果如下。

![img](../images/three3D/shoes_13.png)

图13 白色环境光 + 强度为1，距离为100的白色平行灯的鞋子效果

## 总结

我们现在来看一下，相同环境光，相同参数的点光源，聚光灯，平行光之间的效果差别。仔细地观察俯视图的鞋尖部分，发现平行光 > 聚光灯 > 点光源，这其实也好理解。我觉得关键差别在于平行光和聚光灯能产生阴影吧。还有聚光灯能做出舞台的效果。至于具体需用到什么光源，怎么去布置光源，需要根据具体场景来定。我并没有多少实战经验，也不具体说了。

![img](../images/three3D/shoes_14.png)

图14 点光源，聚光灯，平行光之间的效果（坐标均为(0,4,0)）

![img](../images/three3D/shoes_15.png)

图15 点光源，聚光灯，平行光之间的效果（坐标均为(3,3,3)）

[github](https://wufenfen.github.io/threeJS_Demo/shoe.html)
