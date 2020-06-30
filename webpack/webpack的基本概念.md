## 一、入口

## 二、出口

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