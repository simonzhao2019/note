# 函数

#### 1、执行上下文+执行上下文对象

执行上下文是js代码在执行之前首先会创建执行上下文环境，在创建执行上下文的时候，会做以下的事情：

1、创建变量对象，这个变量对象不仅保存参数的声明还会保存函数的声明。

2、确定this的指向

3、生成作用域链

#### 2、闭包

当产生函数嵌套时候，内部函数保存的一个指向外部函数变量对象的指针（按理是整个变量对象），但是在浏览器环境中，浏览器会进行优化，他指向的这个变量对象只会保存内部函数要用到的变量。

#### 3、函数的call调用和apply调用

在 javascript 中，call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 this 的指向。

```
function Fruit(params) {
    
  }
  Fruit.prototype = {
  color: "red",
  say: function() {
  console.log("My color is " + this.color);
  }
  }
  let apple=new Fruit()
  let banner={
    color:"yellow"
  }
  apple.say()  //输出结果 My color is red
  
  apple.say.call(banner)  //输出结果 My color is yellow
  在上面的例子当中我们就改变了函数调用时候this的指向
```

call与apply之间的区别

```
对于 apply、call 二者而言，作用完全一样，只是接受参数的方式不太一样。例如，有一个函数定义如下：

var func = function(arg1, arg2) {
     
};
就可以通过如下方式来调用：
func.call(this, arg1, arg2);
func.apply(this, [arg1, arg2])

一个传入的是数组，一个是依次传入
```

