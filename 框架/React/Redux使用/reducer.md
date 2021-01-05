# 为什么要创建副本state
在redux-devtools中,我们可以查看到redux下所有通过reducer更新state的记录,每一个记录都对应着内存中某一个具体的state,让用户可以追溯到每一次历史操作产生与执行时,当时的具体状态,这也是使用redux管理状态的重要优势之一.

若不创建副本,redux的所有操作都将指向内存中的同一个state,我们将无从获取每一次操作前后,state的具体状态与改变,若没有副本,redux-devtools列表里所有的state都将被最后一次操作的结果所取代.我们将无法追溯state变更的历史记录.

创建副本也是为了保证向下传入的this.props与nextProps能得到正确的值,以便我们能够利用前后props的改变情况以决定如何render组件

```js
import * as types from './actionTypes'

const defaultState = {
  inputValue: 'sss',
  list: [
    'kiana',
    'rita',
    'bronya'
  ]
}

export default (state = defaultState, action) => {

  // Reducer里不能改变state, 只能store才能改变state
  if (action.type === types.CHANGE_VALUE) {
    // Object.assign
    return Object.assign({}, state, {inputValue: action.value})

    // 对象展开运算符
    // return {
    //   ...state,
    //   inputValue: action.value
    // }

    // lodash库
    // let newState = cloneDeep(state)
    // newState.inputValue = action.value
    // return newState
  }

  if (action.type === types.BUTTON_CHANGE) {
    const list = JSON.parse(JSON.stringify(state.list))
    list.push(action.value)
    return Object.assign({}, state, {list})
  }

  if (action.type === types.REMOVE_ITEM) {
    const list = JSON.parse(JSON.stringify(state.list))
    list.splice(action.index, 1)
    return Object.assign({}, state, {list})
  }

  return state
}

```