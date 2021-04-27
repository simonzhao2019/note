---
typora-copy-images-to: ./
---

## 模板语法

##### 插值语法

插值语法最为常见的就是文本形式的插值语法，用{{}}包裹起来，读的是data里面的数据，类似下面的形式

```
<span>Message: {{ msg }}</span>
//msg读的是data里面msg的属性值

```

##### 指令语法

vue中最为常见的就是指令语法，指令语法都是v-开头，v-是前缀，一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，`v-bind` 指令可以用于响应式地更新 HTML 特性

```
<a v-bind:href="url">...</a>
```

常见的指令有如下的几个

###### 1、v-bind:

缩写：`:`

v-bind:用来动态地绑定一个或多个特性，或一个组件 prop 到表达式。这里面我们要特别注意class的绑定：

```
:class="{ red: isRed }" 这种对象方式通常是用在类名确定，但是有没有不确定中，比如isRed如果为false,那么这个类名就不存在，可以联想一下active，用这个方法我么可以给某个元素动态的添加active
```

###### 2、v-show=控制显示隐藏

```
<div id="example">
<p v-show="simon"></p>//当simon的值为true时候，这个正常，显示，值为false的时候display会为none,也就是不会显示
</div>
```

###### 3、v-on:click.prevent=""绑定事件监听

可以简写成@click.prevent=""

**修饰符**：（修饰符是.,指令的参数是：）

- `.stop` - 调用 `event.stopPropagation()`。

- `.prevent` - 调用 `event.preventDefault()`。

- `.capture` - 添加事件侦听器时使用 capture 模式。

- `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。

- `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。

- `.native` - 监听组件根元素的原生事件。

- `.once` - 只触发一次回调。

- `.left` - (2.2.0) 只当点击鼠标左键时触发。

- `.right` - (2.2.0) 只当点击鼠标右键时触发。

- `.middle` - (2.2.0) 只当点击鼠标中键时触发。

- `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器

###### 4、v-if与v-show

  这个两个指令都是根据表达式的值的真假条件渲染元素。在切换时元素及它的数据绑定 / 组件被销毁并重建。如果元素是 `<template>` ，将提出它的内容作为条件块。可以把它和v-show进行比较，v-if不会解析模板，而v-show会解析模板，只是把它的style样式设置为none。有的情况下适用于v-show，有的情况是v-if。参考下面的例子

```
  <div class="shop-header-discounts"  @click="showSheet=true" v-if="info.supports" >
        <div class="discounts-left">
          <div class="activity" :class="supportClasses[info.supports[0].type]" >
            <span class="content-tag">
              <span class="mini-tag">{{info.supports[0].name}}</span>
            </span>
            <span class="activity-content ellipsis" >{{info.supports[0].content}}</span>
          </div>
        </div>
        <div class="discounts-right">
              {{info.supports.length}}个优惠
        </div>
        这里面只能用v-if而不能用v-show。因为初次渲染的情况下，info是空，这时候info.supports.length会出现type error。因此不能让他解析这个模板所。以这个时候我们不能用v-show，必须要用v-if.而等到第二次渲染的时候，info已经有值了，所有没有问题。
        这里也要注意一下对象的查找，先查找的是变量
        var a={}，先查找的是a。a.b先在作用域链中查找a，找到了之后会在原型链中查找b
```
###### 5、v-for

  对数据进行遍历循环生成标签，这里注意一点，如果和v-if使用的时候，v-for的优先级比较高。

###### 6、v-text与v-html

  值为字符串类型v-text是把标签当做文本来处理，v-html相当于innerHTML

###### 7、ref

ref指令和react中的指令非常的类似，就是在子组件身上注册引用信息，这个引用信息会保存在父组件的refs对象上面，具体的例子如下：

```
<base-input ref="usernameInput"></base-input>

这里给这个组件标签注册了引用信息，并且把引用信息保存在vm的refs身上


this.$refs.usernameInput
通过refs里面的引用信息，得到子组件标签，从而取得子组件身上的数据或者是方法


```



# 过渡动画

组件的动画主要是通过添加类名的样式实现的。给要过渡的元素加一个<transitons>标签，里面要有个name属性，然后给他添加类，代码如下，

```
<transition  name="move">
  <div class="iconfont icon-remove_circle_outline" v-if="food.count" @click.stop="updateFoodCount(false)"></div>
</transition>

然后类中加这样的样式（注意move必须和transition中name的值同名）
    &.move-enter-active, &.move-leave-active
        transition all .5s
      &.move-enter, &.move-leave-to
        transform translateX(15px) rotate(180deg)
        opacity 0
```



# 深入组件

  ##### 组件通信

###### 1、通过slot进行通信

（1）slot基础

  **slot被称为插槽，是组件的一块HTML模板，这块模板显示不显示、以及怎样显示由父组件来决定。** 实际上，一个slot最核心的两个问题是显示不显示和怎么显示。具体看下面的例子。

  ```
  //search.vue组件：在这个组件模板里面我们加了一个匿名的slot组件
  <template>
    <section class="jumbotron">
      <h3 class="jumbotron-heading">Search Github Users</h3>
      <div>
        <slot></slot>
        <button @click="search">Search</button>
      </div>
  
    </section>
  </template>
  
  
  //用到Search组件的父组件：注意，在这里我们引入并且使用了Search组件，但是在使用组件的时候我们给组件中传入了一个标签，而这个父组件传入的标签，就会出现在Search组件template(模板)里面我们写slot的位置。一句话总结slot显示的位置由子组件自身决定，slot写在子组件template的什么位置，父组件传过来的html标签将来就显示在什么位置。
  <template>
    <div class="container">
      <Search>
          <input type="text" placeholder="请输入搜索关键词" v-model="searchName"/>
      </Search>
      <Main/>
    </div>
  </template>
  
  <script type="text/ecmascript-6">
    import Search from './components/search.vue'
    import Main from './components/main.vue'
    export default {
      components: {
        Search,
        Main
      }
    }
    
    
  ```

（2）slot插槽的分类

  ①匿名插槽：没有名字的插槽，一个组件中只有一个

 ②具名插槽：匿名插槽没有name属性，所以是匿名插槽，那么，插槽加了name属性，就变成了具名插槽。具名插槽可以在一个组件中出现N次，出现在不同的位置。

③作用域插槽：作用域插槽就是在slot上面可以绑定数据的插槽。换句话说，数据在子组件身上，但是子组件slot的标签内容并不是子组件决定的，而是父组件决定的，怎么把这些数据传给html标签的问题就是作用域插槽要做的。具体看下面的例子

  ```
  //子组件Search。在子组件上面有个slot插槽。绑定了search。这样数据就被slot带了过去
  <template>
    <section class="jumbotron">
      <h3 class="jumbotron-heading">Search Github Users</h3>
      <div>
        <slot :seach="searchName"  name="left"></slot>
        <button @click="search">Search</button>
      </div>
  
    </section>
  </template>
  
  <script type="text/ecmascript-6">
    export default {
      data(){
        return {
          searchName:"simon"
        }
      }
      }
      
  //这里是父组件，slot已经被数据带了过来，这里我们要思考的是如何去数据的问题。取数据需要用到template， 里面要写个slot-scope属性，标签体里面就可以取数据了，另外如果是具名的slot插槽。名字也是写在这里
  <template>
    <div class="container">
      <Search>
        <template slot-scope="slots" slot="left" >
          <div >{{slots.seach}}</div>
        </template>
         
      </Search>
      <Main/>
    </div>
  </template>
  
  <script type="text/ecmascript-6">
    import Search from './components/search.vue'
    import Main from './components/main.vue'
    export default {
      components: {
        Search,
        Main
      }
    }
  ```

###### 2、通过props属性进行数据的通信

通常是把父组件的数据传递给子组件，例如下面的做法

```
<template>
   <div class="todo-container">
    <div class="todo-wrap">
    <Header v-bind:addTodo="addTodo"></Header>
    <List :todos="todos" :removeTodo="removeTodo"></List>
    <Footer :todos="todos" :selectAll="selectAll" :clearFinish="clearFinish"></Footer>
     
     
    </div>
  </div>
  给Header组件传了一个函数
    export default {
      props: { // 指定了标签属性: 属性名/属性值类型/属性的必要性
      addTodo: {
        type: Function,
        required: true,
      }
    },
    在组件的props里面进行了接收
    
```

###### 3、监听子组件事件

```js
//父组件
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
	@enlarge-text="postFontSize += 0.1"
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
    ></blog-post>
  </div>
</div>

//子组件
<button @click="$emit('enlargeText')">
  Enlarge text
</button>


//这里面父组件监听了enlargeText事件，而子组件通过触发enlargeText来通知父组件，可以进行某些操作
```



###### 4、消息订阅与发布(pubsub-js库)

这种方式多用在react里面，react里面有详细的分析，这里不再论述

###### 5、利用vuex

对状态进行集中处理的插件库，可以查看vuex的专门笔记

###### 6、事件总线（EventBus）

EventBus和pubsub非常的类似，其中emit提供数据，on监听数据的变化，里面第二个参数就是回调函数，在这里我们可以可以拿到数据，

（1）事件总线有两种第一种没有绑定在原型上面，是下面代代码中的形式。

```
//这个是APP父组件，Header组件被绑定了监听
<template>
   <div class="todo-container">
    <div class="todo-wrap">
    <Header @addTodo="addTodo"></Header>
   
    <List :todos="todos" :removeTodo="removeTodo"></List>
    <Footer :todos="todos" :selectAll="selectAll" :clearFinish="clearFinish"></Footer>
     
     
    </div>
  </div>
</template>
 如果要执行一些操作，也可以不用上面的形式
  <Header ref="addTodo"></Header> this.$refs.addtodo.$on("addtodo")
			//对应的js
			  methods:{
    addTodo (todo){
      this.todos.unshift(todo)
    },
//这个就是我们监听的Header子组件里面methods的方法
    add (){
      if(!this.title){
        return
      }
      const todo={id:Date.now(),title:this.title,finish:false}
      this.title=""
      this.$emit("addTodo",todo)//需要注意的是这里，我们看到他发送了数据

    }
  }
这种自定义的监听非常的少,因为局限比较大，在这里数据是header子组件发射的，我们必须给他绑定监听才能取到数据。而如果想要给他绑定鉴定，必然是父组件给他绑定的，这样我们还是不能实现不同组件（不是父子关系）之间的组件同行
```

（2）还有一种是借助一个vm对象，把它作为全局事件总线(对象)保存到Vue原型对象上,这样所有的组件就都能使用，可以通过下面的例子来了解

```
//main.js入口文件

import Vue from 'vue'

import App from './App.vue'
import './base.css'
Vue.prototype.$bus = new Vue() //这里必须让eventbus的值是个vm对象而不是空对象，是因为只有vm对象上面才有emit/on/off等方法

new Vue({
  el: '#app',
  components: {
    App
  },
  template: '<App/>'
})
//其他组件mount中的代码
 mounted() {
      // 绑定自定义事件监听(removeTodo)
      this.$bus.$on('removeTodo', this.removeTodo)
      
      这个组件是没有$bus对象的,但是由于我们在上文的main文件给vue的原型中加了一个，并且把这一个vm对象的值赋给了他，所以他会到vue原型当中中，而原型当中的$bus值又是一个vm对象，所以可以使用$on方法.这个就是全局事件总线的本质
    },
```



  ##### 组件的选项配置

######   1、data

  Vue 实例的数据对象。Vue 将会递归将 data 的属性转换为 getter/setter，从而让 data 的属性能够响应数据变化。**对象必须是纯粹的对象 (含有零个或多个的 key/value 对)**：浏览器 API 创建的原生对象，原型上的属性会被忽略。大概来说，data 应该只能是数据 ，不是直接的vm实例，我们在定义属性的时候，必须把它定义成函数的形式，例如，这种我们就必须采用函数的形式，这样vm实例上面的data才会不一样，本质上来说，我们new Vue的时候只能返回一个vm实例。因此，处理new Vue的时候我们可以直接采用data为对象的形式之外，其他的

  ```
  //除了这种形式的vue里面的data可以采用对象的形式之外，其它形式，例如我们我们下面说的那种，data的形式必须是一个函数，返回一个对象，因为data是vue实例的数据对象，只有采用函数的形式返回，我们才能确保实例对象上的data是那个组件私有的。确保了每个组件对应的vm实例对象上的data都是不一样的
  new Vue({
  	data:{}
    render: h => h(App),
    router,
    store
  }).$mount('#app')。
  
  //这种暴露的形式，里面的data必须写成函数，这种才能确保vm实例中保存的data不是同一个data对象
  export default {
  data(){
  return {
  
  }
  },
    name: 'app',
     mounted () {
        this.$store.dispatch('getAddress')
        //this.$store.dispatch('getShops')
        this.$store.dispatch("autoLogin")
      },
    components:{
      FooterGuide
    },
    
  }
  ```

######   2、props

  props 可以是数组或对象，用于接收来自父组件的数据。入下面的代码所示

  ```
  export default{
  props: {
      // 检测传过来的height类型
      height: Number,
      // 检测类型 + 其他验证
      age: {
        type: Number,
        default: 0,
        required: true,
        validator: function (value) {
          return value >= 0
        }
      }
  }
  ```

######   3、computed

  计算属性将被混入到 Vue 实例中。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。注意，里面的方法不能写成箭头函数的形式， 因为这里的计算属性需要被混入到Vue的实例当中，因此它的this必须指定为Vm实例。采用箭头函数的形式，Vue控制不了this的指向。

  ```
     computed: {
        /* 判断phone是否是一个正确的手机号 */
        isRightPhone () {
          return /^1\d{10}$/.test(this.phone)
        }
      },
      如果属性不仅有get,还有setter方法，那么我们必须把属性写成对象的形式。入下图所示，注意，里面是一个配置对象，顺序无所谓，但是属性名不能变。
       computed:{
     fullName2:{
       get (){
         return this.firstName+"-"+this.lastName
       },
       set(val){
        const name=val.split("-")
        this.firstName=name[0]
        this.lastName=name[1]
       }
     }
   },
  ```

###### 4、methods  

methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 `this` 自动绑定为 Vue 实例。和上面的computed一样，不能使用箭头函数的形式，这里面的this指向才能确保指向vm这个实例

```
var vm = new Vue({
  data: { a: 1 },
  methods: {
    plus: function () {
      this.a++
    }
  }
})
```

###### 5、watch

一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 `$watch()`，遍历 watch 对象的每一个属性。同样，不能使用箭头函数的形式来定义watch里面的回调函数，这样会导致vue控制不了this的指向。

```
  watch: {
    a: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    },
    }
```

##### 组件注册

###### 1、全局组件注册

Vue.component('my-component-name', { /* ... */ })这种注册组件的方式为全局注册，也就是说它们在注册之后可以用在任何新创建的 Vue 根实例 (`new Vue`) 的模板中，因为这种注册方式等于是给构造函数中直接添加了一个属性，所以vm实例可以直接使用

```
Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })

new Vue({ el: '#app' })
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
  <component-c></component-c>
</div>
```

###### 2、局部注册的组件

局部注册就是只能在父元素中使用的组件，比如我们在暴露了一个组件模块为A，那么在B模块中就可以这样使用。可以联想一下作用域，这个A只可以在B组件中使用

```
import ComponentA from "路径"
export default {
  components: {
    ComponentA
  },
  // ...
}
```

##### 组件的生命周期

| 生命周期钩子  | 组件状态                                                     |                          最佳实践                           |
| :------------ | :----------------------------------------------------------- | :---------------------------------------------------------: |
| beforeCreate  | 实例初始化之后，this指向创建的实例，不能访问到data、computed、watch、methods上的方法和数据 |                  常用于初始化非响应式变量                   |
| created       | 实例创建完成，可访问data、computed、watch、methods上的方法和数据，未挂载到DOM，不能访问到$el属性，$ref属性内容为空数组 |             常用于简单的ajax请求，页面的初始化              |
| beforeMount   | 在挂载开始之前被调用，beforeMount之前，会找到对应的template，并编译成render函数 |                              -                              |
| mounted       | 实例挂载到DOM上，此时可以通过DOM API获取到DOM节点，$ref属性可以访问 |             常用于获取VNode信息和操作，ajax请求             |
| beforeupdate  | 响应式数据更新时调用，发生在虚拟DOM打补丁之前                | 适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器 |
| updated       | 虚拟 DOM 重新渲染和打补丁之后调用，组件DOM已经更新，可执行依赖于DOM的操作 |        避免在这个钩子函数中操作数据，可能陷入死循环         |
| beforeDestroy | 实例销毁之前调用。这一步，实例仍然完全可用，this仍能获取到实例 |     常用于销毁定时器、解绑全局事件、销毁插件对象等操作      |
| destroyed     | 实例销毁后调用，调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁 |                              -                              |

![vue组件的生命周期](D:\前端思维导图\VUE\vue组件的生命周期.png)

# vue的全局API

##### 1、vue.filter过滤器

vue中过滤器经常被用来格式化文本，也就是说对文本进行一些格式化的处理，过滤器不会改变vue中data的数据，但是在进行页面渲染的时候，会以把过滤器中的返回值交给浏览器然浏览器进行渲染。下面是一个简单的过滤器

```
<div id="test">
  <h2>{{money|currency}}</h2>  这个显示结果是$345
</div>

<script type="text/javascript" src="../js/vue.js"></script>
<script>
  Vue.filter("currency", function (value) {
      return "$" + value;      //回调函数里面value接受的值是管道操作符“ | ”前面的值。返回值，就只最终你使用了过滤器的那个便签的显示结果
    });
  new Vue({
    el:"#test",
    data:{
      money:345
    }
  })
```

##### 2、Vue.directive()注册指令

这个方法可以用用来注册全局指令，具体的例子看下面的代码

```
<div id="test">
  <p v-my-directive="msg"></p>
</div>
  
  
  Vue.directive('my-directive', (el, binding)=> {
       el.innerHTML = binding.value.toUpperCase()
    })
    new Vue({
      el:"#test",
      data:{
        msg:"helloWorld"
      }
    })
  
  第一个参数是指令名，在使用的时候我们需要给他加上v-第二个binding会接受这个指令传过来的信息，value存的就是指令闯传古来的msg的值
```

##### 3、Vue.use( plugin )

使用 Vue.js 插件。如果插件是一个对象，必须提供 `install` 方法。如果插件是一个函数，它会被作为 install 方法。install 方法调用时，会将 Vue 作为参数传入。

该方法需要在调用 `new Vue()` 之前被调用。

##### 4、Vue.set( target, propertyName/index, value )

向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新属性，因为 Vue 无法探测普通的新增属性，例如下面的例子

```
  [ADD_FOOD_COUNT](state, food) {
    if (!food.count) {
      Vue.set(food, "count", 1);
      state.cartFood.push(food);
    } else {
      food.count++;
    }
  },
  在这个例子当中，state里面的food最初始没有count这个属性的，这里我们给他添加count时候，并没有对这个属性进行数据绑定，所以我们必须通过Vue.set来设置，通知Vue来监视这个数据
```

