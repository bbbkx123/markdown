```js
import * as actionTypes from './actionTypes'

const actionCreator = {
  testAction (value) {
    return {
      type: actionTypes.TEST,
      value
    }
  }
}

export default actionCreator

```