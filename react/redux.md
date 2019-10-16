#   一、redux的三大核心概念

1、action：**Action** 是把数据传到 store 的有效载荷。它是 store 数据的**唯一**来源。一般来说你会通过 [`store.dispatch()`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 将 action 传到 store。类似于下面的代码

```
 this.props.store.dispatch(actions.increment(number));
 这里就是通过action对象，把数据存到了store，根据下面的英文解释，action传入的数据是store的唯一数据信息来源，你可以通过store.dispatch().把他们发送到store里面去。
 Actions are payloads of information that send data from your application to your store. They are the only source of information for the store. You send them to the store using store.dispatch().
```

Action 创建函数** 就是生成 action 的方法。“action” 和 “action 创建函数” 这两个概念很容易混在一起，使用时最好注意区分。在 Redux 中的 action 创建函数只是简单的返回一个 action。**简单来说我们就是用action创建函数来生成传给dispatch的action对象**

2、*Reducers*：

Reducers指定了state对发送到store的action做出的响应（换句话说，reducer是根据发送到store里面的action进行来改变state的，记住这一步只是改变了store里面的state，外部需要监听store里面的state来渲染界面）。请记住，action仅描述发生了什么，但没有描述具体的变化。参考下面的英文

**Reducers** specify how the application's state changes in response to [actions](https://redux.js.org/basics/actions) sent to the store. Remember that actions only describe *what happened*, but don't describe how the application's state changes.



**reducer 是一个纯函数，接收旧的 state 和 action，返回新的 state。**

3、**Store** ：redux中的store是个统筹兼顾者，reducer改变的state也是由它管理者，action也是把数据交给他，从而让reducer使用来改变状态。所有我们能够看到dispatch分发action以及getState获取state以及监听都是发生在他的身上。入下面英文所说，第一条就强调了它持有state

The **Store** is the object that brings them together. The store has the following responsibilities:

- Holds application state;持有状态
- Allows access to state via [`getState()`](https://redux.js.org/api/store#getState);允许你通过getState()获取state
- Allows state to be updated via [`dispatch(action)`](https://redux.js.org/api/store#dispatchaction);
- Registers listeners via [`subscribe(listener)`](https://redux.js.org/api/store#subscribelistener);
- Handles unregistering of listeners via the function returned by [`subscribe(listener)`](https://redux.js.org/api/store#subscribelistener).

维持state；

提供 [`getState()`](https://www.redux.org.cn/docs/api/Store.html#getState) 方法获取 state；

提供 [`dispatch(action)`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 方法更新 state；

通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 注册监听器;

通过 [`subscribe(listener)`](https://www.redux.org.cn/docs/api/Store.html#subscribe) 返回的函数注销监听器。

![1](C:\Users\www\Desktop\1.png)



4、一些API

（1）`createStore(reducer, [preloadedState], [enhancer])`

参数：`reducer` *(Function)*: A [reducing function](https://redux.js.org/glossary#reducer) that returns the next [state tree](https://redux.js.org/glossary#state), given the current state tree and an [action](https://redux.js.org/glossary#action) to handle.

返回值：([*Store*](https://redux.js.org/api/store)): An object that holds the complete state of your app. The only way to change its state is by [dispatching actions](https://redux.js.org/api/store#dispatchaction). You may also [subscribe](https://redux.js.org/api/store#subscribelistener) to the changes to its state to update the UI.

（2）`combineReducers(reducers)`

参数：`reducers` (*Object*): An object whose values correspond to different reducing functions that need to be combined into one. See the notes below for some rules every passed reducer must follow.

返回值(*Function*) ：A reducer that invokes every reducer inside the `reducers` object, and constructs a state object with the same shape.



# 二、redux的整体思路

1、首先必须要有一个store对象，这个对象我们通常是用createStore()创造，里面的参数是一个reducer纯函数

```
export default createStore(reducer)
```

2、reducer纯函数式类似下面的函数，有两个参数，第一个state是以前的参数，第二个参数是个action对象，action对象，必然包含一个type属性，用来表示是什么行为，第二参数表示行为的变化，在函数内部是用switch case函数来对不同的行为指定不同的变化

```
export default function count(state = 1, action) {
  console.log("count()", state, action);
  switch (action.type) {
    
    case INCREMENT:
      return state + action.number;
      break;
    case DECREMENT:
      return state - action.number;
      break;
    default:
      return state;
      break;
  }
}
```

3、action对象，action对象是dispatch分发的参数，通常来说，我们都是通过函数来返回这个action对象

```
export const increment = number => ({ type: INCREMENT, number });
export const decrement = number => ({ type: DECREMENT, number });
```

4、通过dispatch传入actions参数来改变store的状态

```
  this.props.store.dispatch(actions.decrement(number));
```

5、action被传入store之后，reduce会调用它改变store里面的状态

6、不要忘了给App组件传入store对象和监听store的变化，从而重新渲染组件

```
ReactDOM.render( < App store = {store}/>, document.getElementById('root'))
//store的数据发生改变的时候调用，重新渲染
store.subscribe(()=>{
  ReactDOM.render( <App store = {store}/> , document.getElementById('root'))

})

```

# 三、react-redux

1、react-redux是Redux 官方提供的 React 绑定库。 具有高效且灵活的特性。

2、容器组件和展示组件：react-redux里面的组件分为两大类，一类是容器组件，还有一类是展示组件，通常来说我们是把数据放在容器组件里面，把负责UI渲染的组件叫做展示组件，通常来说展示组件进行渲染的时候是需要用到数据的，这时候我们可以通过props的属性传递，把容器组件里面的方法或者是属性传递给UI组件。容器组件是个父组件，这个数据谁需要都可以过来取。下面展示了使用容器组件和不适用容器组件的区别。

```
// CommentList.js
class CommentList extends React.Component {
  constructor() {
    super();
    this.state = { comments: [] }
  }
  componentDidMount() {
    $.ajax({
      url: "/my-comments.json",
      dataType: 'json',
      success: function(comments) {
        this.setState({comments: comments});
      }.bind(this)
    });
  }
  render() {
    return <ul> {this.state.comments.map(renderComment)} </ul>;
  }
  renderComment({body, author}) {
    return <li>{body}—{author}</li>;
  }
}
//不使用容器组件的情况下，这个状态只有他自己可以用
```

```
// CommentListContainer.js
class CommentListContainer extends React.Component {
  constructor() {
    super();
    this.state = { comments: [] }
  }
  componentDidMount() {
    $.ajax({
      url: "/my-comments.json",
      dataType: 'json',
      success: function(comments) {
        this.setState({comments: comments});
      }.bind(this)
    });
  }
  render() {
    return <CommentList comments={this.state.comments} />;
  }
}


// CommentList.js
class CommentList extends React.Component {
  constructor(props) {
    super(props);
  }
  render() { 
    return <ul> {this.props.comments.map(renderComment)} </ul>;
  }
  renderComment({body, author}) {
    return <li>{body}—{author}</li>;
  }
}
//使用容器组件的情况下，直接通过props传递就可以了
```

3、重要的API

（1)<Provider />:<Provider />组件使Redux的store可用于已包含在connect（）函数中的任何嵌套组件。下面的Counter组件，在我们经过connect函数的包装之后（联想一下form组件的包装Form.create（）（））向外界提供的就是一个form组件。而这里，在经过我们的包装之后，counter就能够接受Provide提供的store

```
1）向容器组件提供store
<Provider store={store}>
    <App />
  </Provider>
2)	
将counter组件包装成容器组件
import { connect } from 'react-redux'
  connect(
    mapStateToprops,
    mapDispatchToProps
  )(Counter)

```

（2）connect函数：connect将React组件连接到Redux store，为大多数常见场景提供了易于使用的API。`connect`**函数并不会修改传递的组件，相反，它会返回一个新的，连接到`store`的组件类**。**

connect函数接受四个可选参数

- `mapStateToProps: 函数类型

- `mapDispatchToProps:函数或者对象类型

- `mergeProps: 函数类型

- `options: 对象类型`

  ①mapStateToProps：回调函数

  如果指定了mapStateToProps函数，则新的包装器组件将订阅Redux存储库更新。这意味着每次redux的stores时，都会调用mapStateToProps。mapStateToProps的返回值必须是普通对象，它将合并到包装组件的props中。如果您不想订阅redux中的store更新，请传递null或undefined来代替mapStateToProps。mapStateToProps接受的第一个参数为state。也就是redux里面的state。

  ```
  const mapStateToprops = (state) => ({
    count: state
  })
  ```

  ②如果将mapDispatchToProps定义为函数，则它会接受两个参数，第一个参数为dispatch()。如果mapDispatchToProps被声明为带有一个参数的函数，那么这个dispatch就是redux的store里面的dispatch。mapDispatchToProps函数应返回一个对象。对象中的属于应该是方法，从而将actions给dispatch,如下面的代码所示

  ```
  const mapDispatchToProps = (dispatch) => ({
    increment: number => dispatch(increment(number)),
    decrement: number => dispatch(decrement(number))
  })
  ```

  mapDispatchToProps还可以传入一个对象：mapDispatchToProps可以是一个对象，其中每个字段都是一个action creator，就像下面的代码一样，这个等同于上面的代码，内部会自动的用dispatch给你包装。

  ```
  //import { increment, decrement } from '../redux/actions'
  export default connect(
    state => ({count: state}),
    {increment, decrement} /* 不会原样传入, 而传入包装后的函数(包含dispatch()) */，这里面的increment和decrement把他们理解为我们引入的actions对象，它会自动给你用过dispatch包装
  )(Counter)
  ```

  

# 四、redux-thunk步骤

1、引入redux-thunk并且使用thunk中间件，

```
//store文件，注意创建store的第二个参数

import { createStore,applyMiddleware } from "redux"
import thunk from 'redux-thunk';
import reducer from './reducer';

export default createStore(reducer,applyMiddleware(thunk))
```

2、在actions中的做法。本来都是返回的都是action对象，现在变为了函数

```
export function incrementAsync(number) {
  return dispatch => {
    // 1. 执行异步代码
    setTimeout(() => {
      // 2. 有结果后, 分发同步action
      dispatch(increment(number))
    }, 1000);
    
  }
}
```

