```js
import * as React from 'react'
import {connect} from 'react-redux'
import actionCreator from '../../store/actionCreator'

// redux
// this.state = store.getState()
// store.subscribe(() => this.setState(store.getState()))

const AppTemplate = (props) => {
  return (
    <div>
      <div>{props.test}</div>
      <button onClick={() => props.submit(props.test)}>submit</button>
    </div>
  )
}

const stateToProps = (state) => {
  return {
    test: state.test
  }
}

const dispatchToProps = (dispatch) => {
  return {
    submit (value) {
      const action = actionCreator.testAction(22222222222222)
      dispatch(action)
    }
  }
}

// 映射到props

const App = connect(stateToProps, dispatchToProps)(AppTemplate)
export default App

```