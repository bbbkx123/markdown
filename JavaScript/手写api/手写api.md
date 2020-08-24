## call
```js
Function.prototype.mycall = function () {
  if (typeof this !== 'function') {
      throw new Error('not function')
  }
  let self = this
  let args = [...arguments]
  let context = args.shift()
  context.fn = self
  let res = context.fn(...args)
  delete context.fn
  return res
}
```

## apply
```js
 Function.prototype.apply1 = function () {
  if (typeof this !== 'function') {
      throw new Error('not function')
  }
  let self = this
  let args = [...arguments]
  let context = args.shift()
  context.fn = self
  let res = context.fn(...args[0])
  delete context.fn
  return res
}
```


## bind
```js
Function.prototype.mybind = function () {
  if (typeof this !== 'function') {
    throw new Error('not function')
    return
  }
  let self = this
  let args = [...arguments]
    // let args = Array.prototype.slice.call(arguments)
  let context = args.shift()
  let F = new Function()
  let funBind = function () {
    return self.apply(this instanceof funBind ? this : context, args.concat([...arguments]))
  }
  F.prototype = self.prototype
  funBind.prototype = new F()
  // funBind.prototype = Object.create(self.prototype)
  funBind.prototype.constructor = funBind
  return funBind
}
```

## new
```js
function mynew (constructor, ...args) {
  if (typeof constructor !== 'function') {
   throw new Error('not function')
   return
  }
  let target = {}
  target.__proto__ = constructor.prototype
  // let target = Object.create(constructor.prototype)
  let res = constructor.call(target, ...args)
  if (typeof res === 'object' || typeof res === 'function') {
    return res
  }
  return target
}
```

## instanceof
```js
function myinstanceof (object, constructor) {
  let left = object.__proto__
  let right = constructor.prototype
  while (true) {
    if (left === null) {
      return false
    }
    if (left === right) {
      return true
    }
    left = left.__proto__
  }
}

// 测试
function Foo() {}

console.log(Object instanceof Object, myinstanceof(Object, Object))
console.log(Function instanceof Function, myinstanceof(Function, Function))
console.log(Function instanceof Object, myinstanceof(Function, Object))
console.log(Foo instanceof Foo, myinstanceof(Foo, Foo))
console.log(Foo instanceof Object, myinstanceof(Foo, Object))
console.log(Foo instanceof Function, myinstanceof(Foo, Function))

```

## Object.create
```js
function myObjCreate (funcPrototype) {
  let target = {}
  target.__proto__ = funcPrototype

  let F = new Function()
  F.prototype = funcPrototype

  return new F()
}

// 测试
function Super () {}
Super.prototype.name = 'kiana'

let sup1 = Object.create(Super.prototype)
console.log(sup1)

let sup2 = myObjCreate(Super.prototype)
console.log(sup2)

```

## 深拷贝
**什么是浅拷贝：**
> 创建一个新对象，基本类型拷贝后就是基本类型，如果是引用类型，则拷贝对象的内存地址，其中一个对象发生变化会影响另外一个对象

**什么是深拷贝：**
> 将一个对象从内存中完整的拷贝出来一份，从堆内存中开辟一个新的区域存放新对象，修改对象不会影响原对象
```js
function deepClone (target, map = new Map()) {
  if (target instanceof Date) {
    return new Date(target)
  } else if (target instanceof RegExp) {
    return new RegExp(target)
  } else if (typeof target !== 'object') {
    return target
  }
  if (value = map.get(target)) {
    return value
  }
  let tempObj = Array.isArray(target) ? [] : {}
  map.set(target, tempObj)
  Object.keys(target).forEach((key, index) => {
    tempObj[key] = deepClone(target[key], map)
  })
  return tempObj
}

const obj = {
  arr: [111, 222],
  obj: {key: '对象'},
  a: () => {console.log('函数')},
  date: new Date(),
  b: null
}
obj.obj1 = obj

console.log(deepClone(obj), obj)
```