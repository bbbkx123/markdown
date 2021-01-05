```js


import React, {Component} from 'react'
import 'antd/dist/antd.css'
import {Input, Button, List} from 'antd'
import store from './store'
import * as actionCreator from './store/actionCreator'

class App extends Component {
  constructor (props) {
    super(props)
    this.state = store.getState()
    this.changeValue = this.changeValue.bind(this)
    this.storeChange = this.storeChange.bind(this)
    this.buttonChange = this.buttonChange.bind(this)
    this.removeItem = this.removeItem.bind(this)
    // 订阅
    store.subscribe(this.storeChange)
  }

  render () {
    return (
      <div style={{margin: '10px'}}>
        <div>
          <Input style={{width: '250px', marginRight: '10px'}} placeholder={this.state.inputValue} onChange={this.changeValue} value={this.state.inputValue}></Input>
          <Button type="primary" onClick={this.buttonChange}>增加</Button>
        </div>
        <div style={{'margin': '10px', width: '300px'}}>
          <List bordered dataSource={this.state.list} 
            renderItem={(item, index) => (<List.Item onClick={() => this.removeItem(index)}>{item}</List.Item>)}>
          </List>
        </div>
      </div> 
    )
  }

  storeChange () {
    this.setState(store.getState())
  }

  changeValue (e) {
    const action = actionCreator.changeInputAction(e.target.value)
    store.dispatch(action)
  }

  buttonChange () {
    const action = actionCreator.buttonChangeAction(this.state.inputValue)
    store.dispatch(action)
  }

  removeItem (index) {
    const action = actionCreator.removeItemAction(index)
    store.dispatch(action)
  }
}

export default App
```