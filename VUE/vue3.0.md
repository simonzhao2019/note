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

## 二、组合式API

### 1、setup

新的 `setup` 组件选项在创建组件**之前**执行，一旦 `props` 被解析，就作为组合式 API 的入口点。

例子：

```js
  export default {
    components:[tableSon],
    data() {
      return {
        data: [],
        pagination: {
          showLessItems: true,
          showQuickJumper: true,
          showSizeChanger: true,
        },
        query: {},
        loading: false,
        columns,
      }
    },
    setup(props,context){
      console.log('props',props)
      console.log('context',context)
    },    //两个参数
     //输出结果：props Proxy {} 
/*context 
{expose: ƒ}
attrs: Proxy
emit: (event, ...args) => instance.emit(event, ...args)
expose: exposed => {…}
slots: (...)
get attrs: ƒ attrs()
get emit: emit() { return (event, ...args) => {…}
get slots: ƒ slots()
__proto__: Object */
  //这里通过setup函数return出去的数据，我们在组建中都可以直接使用
   /*   例如：
         setup(props,context){
      console.log('props',props)
      console.log('context',context)
      const count=1
      return {
        count
      }
    },*/
  //这里我们return出去的count，调用的时候取值就是1    
      
    mounted() {
      this.fetch()
    },
    methods: {
      handleTableChange(pagination) {
        const pager = { ...this.pagination }
        pager.current = pagination.current
        this.pagination = pager
        this.fetch()
      },
      fetch() {
        this.loading = true
        getList({
          pageSize: this.pagination.pageSize,
          current: this.pagination.current,
        }).then(({ data, total }) => {
          const pagination = { ...this.pagination }
          pagination.total = total
          this.loading = false
          this.data = data
          this.pagination = pagination
        })
      },
    },
  }
```

### 2、带 ref的响应式变量

上面例子中，我们虽然通过setup返回了count,但是counte并不是响应式的，这时候就需要引入ref.

```js
import { ref } from 'vue'

const counter = ref(0)
ref函数返回的是一个对象，并且你传入的参数，就是作为value的初始值


console.log(counter) // { value: 0 }
console.log(counter.value) // 0



在setup函数中，更改count必须使用value.

如果我们通过下面的方式去更改count是可以的
  setup(props,context){
      const count=ref(3)
       count.valu=4
      console.log('props',props)
      console.log('context',context)
      // const count=1
      return {
        count
      }
    },
但是，如果我们通过以下的方式更改coun则不被允许
  setup(props,context){
      const count=ref(3)
       count=4
      console.log('props',props)
      console.log('context',context)
      // const count=1
      return {
        count
      }
    },
报错为 "count" is read-only       
```

### 3、setup内部注册生命周期钩子

```js
import { fetchUserRepositories } from '@/api/repositories'
import { ref, onMounted } from 'vue'

// 在我们的组件中
setup (props) {
  const repositories = ref([])
  const getUserRepositories = async () => {
    repositories.value = await fetchUserRepositories(props.user)
  }

  onMounted(getUserRepositories) // 在 `mounted` 时调用 `getUserRepositories`

  return {
    repositories,
    getUserRepositories
  }
}

在setup函数中，我们注册了onmout。传入的函数类型会在mounted的时候滴调用。onmouted的参数为回调函数
```

### 4、watch

```js
import { fetchUserRepositories } from '@/api/repositories'
import { ref, onMounted } from 'vue'

// 在我们的组件中
setup (props) {
  const repositories = ref([])
  const getUserRepositories = async () => {
    repositories.value = await fetchUserRepositories(props.user)
  }

  onMounted(getUserRepositories) // 在 `mounted` 时调用 `getUserRepositories`
 watch(user, getUserRepositories) //watch函数，当user发生变化时会调用第二个函数
  return {
    repositories,
    getUserRepositories
  }
}
```

### 5、computed

```
const twiceTheCounter = computed(() => counter.value * 2)


在这里，computed 函数返回一个作为 computed 的第一个参数传递的 getter 类回调的输出的一个只读的响应式引用。为了访问新创建的计算变量的 value，我们需要像使用 ref 一样使用 .value property。   


简单来说，computed。传递的是引用的值
```

