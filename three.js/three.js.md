---
typora-root-url: 图片
---

### 一、three.js几组基础概念

![](https://threejsfundamentals.org/threejs/lessons/resources/images/threejs-structure.svg)

​                                                                                    **three.js应用的整体结构**

```js
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>第一个three.js文件_WebGL三维场景</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      /* 隐藏body窗口区域滚动条 */
    }
  </style>
  <!--引入three.js三维引擎-->
  <script src="http://www.yanhuangxueyuan.com/versions/threejsR92/build/three.js"></script>
  <!-- <script src="./three.js"></script> -->
  <!-- <script src="http://www.yanhuangxueyuan.com/threejs/build/three.js"></script> -->
</head>

<body>
  <script>
    /**
     * 创建场景对象Scene
     */
    var scene = new THREE.Scene()

	 //一个场景(Scene)对象定义了场景图最基本的要素，并包了含背景色和雾等属性。这些对象通过一个层级关系明确的树状结构来展示出各自的位置和方向。子对象的位置和方向总是相对于父对象而言的。比如说汽车的轮子是汽车的子对象，这样移动和定位汽车时就会自动移动轮子。


   /**
     * 创建网格模型
     */
    var geometry = new THREE.BoxGeometry(100, 100, 100); 
             //创建一个立方体几何形状Geometry，这三个参数分别代表boxWidth, boxHeight, boxDepth
    var material = new THREE.MeshLambertMaterial({color: 0x0000ff}); 
			//材质对象Material材质(Material)对象代表绘制几何体的表面属性，包括使用的颜色，和光亮程度。一个材质(Material)可以引用一个或多个纹理(Texture)，这些纹理可以用来，打个比方，将图像包裹到几何体的表面。

    var mesh = new THREE.Mesh(geometry, material); 
            //网格(Mesh)对象可以理解为将几何形状和材质结合之后生成了一个网格对象。在几何对象中，我们通过给出坐标点确定了这个mesh的形状，通过 material确定了绘制时候得到色彩相关属性。


   /**
     * 将mesh添加到场景
    */
		scene.add(mesh); 
    

	/**
     * 光源设置
     */
    //点光源
    var point = new THREE.PointLight(0xffffff);
    point.position.set(400, 200, 300); //点光源位置
    scene.add(point); //点光源添加到场景中
    //环境光
    var ambient = new THREE.AmbientLight(0x444444);
    scene.add(ambient);
  

    /**
     * 相机设置
     */
    var width = window.innerWidth; //窗口宽度
    var height = window.innerHeight; //窗口高度
    var k = width / height; //窗口宽高比
    var s = 200; //三维场景显示范围控制系数，系数越大，显示的范围越大
    //创建相机对象
    var camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
    camera.position.set(200, 300, 200); //设置相机位置
    camera.lookAt(scene.position); //设置相机方向(指向的场景对象)


    /**
     * 创建渲染器对象
     */
    var renderer = new THREE.WebGLRenderer();
    renderer.setSize(width, height);//设置渲染区域尺寸
    renderer.setClearColor(0xb9d3ff, 1); //设置背景颜色
    document.body.appendChild(renderer.domElement); //body元素中插入canvas对象
    


	/**
     * 执行渲染生成
     */
    renderer.render(scene, camera); 	//   指定场景、相机作为参数
  </script>
</body>
</html>
```

### 二、three.js的响应式设置

```js
function main() {
const canvas = document.querySelector('#c');
const renderer = new THREE.WebGLRenderer({canvas});

const fov = 75;
const aspect = 2; // the canvas default
const near = 0.1;
const far = 5;
const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
camera.position.z = 2;

const scene = new THREE.Scene();

{
const color = 0xFFFFFF;
const intensity = 1;
const light = new THREE.DirectionalLight(color, intensity);
light.position.set(-1, 2, 4);
scene.add(light);
}

const boxWidth = 1;
const boxHeight = 1;
const boxDepth = 1;
const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);

function makeInstance(geometry, color, x) {
const material = new THREE.MeshPhongMaterial({color});

const cube = new THREE.Mesh(geometry, material);
scene.add(cube);

cube.position.x = x;

return cube;
}

const cubes = [
makeInstance(geometry, 0x44aa88, 0),
makeInstance(geometry, 0x8844aa, -2),
makeInstance(geometry, 0xaa8844, 2),
];

function render(time) {
time *= 0.001; // convert time to seconds

cubes.forEach((cube, ndx) => {
const speed = 1 + ndx * .1;
const rot = time * speed;
cube.rotation.x = rot;
cube.rotation.y = rot;
});

renderer.render(scene, camera);

requestAnimationFrame(render);
}
requestAnimationFrame(render);

}

main();
```

![ce](https://threejsfundamentals.org/threejs/lessons/resources/images/resize-incorrect-aspect.png)

在没有对webgl进行响应式适配的时候，会出现两个，问题，一个是比例不适配的导致图形被压缩。还有一个就是图形的边缘出现模糊。

1、解决拉伸问题。

通过将传入canvas对象的宽高比，设置为相机的宽高比

```js
  if (resizeRendererToDisplaySize(renderer)) {
        const canvas = renderer.domElement;
        camera.aspect = canvas.clientWidth / canvas.clientHeight;
        camera.updateProjectionMatrix();
      }
```

2、解决边缘模糊问题

模糊问题主要是css像素与物理像素之间的不一致引起的，可以使用像素比进行转换

```js
  function resizeRendererToDisplaySize(renderer) {
      const canvas = renderer.domElement;
      const pixelRatio = window.devicePixelRatio;   //获取设备物理像素，从而进行转换
      const width = canvas.clientWidth * pixelRatio | 0;
      const height = canvas.clientHeight * pixelRatio | 0;
      const needResize = canvas.width !== width || canvas.height !== height;
      if (needResize) {
        renderer.setSize(width, height, false);
      }
      return needResize;
    }
```

### 三、纹理

#### 1、纹理图集

纹理图集是将多个图像放在一个单一的纹理中，然后使用几何体顶点上的纹理坐标来选择在几何体的每个三角形上使用纹理的哪些部分。

#### 2、纹理坐标：

纹理坐标是添加到一块几何体的每个顶点上的数据，用于指定该顶点对应的纹理的哪个部分。

#### 3、加载纹理

（1）简单加载纹理

借用纹理loader加载器进行纹理加载

```js
 const loader = new THREE.TextureLoader()
  //生成纹理加载器
 const texture = loader.load('resources/images/flower-1.jpg');
	//调用加载器的load方法加载纹理

  /*<-------- 注意-------->*/
//需要注意的是，使用这个方法，我们的纹理将是透明的，直到图片被three.js异步加载完成，这时它将用下载的图片更新纹理,这种情况会出现，先是空白，后面再出现纹理的情况


```

（2）同步加载纹理

同步加载纹理就是等纹理都加载完毕之后再进行渲染，可以给load传入第二个参数实现

```js
const loader = new THREE.TextureLoader();
loader.load('resources/images/wall.jpg', (texture) => {
  const material = new THREE.MeshBasicMaterial({
    map: texture,
  });
  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);
  cubes.push(cube);  // 添加到我们要旋转的立方体数组中
});

//给loader传入第二个参数，等加载成功之后，在将纹理设置为材质对象，然后生成几何体
```

上面是对于当个纹理的同步加载，对于多个纹理的同步加载，我们可以采用LoadingManager来进行批量管理

```js
const loadManager=new THREE.LoadingManager()
const loader = new THREE.TextureLoader(loadManager);
 
const materials = [
  new THREE.MeshBasicMaterial({map: loader.load('resources/images/flower-1.jpg')}),
  new THREE.MeshBasicMaterial({map: loader.load('resources/images/flower-2.jpg')}),
  new THREE.MeshBasicMaterial({map: loader.load('resources/images/flower-3.jpg')}),
  new THREE.MeshBasicMaterial({map: loader.load('resources/images/flower-4.jpg')}),
  new THREE.MeshBasicMaterial({map: loader.load('resources/images/flower-5.jpg')}),
  new THREE.MeshBasicMaterial({map: loader.load('resources/images/flower-6.jpg')}),
];
 
loadManager.onLoad = () => {
  const cube = new THREE.Mesh(geometry, materials);
  scene.add(cube);
  cubes.push(cube);  // 添加到我们要旋转的立方体数组中
}; //这里是纹理完全加载完成之后，在绘制立方体以及将立方体添加到scene当中
```

### 四、光照 

#### 1、环境光（ambientLight）

```js
const color = 0xFFFFFF;  //颜色
const intensity = 1; //强度
const light = new THREE.AmbientLight(color, intensity);
scene.add(light);
```

环境光需要两个参数，一个是光的的颜色，还有一个是光照强度.

环境光，没有方向，无法产生阴影，场景内任何一点受到的光照强度都是相同的，除了改变场景内所有物体的颜色以外，不会使物体产生明暗的变化，看起来并不像真正意义上的光照。通常的作用是提亮场景，让暗部不要太暗。

#### 2、半球光（hemisphereLight）

半球光（HemisphereLight）的颜色是从天空到地面两个颜色之间的渐变，与物体材质的颜色作叠加后得到最终的颜色效果。一个点受到的光照颜色是由所在平面的朝向（法向量）决定的 —— 面向正上方就受到天空的光照颜色，面向正下方就受到地面的光照颜色，其他角度则是两个颜色渐变区间的颜色。

```js
const skyColor = 0xB1E1FF;  // light blue
const groundColor = 0xB97A20;  // brownish orange
const intensity = 1;
const light = new THREE.HemisphereLight(skyColor, groundColor, intensity);
scene.add(light);

```

半球光有三个参数天空颜色、地面颜色，还有就是光照强度。

单纯的半球光也很难使物体有立体感 ，但是可以很好地表现天空和地面颜色照射到物体上时的效果。所以最好的使用场景就是与其他光照结合使用，或者作为环境光（AmbientLight）的一种替代方案。

#### 3、方向光

方向光表示的是来自一个方向上的光，并不是从某个点发射出来的，而是从一个无限大的平面内，发射出全部相互平行的光线，人们通常用方向光来表示太阳光。

```js
 const color = 0xFFFFFF;
      // const skyColor=0xB1E1FF
      // const groundColor= 0xB97A20
      const intensity=1
      const light=new THREE.DirectionalLight(color,intensity)  //方向光
      light.position.set(0, 10, 0);
      light.target.position.set(-5, 0, 0)
      scene.add(light)
      scene.add(light.target)
```

#### 4、点光源

点光源（PointLight）表示的是从一个点朝各个方向发射出光线的一种光照效果。

```js
const color = 0xFFFFFF;
const intensity = 1;
const light = new THREE.PointLight(color, intensity);
light.position.set(0, 10, 0);
scene.add(light);
```

点光源（PointLight）有额外的一个范围（distance）属性。 如果 distance 设为 0，则光线可以照射到无限远处。如果大于 0，则只可以照射到指定的范围，光照强度在这个过程中逐渐衰减，在光源位置时，intensity 是设定的大小，在距离光源 distance 位置的时候，intensity 为 0。

#### 5、聚光灯

聚光灯可以看成是一个点光源被一个圆锥体限制住了光照的范围。实际上有两个圆锥，内圆锥和外圆锥。光照强度在两个锥体之间从设定的强度递减到 0

```js
const intensity = 1;
    const light = new THREE.SpotLight(color, intensity);
    light.position.set(0, 10, 0);
    light.target.position.set(-5, 0, 0);
    scene.add(light);
    scene.add(light.target);
```

#### 6、矩形区域光

矩形区域光（RectAreaLight）, 顾名思义，表示一个矩形区域的发射出来的光照，例如长条的日光灯或者天花板上磨砂玻璃透进来的自然光。另外，我们需要注意矩形区域光（RectAreaLight）只能影响 MeshStandardMaterial 和 MeshPhysicalMaterial，其它如MeshPhongMaterail是无效的。

```js
 const color = 0xFFFFFF;
    const intensity = 5;
    const width = 12;
    const height = 4;
    const light = new THREE.RectAreaLight(color, intensity, width, height);
    light.position.set(0, 10, 0);
    light.rotation.x = THREE.MathUtils.degToRad(-90);
    scene.add(light);


//与方向光（DirectionalLight）和聚光灯（SpotLight）不同，矩形光不是使用目标点（target），而是使用自身的旋转角度来确定光照方向。另外，矩形光的辅助对象（RectAreaLightHelper）应该添加为光照的子节点，而不是添加为场景的子节点。
```

#### 7、PhysicallyCorrectLights

PhysicallyCorrectLights是renderer当中的第一个设置项。这个设置会影响（随着离光源的距离增加）光照如何减弱。这个设置会影响点光源（PointLight）和聚光灯（SpotLight），矩形区域光（RectAreaLight）会自动应用这个特性。



在设置光照时，基本思路是不要设置 distance 来表现光照的衰减，也不要设置 intensity。而是设置光照的 power 属性，以流明为单位，three.js 会进行物理计算，从而表现出接近真实的光照效果。在这种情况下 three.js 参与计算的长度单位是米，一个 60瓦 的灯泡大概是 800 流明强度。并且光源有一个 decay 属性，为了模拟真实效果，应该被设置为 2。

#### 8、几种光的区别

![](/光源.png)

### 五、相机

### 六、自定义几何体

#### 1、bufferGeometry与BufferAttribute和attributes

BufferGeometry 是three.js 表示所有几何的方式。 BufferGeometry 本质上是一个名为 BufferAttributes 的集合。每个 BufferAttribute 表示一种数据类型的数组：位置、法线、颜色、uv 等...一起，命名的 BufferAttributes 表示每个顶点的所有数据的并行数组。我们用 BufferAttribute来存储与BufferGeometry相关联的 attribute（例如顶点位置向量，面片索引，法向量，颜色值，UV坐标以及任何自定义 attribute ）。 利用 BufferAttribute，可以更高效的向GPU传递数据。具体我们可以参考官网的例子。

```js
const geometry = new THREE.BufferGeometry();
// 创建一个简单的矩形. 在这里我们左上和右下顶点被复制了两次。
// 因为在两个三角面片里，这两个顶点都需要被用到。
const vertices = new Float32Array( [
	-1.0, -1.0,  1.0,
	 1.0, -1.0,  1.0,
	 1.0,  1.0,  1.0,

	 1.0,  1.0,  1.0,
	-1.0,  1.0,  1.0,
	-1.0, -1.0,  1.0
] );

// itemSize = 3 因为每个顶点都是一个三元组。
geometry.setAttribute( 'position', new THREE.BufferAttribute( vertices, 3 ) );
const material = new THREE.MeshBasicMaterial( { color: 0xff0000 } );
const mesh = new THREE.Mesh( geometry, material );


```

这里我们还需要了解一下attributes。attributes存储着我们通过setAttribute设置的所有buffer数据。就像官网所说attributes通过 hashmap 存储该几何体相关的属性，hashmap 的 id 是当前 attribute 的名称，值是相应的 buffer。 你可以通过 .setAttribute 和 .getAttribute 添加和访问与当前几何体有关的 attribute。

*HashMap*是一个用于存储Key-Value键值对的集合。简单来说就是个Object