webpack性能优化的配置的总配置 https://juejin.cn/post/6903404018945654791#heading-10

重学webpack(开发环境的配置) https://juejin.cn/post/6902761903605415950#heading-10

webpack基础 https://juejin.cn/post/6844904094281236487#heading-16


## 什么是webpack？

webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。

## webpack有什么作用？
1. ```模块打包```
可以将不同的模块==整合==到一起， 并且保证它们之间的==引用正确==，有序执行。**利用打包我们可以自由划分模块，保证项目结构清晰和可读性**。

2. ```编译兼容```
使用 ```loader``` 对代码做 polyfill（支持新的特性，如api），还可以编译```less, vue, jsx```等浏览器无法识别的文件，使用新的特性提高开发效率。

3. ```能力扩展```
使用```plugin```机制，可以在模块打包和编译兼容的基础上，进行==按需加载==和==代码压缩==等一系列功能，提高工程效率和打包输出文件质量。

## webpack打包原理
Webpack 实际上为每个模块创造了一个可以==导出==和==导入==的环境，本质上并没有修改 代码的执行逻辑，代码执行顺序与模块加载顺序也完全一致。

## webpack运行过程

* ```初始化参数``` 读取 ```webpack```配置参数。
* ``` 开始编译``` 通过读取的参数初始化```Compiler```对象，加载所有配置的插件(plugin)，Compiler的run方法开始执行。
* ``` 确定入口``` 通过```entry```配置找出所有入口文件。
* ```模块编译``` 从入口文件出发，使用配置的```loader```进行编译，再找出依赖的模块，使用```loader```进行编译， 递归进行处理。
* ``` 完成模块编译``` 上一步全部完成后，得到每个模块被编译后的最终内容和依赖关系。
* ``` 输出资源``` 根据入口文件和依赖关系，组成==一个或者多个包含模块(module)集合==的chunk， 再把每个chunk转换成单独文件加入到输出列表，这是修改输出内容的最后机会。
* ``` 输出完成``` 根据配置输出路径和文件名，输出到文件系统。

对于chunk的理解
* chunk是一堆模块（module）的集合
* 一个entry生成一个chunk
* chunk是webpack运行过程的代码快， bundle是结果的代码快。

## 安装
1. 使用 ```npm init``` 生成 ```package.json```
2. 安装 ```webpack```
```
npm install webpack,webpack-cli --save-dev
```

## 使用
webpack模板配置(五大核心配置)

```js
// webpack.config.js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
  mode: 'development',  
  entry: './main.js',
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  devServer: {
    port: 3000,
    inline: true
  },
  plugins: [
    new HtmlWebpackPlugin()
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {loader: "style-loader"},
          {loader: "css-loader"}
        ]
      }
    ]
  }
}

// 执行webpack
// 全局安装webpack后, entry file是入口文件
// webpack {entry file}

// 非全局安装
// node_modules/.bin/webpack {entry file}

// 使用npx(npx运行时, 会到node_modules/.bin路径和环境变量$PATH检查命令是否存在)
// npx webpack {entry file}
```

## webpack功能

### Source Maps
sourceMap是一项将编译、打包、压缩后的代码映射回源代码的技术

|         devtool选项          |                                                                                                                     配置结果                                                                                                                      |
| :--------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|          source-map          |                                                                       在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包速度；                                                                        |
|   cheap-module-source-map    |                                         在一个单独的文件中生成一个不带列映射的map，不带列映射提高了打包速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便；                                         |
|       eval-source-map        | 使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。在开发阶段这是一个非常好的选项，在生产阶段则一定不要启用这个选项； |
| cheap-module-eval-source-map |                                                这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点；                                                 |

### 使用webpack构建本地服务器
> 让浏览器监听代码的修改，并自动刷新显示修改后的结果


#### 安装
```
npm install webpack-dev-server --save-dev
```

| devserver的配置选项 |                                                        功能描述                                                        |
| :-----------------: | :--------------------------------------------------------------------------------------------------------------------: |
|     contentBase     | 默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录 |
|        port         |                                         设置默认监听端口，如果省略，默认为8080                                         |
|       inline        |                                        设置为true，当源文件改变时会自动刷新页面                                        |
| historyApiFallback  |            在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html             |
配置如下:
```JS
module.exports = {
  devtool: 'eval-source-map',
  entry:  __dirname + "/app/main.js",
  output: {},
  devServer: {
    contentBase: "./public",//本地服务器所加载的页面所在的目录
    historyApiFallback: true,//不跳转
    inline: true//实时刷新
  } 
}
```

### 说一下 Webpack 的热更新原理吧
```Webpack``` 的热更新又称热替换（Hot Module Replacement），缩写为 HMR。 这个机制可以做到不用刷新浏览器而将新变更的模块替换掉旧的模块。

HMR的核心就是客户端从服务端拉去更新后的文件，准确的说是 ```chunk diff```(chunk 需要更新的部分)，实际上 WDS(webpack dev server) 与浏览器之间维护了一个 ```Websocket```，当本地资源发生变化时，WDS 会向浏览器推送更新，并带上构建时的 ```hash```，让客户端与上一次资源进行对比。客户端对比出差异后会向 WDS 发起``` Ajax ```请求来获取更改内容(文件列表、hash)，这样客户端就可以再借助这些信息继续向 WDS 发起 ```jsonp``` 请求获取该chunk的增量更新。

后续的部分(拿到增量更新之后如何处理？哪些状态该保留？哪些又需要更新？)由 ```HotModulePlugin``` 来完成，提供了相关 API 以供开发者针对自身场景进行处理，像```react-hot-loader``` 和 ```vue-loader``` 都是借助这些 API 实现 HMR。


### 如何对bundle体积进行监控和分析
```js
// npm install webpack-bundle-analyzer --save-dev
const BundleAnalyzerPlugin = require("webpack-bundle-analyzer").BundleAnalyzerPlugin;

{
  ...,
  plugins: [
    new BundleAnalyzerPlugin()
  ]
}
```

### 文件指纹

### 如何优化 Webpack 的构建速度？

### 那代码分割的本质是什么？有什么意义呢？有哪些方式？

常用代码分割的方法
* ```Entry Points``` 入口文件设置的时候可以配置, 当多入口文件包含重复模块时, 重复模块会分别引入到bundle找那个
* ```CommonsChunkPlugin``` 这个插件可以抽取所有入口文件都依赖了的模块，把这些模块抽取成一个新的bundle, 解决了 Entry Points 出现的问题
* ```Dynamic Imports``` 动态引入 ???


代码分割的本质其实就是在源代码直接上线和打包成唯一脚本main.bundle.js这两种极端方案之间的一种更适合实际场景的中间状态。

源代码直接上线：虽然过程可控，但是http请求多，性能开销大。

打包成唯一脚本：一把梭完自己爽，服务器压力小，但是页面空白期长，用户体验不好。