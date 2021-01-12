## 一、tsconfig.json文件

tsconfig.json文件所在的目录表示的是该目录是当前项目的根目录，tsconfig.json文件中指定了用来编译这个项目的根文件和编译选项。

常见的配置如下表

```
{
/*------- 编辑选项 -------*/
  "compilerOptions": {
   
   /* 基本选项 */
    "target": "es5",                       // 指定 ECMAScript 目标版本: 'ES3' (default), 'ES5', 'ES6'/'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
    "module": "commonjs",                  // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
    "lib": [],                             // 指定要包含在编译中的库文件
    "allowJs": true,                       // 允许编译 javascript 文件
    "checkJs": true,                       // 报告 javascript 文件中的错误
    "jsx": "preserve",                     // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
    "declaration": true,                   // 生成相应的 '.d.ts' 文件
    "sourceMap": true,                     // 生成相应的 '.map' 文件
    "outFile": "./",                       // 将输出文件合并为一个文件
    "outDir": "./",                        // 指定输出目录
    "rootDir": "./",                       // 用来控制输出目录结构 --outDir.
    "removeComments": true,                // 删除编译后的所有的注释
    "noEmit": true,                        // 不生成输出文件
    "importHelpers": true,                 // 从 tslib 导入辅助工具函数
    "isolatedModules": true,               // 将每个文件作为单独的模块 （与 'ts.transpileModule' 类似）.

    /* 严格的类型检查选项 */
    "strict": true,                        // 启用所有严格类型检查选项
    "noImplicitAny": true,                 // 在表达式和声明上有隐含的 any类型时报错
    "strictNullChecks": true,              // 启用严格的 null 检查
    "noImplicitThis": true,                // 当 this 表达式值为 any 类型的时候，生成一个错误
    "alwaysStrict": true,                  // 以严格模式检查每个模块，并在每个文件里加入 'use strict'

    /* 额外的检查 */
    "noUnusedLocals": true,                // 有未使用的变量时，抛出错误
    "noUnusedParameters": true,            // 有未使用的参数时，抛出错误
    "noImplicitReturns": true,             // 并不是所有函数里的代码都有返回值时，抛出错误
    "noFallthroughCasesInSwitch": true,    // 报告 switch 语句的 fallthrough 错误。（即，不允许 switch 的 case 语句贯穿）

    /* 模块解析选项 */
    "moduleResolution": "node",            // 选择模块解析策略： 'node' (Node.js) or 'classic' (TypeScript pre-1.6)
    "baseUrl": "./",                       // 用于解析非相对模块名称的基目录
    "paths": {},                           // 模块名到基于 baseUrl 的路径映射的列表
    "rootDirs": [],                        // 根文件夹列表，其组合内容表示项目运行时的结构内容
    "typeRoots": [],                       // 包含类型声明的文件列表
    "types": [],                           // 需要包含的类型声明文件名列表
    "allowSyntheticDefaultImports": true,  // 允许从没有设置默认导出的模块中默认导入。

    /* Source Map Options */
    "sourceRoot": "./",                    // 指定调试器应该找到 TypeScript 文件而不是源文件的位置
    "mapRoot": "./",                       // 指定调试器应该找到映射文件而不是生成文件的位置
    "inlineSourceMap": true,               // 生成单个 soucemaps 文件，而不是将 sourcemaps 生成不同的文件
    "inlineSources": true,                 // 将代码与 sourcemaps 生成到一个文件中，要求同时设置了 --inlineSourceMap 或 --sourceMap 属性

    /* 其他选项 */
    "experimentalDecorators": true,        // 启用装饰器
    "emitDecoratorMetadata": true          // 为装饰器提供元数据的支持
  }
  
  /*------- 指明需要编译的ts文件 -------*/
   "files": [
    "./some/file.ts"
  ],
  
   /*------- 指明需要编译的文件 -------*/
   "include": [
    "./folder"
  ],
  
   /*------- 指明排除的编译文件 -------*/
  "exclude": [
    "./folder/**/*.spec.ts",
    "./folder/someSubFolder"
  ]
}
```

注意：

当命令行上指定了输入文件时，`tsconfig.json`文件会被忽略

```
比如，我们使用命令 tsc hello.ts
这时候指定编译hello文件，则tsconfig.json文件会被忽略
```

## 二、数据类型

### （一）基本数据类型

基本数据类型也就是原始数据类型

主要有布尔值、数值、字符串、`null`、`undefined` 以及 ES6 中的新类型 [`Symbol`](http://es6.ruanyifeng.com/#docs/symbol) 和 [`BigInt`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)。下面我们着重的讲一下Symbol和BigInt。

#### Symbol:

​	Symbol表示的是一个独一无二的值，

```
let s=Symbol()  不能用new Symbol，其和字符串一样，不是引用类型
let obj={}

obj.[s]='你好'
输出："你好"
注意：
(1)Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。
(2)在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。
(3)Symbol 作为属性名，遍历对象的时候，该属性不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。但是，它也不是私有属性，有一个Object.getOwnPropertySymbols()方法，可以获取指定对象的所有 Symbol 属性名。该方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。
```

#### BigInt

BigInt是一种内置对象，它提供了一种方法来表示大于 `253 - 1` 的整数。这原本是 Javascript中可以用Number表示的最大数字。BigInt可以表示任意大的整数。

```
999999999999999999999999999999
输出=》1e+30
99999999999999999999999999999n
输出=》
99999999999999999999999999999n
```

```
ts当中的定义数据类型
let num：number=8 //定义数值类型
let name; string='小明' //定义字符串类型
let flg:boolean=true //定义布尔值类型
let u: undefined = undefined; //定义undefined类型
let n: null = null;//定义null类型

注意：（1）ts还有一个void概念,在 TypeScript 中，可以用 void 表示没有任何返回值的函数：
function alertName(): void {
    alert('My name is Tom');
}
另外，声明一个 void 类型的变量没有什么用，因为你只能将它赋值为 undefined 和 null
（2）关于null和undefined 
与 void 的区别是，undefined 和 null 是所有类型的子类型。比如
// 这样不会报错
let num: number = undefined;
```

### （二）引用数据类型

#### 接口

1、什么是接口

在面向对象语言中，接口（Interfaces）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（classes）去实现（implement）。

```
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
我们可以理解为tom就是对Person的抽象，我们在定义了Person接口之后，又声明了tom属于Person接口，那么其属性必须和Person一样，有两个，为name和age,而且还必须和他们在接口当中声明的类型一样。

```

2、接口的可选属性

诚如上面所述，当定义了一个接口之后。一旦某个变量归属于接口，则其定义的属性必须和接口一样，如果我们需要进行动态的改变，可以使用可选属性

```
interface Person {
    name: string;
    age？: number;
}

let tom: Person = {
    name: 'Tom',
   
};
如上面所示，这时候，age就变为可选的了。不过此时注意，还是不能增加，如果我们给他多了一个属性还是会报错
let tom: Person = {
    name: 'Tom',
   	gender:'male'             //会报错
};  
```

3、任意属性

有时候我们希望一个接口允许有任意的属性，可以使用如下方式：

```
interface Person {
  name: string;
  age?: number;
  [propName: string]: any;
}
let xiaoming:Person={
  name:'xiaoming',
  gender:18

}
这里面，gender就是在接口里面没有定义的类型。
注意：如果我们定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集：
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
这里会报错，因为number并不是sring的子集。

```

一个接口中只能定义一个任意属性。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型：

```
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
```

4、只读属性

```
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};

tom.id = 9527;  //这里不允许进行赋值，因为声明了id为只读属性
```

#### 数组

1、数组的定义

（1）「类型 + 方括号」表示法

```
let fibonacci: number[] = [1, 1, 2, 3, 5];

这里面number表述我们数组的元素必须是number类型，[]表示我们定义的是数组类型，
let fibonacci: any[] = [1, 1, 2, 3, '可以'];
如果有多种类型，里面可以使用any
let fibonacci: number[] = [1, 1, 2, 3, 5];
fibonacci.push('8');

// 报错信息：Argument of type '"8"' is not assignable to parameter of type 'number'.
```

（2）用接口表示数组

```
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];



interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}


可以参考接口的任意属性进行理解
NumberArray 表示：只要索引的类型是数字时，那么值的类型必须是数字。

虽然接口也可以用来描述数组，但是我们一般不会这么做，因为这种方式比前两种方式复杂多了。


```

通常来说，我们会用接口的方式来定义类数组

```js
function (){
let args:number[]=arguments
}


function () {
  let args: any[] = arguments
}   //这种方式定义还是会报下面的错误

/*  
注：arguments 是一个对应于传递给函数的参数的类数组对象。可以参考mdn定义
编译报错：Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 15 more.
这里报错，因为arguments并不是一个数组*/


function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments;
}


这里我们就定义了一个伪数组


function sum() {
    let args: IArguments = arguments;
}

其中IArguments是ts当中定义好了的伪数组类型
```

（3）数组泛型

```js
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

#### 函数

1、函数声明定义函数

```js
function sum(x: number, y: number): number {
    return x + y;
}
/*  
这里我们定义了参数的类型和返回值的类型，
注意：输入多余的（或者少于要求的）参数，是不被允许的：*/
```

2、函数表达式定义函数

```js
let plus: (a: number, b: number) => number = function (a: number, b: number) {
  return a+b
}
/*注意不要混淆了 TypeScript 中的 => 和 ES6 中的 =>。
在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。*/
```

3、用接口定义函数的形状

```js
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
/*采用函数表达式|接口定义函数的方式时，对等号左侧进行类型限制，可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变。*/
```

4、关于函数的tips

(1)可选参数

```js
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');

可选参数的表现形式类似于接口当中的定义用？标识。其中lastName为可选参数，这里要注意可选参数必须写在最后
```

（2）参数默认值

```js
function buildName(firstName: string, lastName: string = 'Cat') {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');

/*注意，这里lastName就有了参数默认值，不过需要注意，ts会把有默认值的参数自动认为为可选参数，还有就是这时候可选参数就不一定非要在后面了*/
```

（3）剩余参数

```
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3)

剩余参数其实就是个数组，我们可以用定义数组的方式去定义它

```

（4）重载

```js
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}这时候的提示为function reverse(x: number | string): number | string



function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
  if (typeof x === 'number') {
    return Number(x.toString().split('').reverse().join(''));
  } else if (typeof x === 'string') {
    return x.split('').reverse().join('');
  }
}
console.log(reverse('hellp'))   //这时候的提示为function reverse(x: string): string (+1 overload)提示了我们当输入为字符串的时候，输出必须为字符串





上例中，我们重复定义了多次函数 reverse，前几次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确的看到前两个提示。

PS:在我理解重载就是提示更加友好
```



#### 任意值

任意值(any)用来表示允许赋值为任意类型

```
eg:
let person:any=123
 person='明天'  //编译的时候并不会报错
 
 注意在ts当中，如果定义一个变量未声明其类型，则就会被认为是任意值类型。
ps:
在任意值上访问任何属性都是允许的：
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
这时候仍然会编译成功    
```

