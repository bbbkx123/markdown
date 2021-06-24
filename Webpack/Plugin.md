## 什么是Plugin?
插件是 webpack 生态系统的重要组成部分, 插件能够 钩入(hook) 到在每个编译(compilation)中触发的所有关键事件。在编译的每一步，插件都具备完全访问``` compiler``` 对象的能力，如果情况合适，还可以访问当前 compilation 对象。

## 常用Plugin有哪些?
* ```ignore-plugin```：忽略部分文件
* ```html-webpack-plugin```：简化 HTML 文件创建 (依赖于 html-loader)
* ```web-webpack-plugin```：可方便地为单页应用输出 HTML，比 html-webpack-plugin 好用
* ```clean-webpack-plugin```: 目录清理
* ```webpack-bundle-analyzer```: 可视化 Webpack 输出文件的体积 (业务组件、依赖第三方模块)
* ```mini-css-extract-plugin```: 分离样式文件，CSS 提取为独立文件，支持按需加载 (替代extract-text-webpack-plugin)
* ```hot-module-plugin```

https://juejin.cn/post/6844904094281236487#heading-2

https://juejin.cn/post/6943468761575849992#heading-5

## 编写一个简单的Plugin

* ```compiler对象``` 暴露了和 Webpack 整个生命周期相关的钩子(如emit, watchRun等)
* ```compilation对象``` 暴露了与模块和依赖有关的粒度更小的事件钩子
* ```同一引用``` 传给每个插件的 compiler 和 compilation对象都是同一个引用，若在一个插件中修改了它们身上的属性，会影响后面的插件
* ```异步事件处理``` 异步的事件需要在插件处理完任务时==调用回调函数==通知 Webpack 进入下一个流程，不然会卡住
* ```apply``` 插件需要在其原型上绑定apply方法，才能访问 compiler 实例


### Tapable
webpack 自己实现了一套```Tapable```事件流方案, 提供了```tap```, ```tapAsync```, ```taoPromise```, 使用这些方法，注入自定义的构建步骤，这些步骤将在整个编译过程中==不同==时机触发

* 当钩入 compile 阶段时, 只能使用同步的 tap 方法

```js
compiler.hooks.compile.tap('MyPlugin', params => {
  console.log('以同步方式触及 compile 钩子。')
})
```

* 然而，对于能够使用了 AsyncHook(异步钩子) 的 run，我们可以使用 tapAsync 或 tapPromise（以及 tap）：

```js
compiler.hooks.run.tapAsync('MyPlugin', (compiler, callback) => {
  console.log('以异步方式触及 run 钩子。')
  callback()
})

compiler.hooks.run.tapPromise('MyPlugin', compiler => {
  return new Promise(resolve => setTimeout(resolve, 1000)).then(() => {
    console.log('以具有延迟的异步方式触及 run 钩子')
  })
})
```

### 常用的hook, 实际应用场景
emit, watchRun (暂时不知道实际应用场景)???

### 编写
```js
// 获取文件个数, 文件名字

class myPlugins {
  constructor (options) {
    this.options = options || {}
    this.filename = this.options.filename || 'fileList.md'
  }

  apply (compiler) {
    compiler.hooks.emit.tapAsync('myPlugins', (compliation, callback) => {
      setTimeout(() => {
        const fileListName = this.filename
        const len = Object.keys(compliation.assets).length
        let content = `# 一共有${len}个文件\n\n`
        for (let filename in compliation.assets) {
          content += `- ${filename}\n`
        }
        compliation.assets[fileListName] = {
          source () {
            return content
          },
          size () {
            return content.length
          }
        }
        // 异步事件需要调用callback通知 Webpack 进入下一个流程，不然会卡住
        callback()
      }, 2000)
    })
  }
}
module.exports = myPlugins


// 使用Plugin  new myPlugins()

```


compiler 对象代表的是不变的webpack环境，是针对webpack的
compilation 对象针对的是随时可变的项目文件，只要文件有改动，compilation就会被重新创建


