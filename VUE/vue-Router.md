# vue-router概念

vue-router就是WebApp的链接路径管理系统。 vue-router是Vue.js官方的路由插件（因此在引入路由组件之后的第一部就是声明使用vueRouter路由器组件），它和vue.js是深度集成的，适合用于构建单页面应用。vue的单页面应用是基于路由和组件的，路由用于设定访问路径，并将路径和组件映射起来。

```
import Vue from 'vue'
import VueRouter from 'vue-router'  //引入VueRouter路由器插件

import routes from './routes'

Vue.use(VueRouter)    //声明使用路由器插件

export default new VueRouter({
  mode: 'history',   
  routes ， //路由器里面注册路由
})
```

路由器对象用来操作路由的跳转。$Router里面存在着一些方法来帮助我们操作路由比如$Router.push()、$Router.replace等。而路由对象$route里面存放着当前路由的信息比如path

# vue-router的两种模式

##### 1、hash模式(带#的模式)

 —— 使用 URL 的 hash 来模拟一个完整的 URL，当 URL 改变时，页面不会重新加载。** hash（#）是URL 的锚点，代表的是网页中的一个位置，单单改变#后的部分，浏览器只会滚动到相应位置，不会重新加载网页，也就是说hash 出现在 URL 中，但不会被包含在 http 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面；同时每一次改变#后的部分，都会在浏览器的访问历史中增加一个记录，使用”后退”按钮，就可以回到上一个位置；所以说Hash模式通过锚点值的改变，根据不同的值，渲染指定DOM位置的不同数据。hash 模式的原理是 onhashchange 事件(监测hash值变化)，可以在 window 对象上监听这个事件。

##### 2、history模式（不带#的模式）

由于hash模式会在url中自带#，如果不想要很丑的 hash，我们可以用路由的 history 模式，只需要在配置路由规则时，加入"mode: 'history'",这种模式充分利用了html5 history interface 中新增的 pushState() 和 replaceState() 方法。这两个方法应用于浏览器记录栈，在当前已有的 back、forward、go 基础之上，它们提供了对历史记录修改的功能。只是当它们执行修改时，虽然改变了当前的 URL ，但浏览器不会立即向后端发送请。

##### 3、两种模式的写法

```
const router = new VueRouter({
  mode: 'history',、//这里能够决定使用hash模式还是history模式
  routes: [...]
})
```

# vue-Router的使用

1、引入路由组件，并声明使用 路由

```
import Vue from 'vue'
import VueRouter from 'vue-router'  //引入VueRouter
import routes from './routes';


Vue.use(VueRouter)    //声明使用路由器

export default new VueRouter({
  mode:"history",     
  routes
})
用构造函数返回一个路由器对象，在这个构造函数中有两个必须要配置的东西，一个是路由模式，还有就是一个路由数组。其中路由数组我们采用的是外部引入的方式，下面是我们引入的路由数组。在这个路由数组中有几个地方需要注意，一个是默认显示的路由，在这个路由数组中，我们看到他是通过redirece的方式将其重定向了一个路由，还有就是子路由的书写方式。
import About from "../pages/About.vue";
import Home from "../pages/Home.vue";
import News from '../pages/News.vue';
import Message from '../pages/Message.vue';
import MessageDetail from '../pages/MessageDetail.vue';
export default [
  {
    path: "/about",
    component: About
  },
  {
    path: "/home",
    component: Home,
    children: [
      {
        path: "news", //这种方式代表的是查找home下面的news路由。如果是/new就代表的是查找root下面的news路由。子路由除了采用这种方式书写意外，还可以采用下面message的那种形式
        component: News
      },
      {
        path: "/home/message",
        component: Message,
        children: [
          {
            path: "/home/message/detail/:id",
            component: MessageDetail
          }
        ]
      }
    ]
  },
  {
    path: "/",
    redirect: "/about"
  }
];
下面一步是在vm对象中，添加路由器对象

new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
在模板中显示路由的方法
   <router-link class="list-group-item" to="/about">About</router-link>
          <router-link class="list-group-item" to="/home">Home</router-link>
          //生成路由链接
        </div>
      </div>
      <div class="col-xs-6">
        <div class="panel">
          <div class="panel-body">
          <router-view></router-view>  //路由显示的地方
          </div>
        </div>
        
           除此之外还有个<keep-alive>
<router-view></router-view>
</keep-alive>	这个会让被包裹的路由组件在我们切换的时候不会立即死亡
```

# 路由传参

路由传参的大概有五种左右，这里我们就说下常见的两种路由传参方式

1、通过name属性传参，例如下面的例子

```
  {
    path: "/about",
    name:"hello",
    component: About
  },
  我们在about组件的$route中就能够找到name属性，值就是hello
```

2、通过params来传参，例如下面的这种

```
 children: [
          {
            path: "/home/message/detail/:id",
            component: MessageDetail
          }
        ]
        这里我们在路径中加入了一个params参数，参数的key为id。
        在路由链接中，我们就可以这样写
         <router-link :to="`/home/message/detail/${message.id}`" >{{message.title}}</router-link>
         这里注意to前面要加：。要把他们当做表达式来解释，而不是字符串了。这个我们传入了message.id
         他就会被作为$route.params中id的属性值。
```

# $route和$router的区别

1、$route 是“路由信息对象”，包括 path，params，hash，query，fullPath，matched，name 等路由信息参数。

① $route.path** 字符串，对应当前路由的路径，总是解析为绝对路径，如 "/order"。

② $route.params** 一个 key/value 对象，包含了 动态片段 和 全匹配片段， 如果没有路由参数，就是一个空对象。

③ $route.query** 一个 key/value 对象，表示 URL 查询参数。 例如，对于路径 /foo?user=1，则有 $route.query.user为1， 如果没有查询参数，则是个空对象。

*$route.hash** 当前路由的 hash 值 (不带 #) ，如果没有 hash 值，则为空字符串。

⑤ $route.fullPath** 完成解析后的 URL，包含查询参数和 hash 的完整路径。

⑥ $route.matched** 数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象。

⑦ $route.name   当前路径名字**

2、$router常见跳转方法:

```
 this.$router.go(-1)//跳转到上一次浏览的页面
        this.$router.replace('/menu')//指定跳转的地址
        this.$router.replace({name:'menuLink'})//指定跳转路由的名字下
        this.$router.push('/menu')//通过push进行跳转
        this.$router.push({name:'menuLink'})//通过push进行跳转路由的名字下

```

# 路由守卫

1、全局路由守卫，代码如下

```
import Vue from 'vue';
import VueRouter from 'vue-router';
import routes from './routes';
//声明使用路由
Vue.use(VueRouter);

const router=new VueRouter({
  mode: 'history',
  routes
})
router.beforeEach((to,from,next)=>{
  console.log(to)
  console.log("---------------------")
  console.log(from)
  next()
})
export default router

全局路由守卫，必须要执行next函数才会放行
to：即将要进入的路由
from:要离开的路由;
下面的是重定向，由”/“路由跳转至”/home“,分别打印的两个对象
to对象:
{name: undefined, meta: {…}, path: "/home", hash: "", query: {…}, …}
fullPath: "/home"
hash: ""
matched: [{…}]
meta: {isShow: true}
name: undefined
params: {}
path: "/home"
query: {}
from对象：
{name: null, meta: {…}, path: "/", hash: "", query: {…}, …}
fullPath: "/"
hash: ""
matched: []
meta: {}
name: null
params: {}
path: "/"
query: {}
```

2、组件路由守卫

代码如下:

```
    beforeRouteEnter (to, from, next) {
      // 指定回调函数在组件对象创建之后执行, 且会将组件对象传入
      next((component) => { 
         // 如果已登陆了, 自动跳转到profile
        if (component.$store.state.user.token) {
          next('/profile')
        } else { // 否则放行
          next()
        }
      })
      这个方法是写在methods里面的，这里面注意有时候可能要用到组件对象，由于是进入之前，因此组件对象还没有生成，这里不能使用this，this指向的不是组件对象，这时候，可以指定next函数，里面参数是组件对象，能够指定什么时候条调用
```

