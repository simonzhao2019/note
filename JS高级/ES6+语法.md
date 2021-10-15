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

```

