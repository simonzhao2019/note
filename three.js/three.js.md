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

