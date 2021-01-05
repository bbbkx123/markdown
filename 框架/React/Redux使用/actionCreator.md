```js
import * as types from './actionTypes'

export const changeInputAction = (value) => ({type: types.CHANGE_VALUE, value}) 

export const buttonChangeAction = (value) => ({type: types.BUTTON_CHANGE, value})

export const removeItemAction = (index) => ({type: types.REMOVE_ITEM, index})

```