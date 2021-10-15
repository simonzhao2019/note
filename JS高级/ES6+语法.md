### 1、Proxy

**Proxy** 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。

看下面的例子

```js
let target = { x: 10,y: 20};
let hanler = { get: (obj, prop) => 42};
let p=new Proxy(target,hanler)
console.log(target.x)  10
console.log(p.x)   42


target是要使用 Proxy 包装的目标对象（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）。
//注意，这里我们读取p.x能够读取到，是因为这里，p就是target的代理对象，这样我们在处理一些东西的时候能够保护原始对象，无论p的值怎么改变，都不会改变原始对象的值
```

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

