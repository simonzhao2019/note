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

### 2、app.js

这里主要讲一下app.js里面比较重要的部分

```js


// view engine setup，这里可以参考express的官方文档，简单解释来说，app.set('view')指定了网页视图渲染的模板,可以参考下面给出的解释


app.set('views', path.join(__dirname, 'views'));

/*A directory or an array of directories for the application's views. If an array, the views are looked up in the order they occur in the array.*/

app.engine('html', require("ejs").renderFile);

/*By default, Express will require() the engine based on the file extension. For example, if you try to render a “foo.pug” file, Express invokes the following internally, and caches the require() on subsequent calls to increase performance.*/

app.set('view engine', 'html');
/*The default engine extension to use when omitted.
NOTE: Sub-apps will inherit the value of this setting.*/



// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: false}));
app.use(cookieParser());
app.use(session({
    resave: true, // don't save session if unmodified
    saveUninitialized: false, // don't create session until something stored
    secret: 'love'
}));
app.use(express.static(path.join(__dirname, 'public')));

// // 登录校验
app.use(function (req, res, next) {
    var path = req.path;
    if (path != "/login" && path != "/getMessageVerify" && !req.session.user&&path != "/getMsgCode") {
        return res.redirect("login");
    }
    next();
});


app.use('/', index);
app.use('/users', users);
//对于app.use的解释
Mounts the specified middleware function or functions at the specified path: the middleware function is executed when the base of the requested path matches path.



// catch 404 and forward to error handler
app.use(function (req, res, next) {
    var err = new Error('Not Found');
    err.status = 404;
    next(err);
});
//注意，这里的404是我们自己扔出来的，具体可以看express官网的解释。在Express 中，404 回应并不是错误的结果，因此错误处理常式中介软体不会撷取它们。会有此行为是因为404 回应仅表示没有额外的工作来执行；换句话说，Express 已执行所有的中介软体函数和路由，并发现它们都没有回应。您唯一要做的是在堆叠的最底端（其他所有函数下方），新增一个中介软体函数，以处理404 回应。简单的说，前面的路由什么的我们都没有命中，都没有return，一路下来我们没有任何处理结果，这时候，我们就可以扔出404,让下一步的函数处理。

// error handler
app.use(function (err, req, res, next) {
    // set locals, only providing error in development
    res.locals.message = err.message;
    res.locals.error = req.app.get('env') === 'development' ? err : {};

    // render the error page
    res.status(err.status || 500);
    res.render('error');
});
process.on('uncaughtException', function(err) {
    console.error('Error caught in uncaughtException event:', err);
});

module.exports = app;

```

