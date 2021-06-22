## 什么是Loader?

本质就是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果。 因为 Webpack 只认识 JavaScript，所以 Loader 就成了翻译官，对其他类型的资源进行转译的预处理工作。

## 常见loader有哪些? 

* ```file-loader``` 把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
* ```url-loader``` 与 file-loader 类似，区别是用户可以设置一个阈值，==大于==阈值会交给 file-loader 处理，==小于==阈值时返回文件 ==base64== 形式编码 (处理图片和字体), url-loader依赖于file-loader
* ```babel-loader``` 把 ES6 转换成 ES5
* ```css-loader``` 将css文件变成commonjs模块加载到输出的js文件中，里面内容是样式字符串
* ```style-loader``` 创建style标签，将js中的样式资源插入进行，添加到head中生效
* ```less-loader``` 将less文件编译成css文件(scss-loader/sass-loader 类似)
* ```eslint-loader```  通过 ESLint 检查 JavaScript 代码
* ```vue-loader``` 编译vue文件


## 编写一个简单的loader

### loader的特点
* ```执行顺序``` 最后一个loader最先执行，第一个loader最后执行
* ```传入参数``` 上一个loader的返回值作为下一个loader的参数

### 编写原则
* ```单一原则``` 每个 Loader 只做一件事
* ```链式调用``` Webpack 会按顺序链式调用每个 Loader
* ```统一原则``` 遵循 Webpack 制定的设计规则和结构，输入与输出均为字符串，各个 Loader 完全独立，即插即用

需求：将.txt后缀的文件中的内容倒叙并大写。

```js
// ...进行loader配置,
  module:{
    rules: [
      {
        test: /\.txt$/,
        // 注意loader使用顺序
        use: ['./loaders/loader2.js', './loaders/loader1.js']
      }
    ]
  }
// loader1
module.exports = function (content) {
  return content.split('').reverse().join('')
}

// loader2

module.exports = function (content) {
  return `module.exports = '${content.toUpperCase()}'` 
}

```

## 