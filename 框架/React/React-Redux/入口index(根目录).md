```js
import React from 'react'
import ReactDOM from 'react-dom'
import {Provider} from 'react-redux'
import Main from './view/Main'
import store from './store'

ReactDOM.render(
  <Provider store={store}>
    <Main></Main>
  </Provider>, 
  document.getElementById('root'))

```