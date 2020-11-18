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

注意：ts还有一个void概念,在 TypeScript 中，可以用 void 表示没有任何返回值的函数：
function alertName(): void {
    alert('My name is Tom');
}
另外，声明一个 void 类型的变量没有什么用，因为你只能将它赋值为 undefined 和 null
```

