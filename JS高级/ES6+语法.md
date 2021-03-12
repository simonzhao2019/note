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

