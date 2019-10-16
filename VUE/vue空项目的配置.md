## 空项目文件夹概况：

​     

```
         |-- build : webpack相关的配置文件夹(基本不需要修改)

​             |-- dev-server.js : 通过express启动后台服务器

​         |-- config: webpack相关的配置文件夹(基本不需要修改)

​             |-- index.js: 指定的后台服务的端口号和静态资源文件夹（这里面有个配置autoOpenBrowser: 改一下）

​         |-- node_modules

​         |-- src : 源码文件夹

​             |-- components: vue组件及其相关资源文件夹

​          |-- App.vue: 应用根主组件

​             |-- main.js: 应用入口js

​         |-- static: 静态资源文件夹

​         |-- .babelrc: babel的配置文件

​         |-- .eslintignore: eslint检查忽略的配置

​         |-- .eslintrc.js: eslint检查的配置

​         |-- .gitignore: git版本管制忽略的配置

​         |-- index.html: 主页面文件

​         |-- package.json: 应用包配置文件 

​         |-- README.md: 应用描述说明的readme文件
```

## 2、gitkeep：空文件夹也会被管理

## 3、eslintignore:决定哪些文件不会被eslint检查

(1)*代表忽略所有文件

（2）