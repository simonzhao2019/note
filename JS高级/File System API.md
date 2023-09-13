### 一、window.showOpenFilePicker(opt)

接口的 **`showOpenFilePicker()`** 方法用于显示一个文件选择器，以允许用户选择一个或多个文件并返回这些文件的句柄（http不支持，必须是在https域名下面）

看下面的例子

```js
<script setup lang="ts">
const openFile = async () => {
  const pickerOpts = {
    // types: [
    //   {
    //     description: "Images",
    //     accept: {
    //       "image/*": [".png", ".gif", ".jpeg", ".jpg"],
    //     },
    //   },
    // ],
    excludeAcceptAllOption: true,
    multiple: false,
  };
  const res = await window.showOpenFilePicker();
  console.log(res);
};
</script>

<template>
  <div class="open_file" @click="openFile">打开文件</div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>

```

`options` 可选

选项对象，包含以下属性：

- `multiple`

  布尔值，默认为 `false`。设为 `true` 时允许用户选择多个文件。

- `excludeAcceptAllOption`

  布尔值，默认为 `false`。默认情况下，文件选择器带有一个允许用户选择所有类型文件的过滤选项（展开于文件类型选项中）。设置此选项为 `true` 以使该过滤选项不可用。简单来说就是允不允许你选择所有文件。

- `types`

  表示允许选择的文件类型的 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array) 数组，其中的元素为包含以下选项的对象：`description`可选，对此允许文件类型集合的描述。`accept`[`Object`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object) 对象，带有键名为 [MIME 类型](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types)、键值为包含文件扩展名的 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array) 数组的键值对（参考下方的示例）。





### 二、数组相关

#### 1、定型数组（typed array）

定型数组指的是一种特殊的包含数值类型的数组。了解定型数组之前我们先了解两个与定型数组有关的概念

**1.1  ArrayBuffer**

ArrayBuffer 对象用来表示通用的、固定长度的原始二进制数据缓冲区。ArrayBuffer（）构造函数用来在内存中分配特定数量的字节空间。

```js
 const arrayBuffer=new ArrayBuffer(16)  //在内存中分配16字节
 console.log('arrayBuffer',arrayBuffer.byteLength)//16


ArrayBuffer一经创建就不能再调整大小。不过，可以使用 slice()复制其全部或部分到一个新实例中：
例中：
const buf1 = new ArrayBuffer(16)
const buf2 = buf1.slice(4, 12);
alert(buf2.byteLength); // 8
```

不能仅通过对 ArrayBuffer 的引用就读取或写入其内容。要读取或写入 ArrayBuffer，就必须通过视图。视图有不同的类型，但引用的都是 ArrayBuffer 中存储的二进制数据。  



**1.2 DataView**

第一种允许你读写 ArrayBuffer 的视图是 DataView。这个视图专为文件 I/O 和网络 I/O 设计，其
API 支持对缓冲数据的高度控制，但相比于其他类型的视图性能也差一些。 DataView 对缓冲内容没有
任何预设，也不能迭代。

```js
/* 默认情况下DataView使用整个ArrayBuffer */ 
 
const arrayBuffer=new ArrayBuffer(16)
 const fullDataView=new DataView(arrayBuffer)
 console.log('byteOffset',fullDataView.byteOffset) //0
 console.log('buffer',fullDataView.buffer===arrayBuffer) //true
 console.log('byteLength',fullDataView.byteLength) //16
/* DataView构造函数还可以接受一个字节偏移量和字节长度 */ 

 const firstHalfDataView = new DataView(arrayBuffer, 0, 8);
 console.log(firstHalfDataView.byteOffset); // 0
 console.log(firstHalfDataView.byteLength); // 8
console.log(firstHalfDataView.buffer === arrayBuffer); // true


```

**字节和比特**

  位（bit）：也被称为比特，二进制数中的一个数位，可以是0或者1，是计算机中数据的最小单位。二进制的一个“0”或一个“1”叫一位。

字节（Byte，B）：计算机中数据的基本单位，每8位组成一个字节。各种信息在计算机中存储、处理至少需要一个字节。

**ElementType**

DataView 对存储在缓冲内的数据类型没有预设。它暴露的 API 强制开发者在读、写时指定一个ElementType，然后 DataView 就会忠实地为读、写而完成相应的转换。

![微信截图_20211015171350](ES6+语法.assets/微信截图_20211015171350-16342894474302.png)



DataView 为上表中的每种类型都暴露了 get 和 set 方法，这些方法使用 byteOffset（字节偏移量）定位要读取或写入值的位置。类型是可以互换使用的，如下例所示：

```js

const arrayBuffer=new ArrayBuffer(2)

const view=new DataView(arrayBuffer)
console.log(view.getInt8(0))  //0
console.log(view.getInt8(1))   //0
console.log(view.getInt16(0))   //0
console.log(view.getInt16(1)) //Uncaught RangeError: Offset is outside the bounds of the DataView



//在上面，2个字节，对应的是8比特是两个，对应的16比特只有1个，因此我们打印view.getInt16(1)的时候回报错，因为两个字节我们用1个16位就能表示，不存在第二个16位
```

#####   补充：进制转换



比如0X80为16进制，则转换二进制遵循以下方法

因为：2<sup>4</sup>=16 所以每个位数对应的是4位

对应的四位数为：8、4、2、1 ，之所以是8、4、2、1是因为8+4+2+1=15。8、4、2、1也就是2<sup>3</sup>=8、2<sup>2</sup>=4、2<sup>1</sup>=2、2<sup>0</sup>=1



二进制与十进制之间的转换

找出最近的二的次幂，依次叠加

比如34转换成二机制，离34最近的为2<sup>5</sup>=32   

### 三、Class（类）

#### 1、类的概念

上面代码定义了一个“类”，可以看到里面有一个`constructor()`方法，这就是构造方法，而`this`关键字则代表实例对象。ES6当中的类可以把它看作是ES5当中的语法糖。是对new这种构造函数的简化。

```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

#### 2、constructor 方法

```javascript
import axios from 'axios'
import type { AxiosInstance, AxiosRequestConfig } from 'axios'

class Request {
  // axios 实例
  instance: AxiosInstance

  constructor(config: AxiosRequestConfig) {
    this.instance = axios.create(config)
  }
  request(config: AxiosRequestConfig) {
    return this.instance.request(config)
  }
}

export default Request

//在上文当中，我们创建了一个request的类，其中有有个constructor方法，对于所有的class都必然有个constructor方法，在对class进行实例化的时候，第一步就是通过constructor方法生成实例。比如，new Request（）的时候，第一步就是调用constructor方法生成实例。

//注意：在我们定义类的时候，所有类如果我们没有给他添加construtor方法的话，都会被默认添加一个constructor方法

class Point {
}

// 等同于
class Point {
  constructor() {}
}

上图中pointer被默认添加了constructor方法。
（1）constructor()方法默认返回实例对象（即this），完全可以指定返回另外一个对象。

（2）类必须使用new调用，否则会报错


class Foo {
  constructor() {
    return Object.create(null);
  }
}

Foo()
```

#### 3、类的实例



（1）用new操作符是调用函数的的时候返回的是类的实例。

（2）类的实例除非显式定义在其本身（即定义在`this`对象上），否则都是定义在原型上（即定义在`class`上）。

```js
//定义类
class Point {

  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }

}

//在我们上面定义的Point类中，除了x、y是在实例当中的，toString会被定义在原型上面
var point = new Point(2, 3);

point.toString() // (2, 3)

point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
```

#### 4、getter和setter

```js
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
```



```javascript

```

