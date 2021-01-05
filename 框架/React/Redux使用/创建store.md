```js
import {createStore} from 'redux'
import reducer from  './reducer.js'

const store = createStore(
    reducer, 
    // 插件 React Dev Tools  调试redux
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
)

export default store


```