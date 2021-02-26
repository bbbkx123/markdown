# 出现问题
1. 使用```koa-static```后依然无法访问静态文件

发现与执行顺序有关:  

```js
// 需要放在最前面
app.use(static(path(__dirname, 'public')))

app.use((ctx) => {
  // ....
})

app.use(router.routes(), router.allowedMethods())

```
