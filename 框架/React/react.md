# React

## React和ReactDOM
> React只做逻辑层, ReactDOM去渲染真实的DOM

```js 
import React from 'react'
import ReactDOM from 'react-dom'
// 任意组件
import App from './App.jsx'

// index.html 对应dom节点
ReactDOM.render(<App />, document.querySelector('#root'))

```

## state
> 组件状态

### 设置state
```js
class Test extends React.Component {
  // 非必要
  constructor (props) {
    super(props)
    // 设置state
    this.state = {
      data: null
    }
  }

  // 必要
  render () {
    return (
      <div>kiana</div>
    )
  }
}

```

## 组件通信

### 父传子
父组件通过行间属性传递数据到子组件,子组件通过实例上的props属性接收新的数据
React 的数据是单向数据流，只能一层一层往下传递。当组件的属性发生改变，那么当前的视图就会更新

### 子传父
父组件传递一个function到子组件, 子组件传入参数进行调用

### 跨级父传子
> context

```js
// 父组件
import React from 'react'

import ChildTest from './'

const {Consumer, Provider} = React.createContext({name: null})

class Test extends React.Component {
  // 非必要
  constructor (props) {
    super(props)
    // 设置state
    this.state = {
      name: null
    }
  }

  // 必要
  render () {
    return (
      <Provider value={name: 'rita'}>
        <ChildTest />
      </Provider>
    )
  }
}

export {
  Test,
  Consumer
}
// --------------------------------------------------
// 子组件

import {Consumer} from './父组件'

let context = React.createContext({name: null})

class Test extends React.Component {
  // 非必要
  constructor (props) {
    super(props)
    // 设置state
    this.state = {
      name: null
    }
  }

  // 必要
  render () {
    return (
      <Consumerr >
        {
          value => (
            <div>{value.name}</div>
          )
        }
      </Consumer>
    )
  }
}
```


## ref和refs
在React中,类似于Vue,可以通过```ref```标记元素,然后通过```this.refs```获取元素,只是Vue中使用的是```this.$refs```获取元素

### 字符串写法
通过设置一个字符串值来标记元素,然后通过这个字符串作为属性获取元素

```js
class Input extends Component {
  handleChange = (e) => {
   console.log(this.refs.a.value)
  }
  render() {
    return (
      <div>
        <input type="text" ref="a" onChange={this.handleChange} />
      </div>
    )
  }
}
```

### 函数写法
函数作为一个ref的属性值,这个函数接受一个参数,就是真实的DOM元素

可以把这个元素挂载到实例上,方便后面的操作

```js
class Input extends React.Component {
    
    componentDidMount() {
        console.log(this.a); //获取真实的DOM元素
    }
    render() {
        return (
            <div>
               <input type="text" ref={x=>this.a = x}/> 
            </div>
        );
    }
}
```