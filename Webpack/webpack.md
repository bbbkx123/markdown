webpack性能优化的配置的总配置 https://juejin.cn/post/6903404018945654791#heading-10


## 什么是webpack？

webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。

## webpack有什么作用？
1. ```模块打包```
可以将不同的模块==整合==到一起， 并且保证它们之间的==引用正确==，有序执行。**利用打包我们可以自由划分模块，保证项目结构清晰和可读性**。

2. ```编译兼容```
使用 ```loader``` 对代码做 polyfill（支持新的特性，如api），还可以编译```less, vue, jsx```等浏览器无法识别的文件，使用新的特性提高开发效率。


3. ```能力扩展```
使用```plugin```机制，可以在模块打包和编译兼容的基础上，进行==按需加载==和==代码压缩==等一系列功能，提高工程效率和打包输出文件质量。

## webpack运行原理

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

## 有哪些常见的Loader？你用过哪些Loader？
* ```babel-loader``` image-loader
* ```babel-loader```
* ```babel-loader```
* ```babel-loader```
* ```babel-loader```
* ```babel-loader```
* ```babel-loader```






## 安装
生成 ```package.json```
```
npm init
```


安装 ```webpack```
```
npm install webpack --save-dev
npm install webpack-cli --save-dev
```

```webpack.config.js```
```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
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
```

## 使用
```
# 全局安装webpack后, entry file是入口文件
webpack {entry file}

# 非全局安装
node_modules/.bin/webpack {entry file}

# 使用npx(npx运行时, 会到node_modules/.bin路径和环境变量$PATH检查命令是否存在)
npx webpack {entry file}
```

## 更快捷的执行打包任务
在```package.json```中进行配置
```JSON
{
  "name": "webpack-sample-project",
  "version": "1.0.0",
  "description": "Sample webpack project",
  "scripts": {
    // 可以配置自定义命令
    "start": ""
  },
  "author": "zhang",
  "license": "ISC",
  "devDependencies": {
    "webpack": "3.10.0"
  }
}
```

## webpack功能

### 生成Source Maps（使调试更容易）
|         devtool选项          |                                                                                                                     配置结果                                                                                                                      |
| :--------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|          source-map          |                                                                       在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包速度；                                                                        |
|   cheap-module-source-map    |                                         在一个单独的文件中生成一个不带列映射的map，不带列映射提高了打包速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便；                                         |
|       eval-source-map        | 使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。在开发阶段这是一个非常好的选项，在生产阶段则一定不要启用这个选项； |
| cheap-module-eval-source-map |                                                这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点；                                                 |
配置如下:
```JS
module.exports = {
  devtool: 'eval-source-map',
  entry:  __dirname + "/app/main.js",
  output: {
  }
}
```

### 使用webpack构建本地服务器
**作用:** 让浏览器监听代码的修改，并自动刷新显示修改后的结果
**安装:**
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

### loader和plugin的区别
* loader是使webpack拥有加载和解析非js文件的能力
* plugin是扩展器， 针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点，执行广泛的任务。

### loader
loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图（和最终的 bundle）可以直接引用的模块。

配置如下：
```js
// test和use
let baseConfig = {
  entry: __dirname + '/app/main.js,
  output: {
    path: __dirname + '/dist',
    filename: 'bundle.js'
  },
  devtool: ...,
  module: {
    rules: [
      // 注意顺序，先style-loader，再css-loader
      // 单独使用了css-loader只能保证我们能引用css模块进来，但是并没有效果
      // 而style-loader就可以创建一个style标签，并且把引入进来的css样式都塞到这个标签里
      {
        test: /\.css$/
        use: [
          {loader: "style-loader"},
          {loader: "css-loader"}
        ]
      },
      {
        test: /\.less$/,
        use: [
          {loader: 'style-loader'},
          {loader: 'css-loader'},
          {loader: 'less-loader'}
        ],
        exclude: /node_modules/
      },
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: {
            loader: "file-loader",
            options: {
              name: "[name].[ext]",
              outputPath: "images/",
            },
          },
      }
      //...
    ]
  }
}
```
### babel
Babel其实是一个编译JavaScript的平台，它可以编译代码帮你达到以下目的：

* 让你能使用最新的JavaScript代码（ES6，ES7...），而不用管新标准是否被当前使用的浏览器完全支持；
* 让你能使用基于JavaScript进行了拓展的语言，比如React的JSX

### plugins
插件目的在于解决 loader 无法实现的其他事。

#### 性能分析
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

#### 自定义一个plugin
```js
class myPlugins {
  constructor (options) {
    this.options = options || {}
    this.filename = this.options.filename || 'fileList.md'
  }

  apply (compiler) {
    compiler.hooks.emit.tapAsync('myPlugins', (compliation, cb) => {
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
        cb()
      }, 2000)
    })
  }
}
module.exports = myPlugins

```

使用plugin
```JS
var myPlugins = require('./myPlugins')
var lessRules = {
    use: [
        {loader: 'css-loader'},
        {loader: 'less-loader'}
    ]
}

var baseConfig = {
  // ... 
  module: {
    rules: [
      // ...
      {test: /\.less$/, use: ExtractTextPlugin.extract(lessRules)}
    ]
  },
  plugins: [
    new myPlugins()
  ]
}
```
1. HtmlWebpackPlugin （打包生成html文件，并引入其他模块文件）
2. clean-webpack-plugin
