## 一、入口(entry)

入口起点(entry point) 指示 webpack 应该使用哪个模块，来作为构建其内部 依赖图(dependency graph) 的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

默认值是 ./src/index.js，但你可以通过在 webpack configuration 中配置 entry 属性，来指定一个（或多个）不同的入口起点。例如：

```js
module.exports = {
  entry: './path/to/my/entry/file.js',
};

```

## 二、出口(output)

output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。基本上，整个应用程序结构，都会被编译到你指定的输出路径的文件夹中。你可以通过在配置中指定一个 output 字段，来配置这些处理过程：

### （一）path的相关问题

```
let config = {
    devtool: "source-map",
    mode : "development",
    output: {
        path: resolve('../dist'),
        filename: '[name].js',
        chunkFilename: '[name].js',
        publicPath: process.env.NODE_ENV === 'production' ? './' : '/'
    },
}
这里是咪咕音乐的一个出口配置，出口的最低要求为一个对象，这里面重点讲一下publicPath和path的区别
path是webpack所有文件的输出的路径，必须是绝对路径，比如：output输出的js,url-loader解析的图片，HtmlWebpackPlugin生成的html文件，都会存放在以path为基础的目录下。
publicPath 并不会对生成文件的路径造成影响，主要是对你的页面里面引入的资源的路径做对应的补全，常见的就是css文件里面引入的图片。具体的我们可以看看下面，结合上面的  publicPath: process.env.NODE_ENV === 'production' ? './' : '/'来理解。在这里，当是生产环境的时候，采用的是URL地址，但是在开发环境当中，这时的node环境为undefined，也就是说取后面的值。这时候我们html当中的静态资源，就需要取根目录下面的
 <link href="./lib/styles/bootstrap.min.css" rel="stylesheet" />
 <link href="./lib/styles/bootstrapValidator.min.css" rel="stylesheet" />
 目录结构如下
 ![目录结构](C:\Users\PC\Desktop\目录结构.png)
 
 

```

![img](http://img.mukewang.com/57ece136000153f208000570.png)

## 三、插件(plugin)

插件可以帮助我们进行打包优化，资源管理，注入环境变量。想要使用一个插件，你只需要 `require()` 它，然后把它添加到 `plugins` 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 `new` 操作符来创建一个插件实例。

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};
```

1、html-webpack-plugin

HtmlWebpackPlugin 简化了 HTML 文件的创建，以便为你的 webpack 包提供服务。这对于那些文件名中包含哈希值，并且哈希值会随着每次编译而改变的 webpack 包特别有用。你可以让该插件为你生成一个 HTML 文件，使用 lodash 模板提供模板，或者使用你自己的 loader。

```
 new HtmlWebpackPlugin({
    template: './src/index.html',//使用模板index.html
    filename: 'index.html',//打包生成的文件名叫index.html
    chunks:['index']//index.html里引用打包生成的index.js
  }),
```

