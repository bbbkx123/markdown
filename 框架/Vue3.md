# 新增API

## defineCompomnent
```js
export default defineComponment({
  props: {},
  setup() {},
  ...
})
```

## setup
```Compostion API``` 入口函数

```js
  // ctx包含 slots, emit, attrs
  setup(props, ctx) {
    // code...

    // 将数据暴露到模板template
    return {}
  }
```

## ref
创建响应式的数据, 一般用于创建原始类型, 也可以创建引用类型; 访问值需要通过```.value```
```js
  setup(props, ctx) {
    let name = ref("kiana")
    let age = ref(18)
    return {name, age}
  }

```

## reactive 
创建响应式的数据对象, 一般用于创建引用类型

```js
  setup(props, ctx) {
    let state = reactive({
      name: "kiana",
      age: 18
    })
    return {state}
  }
```

## unref 
如果参数是```ref```, 则返回它的```value```

## toRefs
把一个响应式对象转换成普通对象

```js
  setup(props, ctx) {
    let state = reactive({
      name: "kiana",
      age: 18
    })
    return {toRefs(state)}
  }
```


## computed

```js
// 只读的计算属性
setup () {
  let name = ref("kiana")
  computed(() => {
    console.log(`I am ${name}`)
  })
}

// 可读可写的计算属性
setup () {
  let name = ref("kiana")
  computed({
    get: () => {
      console.log(`I am ${name}`)
    },
    set: (val, oldVal) => {
      console.log(`I am ${name}`)
    }
  })
}

```

## watchEffect
监测副作用函数。立即执行传入的一个函数，并响应式追踪其依赖，并在其依赖变更时重新运行该函数
```js
setup () {
  let name = ref("kiana")
  let stop = watchEffect(() => {
    console.log(`hello, ${name}`)
  })

  stop()
}
```


## watch
https://segmentfault.com/a/1190000022734935?utm_source=tag-newest
https://mp.weixin.qq.com/s/xt5iFGfz_54Z4txMDserdA