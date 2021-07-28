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

