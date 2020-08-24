## 是什么
webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。

## 模块化
### 简单实现（早期）
```js
(function ModuleStudent (window) {
  let name = 'kiana'
  let sex = 'girl'
  function tell () {
    console.log(`我是${name},一个${sex}`)
  }
  window.ModuleStudent = {tell}
})(window)
```
### 优点
* 作用域封装
* 重用性
* 解除耦合


## 基本功能
1. 代码转换：TypeScript 编译成 JavaScript、SCSS 编译成 CSS 等等
2. 文件优化：压缩 JavaScript、CSS、HTML 代码，压缩合并图片等
3. 代码分割：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载
4. 模块合并：在采用模块化的项目有很多模块和文件，需要构建功能把模块分类合并成一个文件
5. 自动刷新：监听本地源代码的变化，自动构建，刷新浏览器
6. 代码校验：在代码被提交到仓库前需要检测代码是否符合规范，以及单元测试是否通过
7. 自动发布：更新完代码后，自动构建出线上发布代码并传输给发布系统。

## 构建过程
1. 从$\color{red}入口文件开始$，分析整个应用的依赖树
2. 把每个依赖模块包装起来（webpack是使用__webpack__require__先进行包装，然后加载模块），放入一个$\color{red}数组$等待调用
3. 实现模块加载的方法，并把它放到模块执行的环境中，确保模块间可以互相调用
4. 把$\color{red}执行入口文件的逻辑$放在一个函数表达式中，把$\color{red}模块数组$作为形参传入，并立即执行这个函数

## 安装
生成```package.json```
```
npm init
```
安装```webpack```
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
