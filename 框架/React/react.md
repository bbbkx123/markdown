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

* 父组件向子组件通信(props)
* 子组件向父组件通信(传递回调函数)
* 跨级组件通信(Provider, Consumer)
* 没有嵌套关系组件之间的通信(emit)

### 跨级组件通信

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
### 没有嵌套关系组件之间的通信

```js
import {EventEmitter} from 'events'

const Emitter = new EventEmitter()

// 订阅
Emitter.addListener('my-event', (param) => {
  // ......
})

// 发布
Emitter.emit('my-event', 123)

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


## react生命周期
``` static getDerivedFromProps  ```   
会在调用 render 方法之前$\color{red}调用$，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 state，如果返回 null 则不更新任何内容

<br>

``` shouldComponentUpdate ```  
会在渲染执行之前被调用。返回值默认为 true

<br>

``` componentDidUpdate ```
如果你需要执行副作用（例如，数据提取或动画）以响应 props 中的更改

!['Reactv16.0前的生命周期'](./图片/Reactv16.0前的生命周期.jpg)


!['Reactv16.0前的生命周期'](./图片/Reactv16.4+的生命周期图.png)