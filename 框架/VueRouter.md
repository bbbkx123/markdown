# Vue-Router
**作用:** 建立并管理url和对应组件之间的映射关系(组件之间的切换)

## 路由组件传参
### 布尔模式
如果```props``` 被设置为 ```true``` ，```route.params``` 将会被设置为组件属性.

### 对象模式
如果 ```props``` 是一个对象，它会被按原样设置为组件属性。当 ```props``` 是$\color{red}静态$的时候有用。

```js
// 依赖于$route（耦合）
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ] 
})

// new Vue({router, ...})

// 设置props来进行解耦
const User1 = {
  props: ['id'],
  template: '<div>User1 {{ id }}</div>'
}
const router1 = new VueRouter({
  routes: [
    // boolean模式
    { path: '/user1/:id', component: User1, props: true },
    // 函数模式用于变化数据结构
    // {
    //   path: '/user1/:id',
    //   components: User1,
    //   // 
    //   props: (route) => ({id: route.params.id}) 
    // },
    // 对象模式传递静态值
    //  { path: '/promotion/from-newsletter', component: Promotion, props: { newsletterPopup: false } }
  ]
})

```

## 编程式导航
```js

// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
router.push({ path: `/user/${userId}` }) // -> /user/123

```

## 导航守卫

### 全局前置守卫
```beforeEach```

```js
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) {
    next({ name: 'Login' })
  } else next()
})
```

### 全局后置守卫
```router.afterEach((to, from) => {})```

### 路由独享守卫
```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

### 组件内守卫
```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```
beforeRouteEnter 守卫 不能 访问 this，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 next来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。
```js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```
注意 beforeRouteEnter 是支持给 next 传递回调的唯一守卫。对于 beforeRouteUpdate 和 beforeRouteLeave 来说，this 已经可用了，所以不支持传递回调，因为没有必要了。
```js
beforeRouteUpdate (to, from, next) {
  // just use `this`
  this.name = to.params.name
  next()
}
```
这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 next(false) 来取消。
```js
beforeRouteLeave (to, from, next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```
### 完整的导航解析流程
![](图片/VueRouter流程.png)

全局钩子函数2个，组件内钩子函数3个， 路由配置中钩子2个

1. ```beforeRouteLeave``` （组件内）离开该组件的对应路由时调用
2. ```beforeEach``` （全局）进行路由前
3. ```beforeRouteUpdate``` （组件内）组件被复用时调用
4. ```beforeEnter``` （路由配置）进行路由前
5. ```beforeRouteEnter``` （组件内）组件实例还没被创建,不能访问this
6. ```beforeResolve``` （路由配置）
7. ```afterEach``` （全局）
8. ```beforeRouteEnter``` ===> next() 可以访问this