### 1、node 项目目录结构

通过express-generator生成的node项目目录结构如下

```js
├── app.js  //app.js应用的初始化文件，包括引入应用程序的基础依赖项、设置视图即view的引擎目录以及模板、设置静				态资源路径、配置通用的中间件、引入路由和一些错误处理中间件等


├── bin
│   └── www   //启动文件
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes   //应用的路由文件，这些路由文件中设置的接口最终会以指定的HTTP请求方式暴露给用户，并在用户请求之	├					将结果返回。
│   ├── index.js
│   └── users.js
└── views
    ├── error.pug
    ├── index.pug
    └── layout.pug
```

（1）项目的启动主要是通过bin里面的www脚本文件，这里可以看到package.json文件里面的npm run local脚本看到

```
 "scripts": {
    "local": "set NODE_ENV=local&& node bin/www",  //通过执行bin文件启动node服务
    "test": "set NODE_ENV=test&& node bin/www",
    "bj1": "set NODE_ENV=bj1&& node bin/www",
    "bj2": "set NODE_ENV=bj2&& node bin/www",
    "bj3": "set NODE_ENV=bj3&& node bin/www",
    "bj4": "set NODE_ENV=bj4&& node bin/www",
    "anHui": "set NODE_ENV=anHui&& node bin/www",
    "huNan": "set NODE_ENV=huNan&& node bin/www",
    "huBei": "set NODE_ENV=huBei&& node bin/www",
    "pre": "set NODE_ENV=pre&& node bin/www",
    "dev": "set NODE_ENV=dev&& node bin/www"
  },
```

