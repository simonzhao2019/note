# 一、React-router基础

React项目的是官方推荐的路由组件库，它分为三个部分：

- react-router 核心组件

- react-router-dom 应用于浏览器端的路由库（单独使用包含了react-router的核心部分）

- react-router-native 应用于native端的路由

在使用recat-router的时候，我们不应该直接安装 react-router，react-router包为React Router 应用提供了核心的路由组件和函数，另外两个包提供了特定环境的组件（浏览器和 react-native 对应的平台），不过他们也是将 react-router 导出的模块再次导出。因此在使用的时候我们直接选择react-router-dom 

# 二、React-router中的主要组件

#### 1、Router（路由器组件）组件

##### （1）BrowserRouter组件

BrowserRouter主要使用在浏览器中，也就是WEB应用中。它利用HTML5 的history API来同步URL和UI的变化。当我们点击了程序中的一个链接之后,BrowserRouter就会找出与这个URL匹配的Route，并将他们对应的组件渲染出来。 BrowserRouter是用来管理我们的组件的，那么它当然要被放在最顶级的位置，而我们的应用程序的组件就作为它的一个子组件而存在。

##### （2）HashRouter组件

`HashRouter` 使用 URL 的 hash (例如：`window.location.hash`) 来保持 UI 和 URL 的同步。

> 注意： 使用 hash 的方式记录导航历史不支持 `location.key` 和`location.state`。在以前的版本中，我们为这种行为提供了 shim，但是仍有一些问题我们无法解。任何依赖此行为的代码或插件都将无法正常使用。由于该技术仅用于支持传统的浏览器，因此在用于浏览器时可以使用 `<BrowserHistory>` 代替。
>
 ##### （3）MemoryRouter内存路由器组件

##### （4）NativeRouter的路由组件

##### （5）StaticRouter 地址不改变的静态路由组件

对于单页应用来说，通常是只有一个路由器组件，因此，我们是在index.js中引入并且使用路由器组件，类似于下面的形式

```
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from "react-router-dom";
import App from './App';
import "lib-flexible";
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
  <BrowserRouter>
    <App />        //将App根组件包裹在BrowserRouter组件当中
  </BrowserRouter>,
  document.getElementById("root")
);
```

#### 2、Route组件

Route组件是最为核心的组件，主要功能是负责显示路由对应的组件。

##### （1）Route组件的渲染方式

对应路由组件的渲染方式有三种：

（1）component: 这是最常用也最容易理解的方式，给什么就渲染什么。

```
  <Route path="/home" component={Home}></Route>对应路径渲染Home这个组件
```

（2）render: render的类型是function，Route会渲染这个function的返回值。因此它的作用就是附加一些额外的逻辑。

```
<Route path="/home" render={() => {
    console.log('额外的逻辑');
    return (<div>Home</div>);
    }/>
```

（3）children模式的渲染

```
// 在匹配时，容器的calss是light，<Home />会被渲染
// 在不匹配时，容器的calss是dark，<About />会被渲染
<Route path='/home' children={({ match }) => (
    <div className={match ? 'light' : 'dark'}>
      {match ? <Home/>:<About>}
    </div>
  )}/>
```

##### （2）路由指定渲染的组件具有的props

无论是哪种模式的渲染我们都需要知道，只要是我们把这个组件作为路由组件进行了渲染，那么这个组件就会被传入三个props，分别是:

###### ①match

在传入的match属性中，我们最需要注意的就是match.params.这个属性下面，我们可以拿到从location中解析出来的参数。当然，如果想要接收参数，我们的Route的path也要使用特殊的写法。参考下面的代码

```
<Link to='/p/1' />
<Link to='/p/2' />
<Link to='/p/3' />
......
<Route path='/p/:id' render={(match)=<h3>当前文章ID:{match.params.id}</h3>)} />
```

###### ②location

Location 是指你当前的位置，下一步打算去的位置，或是你之前所在的位置，形式大概就像这样：

```
{
  key: 'ac3df4', // 在使用 hashHistory 时，没有 key
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: true
  }
}
```

###### ③history

history 是 React Router 的两大重要依赖之一（除去 React 本身），在不同的 Javascript 环境中，`history` 以多种形式实现了对于 session 历史的管理。

```
history 对象通常会具有以下属性和方法：
    (1)length -（ number 类型）指的是 history 堆栈的数量。
    (2)action -（ string 类型）指的是当前的动作（action），例如 PUSH，REPLACE 以及 POP 。
        location -（ object类型）是指当前的位置（location），location 会具有如下属性：
        pathname -（ string 类型）URL路径。
        search -（ string 类型）URL中的查询字符串（query string）。
        hash -（ string 类型）URL的 hash 分段。
        state -（ string 类型）是指 location 中的状态，例如在 push(path, state) 时，state会描述什么时候 location 被放置到堆栈中等信息。这个 state 只会出现在 browser history 和 memory history 的环境里。
    (3)push(path, [state]) -（ function 类型）在 hisotry 堆栈顶加入一个新的条目。
    (4)replace(path, [state]) -（ function 类型）替换在 history 堆栈中的当前条目。
    (5)go(n) -（ function 类型）将 history 对战中的指针向前移动 n 。
    (6)goBack() -（ function 类型）等同于 go(-1) 。
    (7)goForward() -（ function 类型）等同于 go(1) 。
    (8)block(prompt) -（ function 类型）阻止跳转，（请参照 history 文档）
```

##### （3）path

path用来指定路由路径，值为字符串

```
<Route path="/about" component={About}/> 去about这个路由
```

#### 3、Link组件

Link就像是一个个的路牌，为我们指明组件的位置。Link使用声明式的方式为应用程序提供导航功能，定义的Link最终会被渲染成一个a标签。Link使用to这个属性来指明目标组件的路径，可以直接使用一个字符串，也可以传入一个对象。

```
import { Link } from 'react-router-dom'
// 字符串参数
<Link to="/query">查询</Link>

// 对象参数
<Link to={{
  pathname: '/query',
  search: '?key=name',
  hash: '#hash',
  state: { fromDashboard: true }
}}>查询</Link>
```

#### 4、NavLink组件

NavLink是一个特殊版本的Link，可以使用activeClassName来设置Link被选中时被附加的class，使用activeStyle来配置被选中时应用的样式。此外，还有一个exact属性,此属性要求location完全匹配才会附加class和style。这里说的匹配是指地址栏中的URl和这个Link的to指定的location相匹配。

```
// 选中后被添加class selected
<NavLink to={'/'} exact activeClassName='selected'>Home</NavLink>
// 选中后被附加样式 color:red
<NavLink to={'/gallery'} activeStyle={{color:red}}>Gallery</NavLink>
```
NavLink组件的常用属性
- to 可以是字符串或者对象，同Link组件

- exact 布尔类型，完全匹配时才会被附件class和style

- activeStyle Object类型

- activeClassName 字符串类型

- strict: bool类型，当值为 `true` 时，在确定位置是否与当前 URL 匹配时，将考虑位置 `pathname` 后的斜线。
 #### 5、Switch组件

渲染匹配地址(location)的第一个 `<Route>`或者`<Redirect>`，也就是说如果我们将路由组件包裹在这里面之后，他在进行匹配的时候，在找到第一个匹配的路由之后就会停止查找。

```
import { Switch, Route } from 'react-router'

<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/about" component={About}/>
  <Route path="/:user" component={User}/>
  <Route component={NoMatch}/>
</Switch>

这个如果不写<Switch>他就会渲染所有的路由，因为下面的地址都符合
```

## 6、Redirect组件

当这个组件被渲染是，location会被重写为Redirect的to指定的新location。它的一个用途是登录重定向，比如在用户点了登录并验证通过之后，将页面跳转到个人主页。

```
<Redirect from="/" to="/home" exact />   from表示那个那个路径会被重新定向到/home组件
```