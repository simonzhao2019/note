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

3、用接口定义函数的形状(接口函数)

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

```js
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

未声明类型的变量，会被识别为任意值类型

```js
eg)
let person
  person='xiaoming'
  person=7
  
//eg)能够正常编译  
```

### (三)类型之间的联系

#### 1、类型推论

```js
//eg)如果没有进行类型定义，ts会按照初次给变量赋值的类型进行推论，下面的代码编译会报错

let name1 = 'xiaoming'
name1=7
```

#### 2、联合类型

eg1)：联合类型定义

联合类型表示的是取值可以为多种类型中的一种，比如我们下面的定义

```js
let age:String|Number=18

age='19'

//我们在上面中就规定了age可以为String或者是Number
```

eg2)访问联合类型的属性或方法

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**：例如下面的例子：

```js
function some(something:String|Number):String {
  return something.length
}
//error TS2339: Property 'length' does not exist on type 'String | Number'.
  Property 'length' does not exist on type 'Number'.
```

#### 3、类型断言

eg1)类型断言（Type Assertion）可以用来手动指定一个值的类型。

eg2)类型断言的用途

1、将一个联合类型断言为其中一个类型

```js
interface Sum{
  name:String,
  run():void
}
interface Plus {
  name: String,
  swim(): void
}

function getName(animal: Sum | Plus) {
  return animal.name;
}

//参考上面的联合类型，并不会报错，但是如果我们按照下面的方式进行访问就会报错interface Cat {


function isFish(animal: Plus | Sum) {
    if (typeof animal.swim === 'function') {
        return true;
    }
    return false;
}

//这里swim并不是共有的，所以会报错
    
    
interface Foo {
  bar: number;
  bas: string;
}

const foo = <Foo>{} ;
foo.bar = 123;
foo.bas = 'hello';//  并不会报错,因为我们在这里断言了foo属于Foo类型
```

## 三、进阶

### 1、泛型

```js
   createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];   //这里决定了数组里面的类型也是为T
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

//在上图中，我们在函数名的后面定义了<T>。这里我们就可以把它理解为泛型，因为我们并不知道输出的类型，其主要的的需求是，要求输入的类型和输出的类型一致 


其中 T 代表 Type，在定义泛型时通常用作第一个类型变量名称。但实际上 T 可以用任何有效名称代替。除了 T 之外，以下是常见泛型变量代表的意思：

K（Key）：表示对象中的键类型；
V（Value）：表示对象中的值类型；
E（Element）：表示元素类型。


function identity<T, M>(value: T, message: M): T {
  console.log(message)
  return value
}
identity<Number,String>(23,'45')
//在上面的代码中，我们定义了identity函数，并且用泛型指明了两个参数的类型，一个是Number,另一个是String.
```

#### （1）泛型接口

```js
interface Identities<V,M>{
  value:V,
  message:M
}

//定义了接口，并且在接口中引入了类型变量 V 和 M
function identity<T, U>(value: T, message: U): Identities<T, U> {
  console.log(value + ": " + typeof (value));
  console.log(message + ": " + typeof (message));
  let identities: Identities<T, U> = {
    value,
    message
  };
  return identities;
}

console.log(identity(68, "Semlinker"));   
/*
输出结果为：
68: number
Semlinker: string
{value: 68, message: "Semlinker"}*/  这里我们就利用接口的性质，返回了两种类型的对象
```

#### （2）泛型类



在类中使用泛型也很简单，我们只需要在类名后面，使用 `<T, ...>` 的语法定义任意多个类型变量

```tsx
interface GenericInterface<U>{
  value:U 
  getIdentity:()=>U
}

class IdentityClass<T> implements GenericInterface<T>{
  value:T
  constructor(value:T){
    this.value=value
  }
  getIdentity():T{
    return this.value
  }
}
const myNumberClass=new IdentityClass<Number>(18)
console.log(myNumberClass.getIdentity())
const myStringClass=  new IdentityClass<String>('Tang Wei')   
/*这里注意一下，如果们已经定义了泛型的类型为String,那么参数就必须为String。
const myStringClass=  new IdentityClass<String>(18)    这种写法会报错Argument of type '18' is not assignable to parameter of type 'String'.

如果我们把泛型约束省略了，那么编译器就会自动推测所使用的泛型
比如，如果我们这样写const myStringClass=  new IdentityClass(18)  这里的泛型约束就会被推测为Number,


*/
console.log(myStringClass.getIdentity())

```



#### （3）泛型约束

###### 3.1 确保属性存在

我们可以利用泛型约束来确保属性的存在，下面的例子就是利用了泛型的特性要求我们传参的时候必须为含有length属性的值

```tsx
interface Lengthwise {
    length: number;
}
//定义了接口lengthwise
function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}

<T extends Lengthwise>  
 //这句话的意思是我们定义的类型必须符合接口Lengthwise的要求   
 loggingIdentity(68)   /*报错 Argument of type '68' is not assignable to parameter of type Lengthwise'. */ 
```

###### 3.2 检查对象上的键是否存在

泛型约束的另一个常见的使用场景就是检查对象上的键是否存在。





```tsx
enum Difficulty{
  Easy,
  Intermediate,
  Hard
}

function getProperty<T,K extends keyof T>(obj:T,key:K):T[K] {
  return obj[key]
}
//K extends keyof T 就是要求K必须输属于T当中的一个
let tsInfo = {
  name: "Typescript",
  supersetOf: "Javascript",
  difficulty: Difficulty.Intermediate
}

let difficulty:Difficulty=getProperty(tsInfo,'difficulty')


//对于keyof,typescript的keyof关键字，将一个类型映射为它所有成员名称的联合类型
```

#### (4)泛型参数默认类型

泛型参数默认类型，类似于函数的默认值，比如

```tsx
interface A<T = string> {
  name: T;
} 
//这里我们默认了，在没有对泛型进行赋值的情况下，类型默认为string
const strA: A = { name: 18 }; //这里会报错，没有指定类型的情况下默认为String,但是赋值确是Number
const strB:A <Number>={name:18} //不会报错
```

泛型参数的默认类型遵循以下原则：

4.1有默认类型的类型参数被认为是可选的。

4.2必选的类型参数不能在可选的类型参数后。

4.3如果类型参数有约束，类型参数的默认类型必须满足这个约束。

4.4当指定类型实参时，你只需要指定必选类型参数的类型实参。 未指定的类型参数会被解析为它们的默认类型。

4.5如果指定了默认类型，且类型推断无法选择一个候选类型，那么将使用默认类型作为推断结果。

4.6一个被现有类或接口合并的类或者接口的声明可以为现有类型参数引入默认类型。

4.7一个被现有类或接口合并的类或者接口的声明可以引入新的类型参数，只要它指定了默认类型。



### 2、元组

元组用来表示已经元素数量和类型得到数组，个元素的类型不必相同的，对应位置的类型需要相同

（1）定义元组

```js
let x: [string, number];
x = ['hello', 10]; // OK
x = [10, 'hello']; // Error


(1)当访问一个已知索引的元素，会得到正确的类型：
console.log(x[0].substr(1)); // OK
console.log(x[1].substr(1)); // Property 'substr' does not exist on type 'number'

(2)元组越界
const list: [string, number] = ['Sherlock', 1887]
list.push('hello world')
console.log(list)      // [ 'Sherlock', 1887, 'hello world' ]
console.log(list[2])    //// Tuple type '[string, number]' of length '2' has no element at index '2'


//上面，我们虽然能够push进去，但是用下标查找时却是查找不到的


```

（2）可选元素类型

元组类型允许在元素类型后缀一个 `?` 来说明元素是可选的：

```js
let list: [number, string?, boolean?]
list = [10, 'Sherlock', true]
list = [10, 'Sherlock']
list = [10]
```

可选元素必须在必选元素的后面，也就是如果一个元素后缀了 ?号，其后的所有元素都要后缀 ?号:

```js
let list1: [number, string?, boolean] 
// Error: A required element cannot follow an optional element
```

### 3、类型别名type

类型别名通常用来给类型起个新的名字，使用场景躲在联合类型当中



```tsx
type StringNum=String|Number


//上面我们定义了联合类型的别名
```

### 4、枚举enum



```tsx
enum test{mon,tus}
console.log(test['tus']) //输出为1

console.log(test[1]) //输出为tus


//枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。
```

