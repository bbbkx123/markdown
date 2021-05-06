```js
import * as actionTypes from './actionTypes'

const defaultState = {
  test: '123'
}

const reducer = (state = defaultState, action) => {
  switch (action.type) {
    case actionTypes.TEST: 
      let newState = Object.assign({}, state, {test: action.value})
      return newState
    default:
      return state
  }
}

export default reducer

```