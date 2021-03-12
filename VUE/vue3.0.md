## 一、全局api

### 1.createApp

在Vue3中，使用的是createApp()来生成vue实例

调用 `createApp` 返回一个应用实例。该实例提供了一个应用上下文。应用实例挂载的整个组件树共享相同的上下文，该上下文提供了之前在 Vue 2.x 中“全局”的配置。

```js
参数：该函数接收一个根组件选项对象作为第一个参数，使用第二个参数，我们可以将根 prop 传递给应用程序：
const app = Vue.createApp({
  data() {
    return {
      ...
    }
  },
  methods: {...},
  computed: {...}
  ...
})
//只有一个参数
const app = Vue.createApp(
  {
    props: ['username']
  },
  { username: 'Evan' }
)
//两个参数，这时候实例当中的username为Evan
```

createAPP支持链式调用例如

```js
createApp(App).use(router).use(vuex).mount('#app')
这里就使用的router以及vuex

```

