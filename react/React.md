# React组件

react组件共有两种形式，一种是函数组件，还有一种是类组件，函数组件是不太状态的组件，类组件是带有状态的组件，因此在实际使用的过程中，大部分情况下我们都是使用类组件

#### （一）组件的类型

##### 1、类组件

```
这里就是使用类组件的rect组件，里面涉及到状态的使用
export default class User extends Component {
  state = {
    isShow: false, // 是否显示对话框
    users: [], // 所有用户的列表
    roles: [] // 所有角色的列表
  };
    return (
      <div>
        <Card title={title}>
          <Table
            columns={this.columns}
            rowKey="_id"
            dataSource={users}
            bordered
            pagination={{ defaultPageSize: 4, showQuickJumper: true }}
          />
          <Modal
            title={user._id ? "修改用户" : "添加用户"}
            visible={isShow}
            onCancel={() => this.setState({ isShow: false })}
            onOk={this.AddOrUpdateUser}
          >
            <UserForm
              setForm={form => (this.form = form)}
              user={user}
              roles={roles}
            />
          </Modal>
        </Card>
      </div>
    );
  }
}

```

##### 2、函数组件

函数组件是较为简单的组件形式，没有状态一说，没有render函数，下面这个函数组件就是从外部接受了prop属性，然后直接显示

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

#### (二)受控组件与非受控组件

##### 1、受控组件

在HTML中，表单元素（如 input、textarea、select）之类的表单元素通常可以自己维护state，并根据用户的输入进行更新。而在React中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过 setState()来更新。 在此，我们将 React的state作为唯一的数据源，通过渲染表单的React组件来控制用户输入过程中表单发送的操作。 这个“被React通过此种方式控制取值的表单输入元素”被成为**受控组件**。

##### 2、非受控组件

不被React组件控制的组件。在受控制组件中，表单数据由 React组件处理。其替代方案是不受控制组件，其中表单数据由DOM本身处理。文件输入标签就是一个典型的不受控制组件，它的值只能由用户设置，通过DOM自身提供的一些特性来获取。

简单来说受控组件和非受控组件的区别就是是不是受React控制，比如input这类标签，就是非受控组件，因为他是自己管理自己状态的value值

##### 3、Refs的概念

Refs 是一个 获取 DOM节点或 React元素实例的工具。简单来说，通过refs我们能够拿到真实的Dom节点，从而通过React来管理它的状态，直接修改它的值。

##### 4、Ref的使用方式

###### （1）通过 React.createRef() 

```
lass TestComp extends React.Component {
  constructor(props) {
    super(props);
    // 创建一个 在实例上先创建一个ref，并且他把存储在react实例上面，名字为textInput
    this.textInput = React.createRef();
  }
  focusEvent() {
    //通过current属性拿到真实的Dom元素
    this.textInput.current.value;
  }
  render() {
    return (
      <div>
        <input type="text" ref={this.textInput} />  //这里我们就把input这个标签保存在了this上面的ref实例上面
        <input type="button" value="获取文本框焦点事件" onClick={this.focusEvent}/>
      </div>
    );
  }
}


```

###### （2）字符串形式的Ref形式

```
<input ref="input" />

let inputEl = this.refs.input;这时候就可以拿到Input这个标签
```

#### （三）组件状态

React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。

##### 1、State的形式

state通常是写在 constructor里面的，类似于下面的形式

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
 
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
 
ReactDOM.render(
  <Clock />,
  document.getElementById('example')
);
```

##### 2、组件状态的更新

组件不能直接更新状态，必须通过setState()进行更新，通过setState（）进行更新有两种方式：入下面的代码所示

```
（1）回调函数的形式
this.setState((state, props) => {
  return {counter: state.counter + props.step};
});
（2）对象的形式
this.setState({quantity: 2})//这种形式等于是上面那种形式的语法糖。
对于该使用和何种形式的state,要根据不同的情况决定。通常来说，如果新状态不依赖于原状态使用对象方式
如果新状态依赖于原状态 ===> 使用函数方式。这是因为如果是函数形式，在每次使用state的时候我们等于是当时计算的最新的值
```

##### 3、状态更新的异步和同步问题

（1）setState在生命周期函数、事件监听的回调中都是异步执行的，换句话由react控制的组件，其setState都是异步执行的，这是react官方能够控制的模块以及组件，但是在定时器、Promise中，setState都是同步执行的，换句话说setState在React官方看来就该异步执行，所以把他们编写成异步，但是在其它非react控制的回调中，都是同步的。

（2）setState()的第一个参数，可以是对象，也可以是函数，如果是对象 合并更新一次状态, 以最后一次的值，为准，只调用一次render()更新界面。但是如果setState(fn)，里面传的是函数，会 更新多次状态, 但只调用一次render()更新界面  。另外，setState()可以传第二个参数,这个参数是一个回调函数，在回调函数里面我们可以得到最新得到state，因为它是在render之后执行。

```
 state = {
      count: 0,
    }

    componentDidMount() {
      this.setState({count: this.state.count + 1})
      this.setState({count: this.state.count + 1})
      console.log(this.state.count)  // 2=>0 //这里面是对象的形式，两次合并成了1次

      this.setState(state => ({count: state.count + 1}),()=>{
        console.log(this.state)//这个回调函数会在render之后执行，我们获取的是最新的state
      })
```

#### （四）props

react中数据的来源主要有两个方面，一个是内部的state 和 还有一个是外部传入过来的props 。state和Props主要的区别在于 **props** 是不可变的，而 state 可以根据与用户交互来改变。这就是为什么有些容器组件需要定义 state 来更新和修改数据。 而子组件只能通过 props 来传递数据。我们可以这样理解props和state：

```
props 是组件对外的接口，state 是组件对内的接口。**组件内可以引用其他组件，组件之间的引用形成了一个树状结构（组件树），如果下层组件需要使用上层组件的数据或方法，上层组件就可以通过下层组件的props属性进行传递，因此props是组件对外的接口。组件除了使用上层组件传递的数据外，自身也可能需要维护管理数据，这就是组件对内的接口state。根据对外接口props 和对内接口state，组件计算出对应界面的UI。
```

##### prop-types类型限定

具体步骤

```
import PropTypes from 'prop-types'
import React, { Component } from 'react'

import PropTypes from "prop-types";
import './categoryDetail.styl';

export default class CategoryDetail extends Component {
  static propTypes={
    rightCategorys:PropTypes.array
  }
  //传过来的属性叫做rightCategorys进行类型检查

  render() {
    return (
      <div className="category_detail">
        <img alt="WapBanner" />
        <ul>
          <li >
            <div>
              <img alt="logo" />
              <div>你好</div>
            </div>
          </li>
        </ul>
      </div>
    );
  }
}

```



