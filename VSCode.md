# 调试配置 launch.js

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      // 启动配置的名称。每个程序可能有多个启动配置// 此名称将显示在调试面板的下拉框中
      "name": "Launch",
      // 配置的类型，默认有三种类型(node,momo,extensionHost)// 可以通过插件来自定义更多的类型
      "type": "node",
      // 请求类型, launch表示启动程序调试，attach表示监听某一端口进行调试
      "request": "launch",
      // node程序的入口脚本路径(相对于工作空间)
      "program": "./out/bootstrap.js",
      "stopOnEntry": false,
      // 接在program指定js后面的参数
      "args": [],
      // 程序的启动位置
      "cwd": ".",
      // 启动程序的路径, 如果为null则使用默认的node
      "runtimeExecutable": null,
      // 传递给node的参数
      "runtimeArgs": ["--nolazy", "--es_staging", "--harmony-proxies"],
      // 程序启动时设置的环境变量
      "env": { "NODE_ENV": "development" },
      "externalConsole": false,
      // 是否使用sourceMaps
      "sourceMaps": true,
      // 如果使用sourceMaps，js脚本所在的路径
      "outDir": "./out"
    },
    {
      "name": "Attach",
      "type": "node",
      // attach表示监听某一端口进行调试
      "request": "attach",
      // 要监听的端口
      "port": 5858,
      // 是否使用sourceMaps
      "sourceMaps": true,
      // 如果使用sourceMaps，js脚本所在的路径
      "outDir": "./out"
    }
  ]
}
```

eslint配置

```json
// 开启eslint校验, 需安装eslint
{
  "eslint.autoFixOnSave": true,  //  启用保存时自动修复,默认只支持.js文件
    "eslint.validate": [
        "javascript",  //  用eslint的规则检测js文件
        {
          "language": "vue",   // 检测vue文件
          "autoFix": true   //  为vue文件开启保存自动修复的功能
        },
        {
          "language": "html",
          "autoFix": true
        },
    ],
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    // 检索过滤
    "search.exclude": {
        "**/node_modules": true,
        "**/bower_components": true,
        "**/dist": true
    },
}

```

Prettier 配置

```json
{
    // 使能每一种语言默认格式化规则
    "[html]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[css]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[less]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },

    /*  prettier的配置 */
    "prettier.printWidth": 100, // 超过最大值换行
    "prettier.tabWidth": 4, // 缩进字节数
    "prettier.useTabs": false, // 缩进不使用tab，使用空格
    "prettier.semi": true, // 句尾添加分号
    "prettier.singleQuote": true, // 使用单引号代替双引号
    "prettier.proseWrap": "preserve", // 默认值。因为使用了一些折行敏感型的渲染器（如GitHub comment）而按照markdown文本样式进行折行
    "prettier.arrowParens": "avoid", //  (x) => {} 箭头函数参数只有一个时是否要有小括号。avoid：省略括号
    "prettier.bracketSpacing": true, // 在对象，数组括号与文字之间加空格 "{ foo: bar }"
    "prettier.disableLanguages": ["vue"], // 不格式化vue文件，vue文件的格式化单独设置
    "prettier.endOfLine": "auto", // 结尾是 \n \r \n\r auto
    "prettier.eslintIntegration": false, //不让prettier使用eslint的代码格式进行校验
    "prettier.htmlWhitespaceSensitivity": "ignore",
    "prettier.ignorePath": ".prettierignore", // 不使用prettier格式化的文件填写在项目的.prettierignore文件中
    "prettier.jsxBracketSameLine": false, // 在jsx中把'>' 是否单独放一行
    "prettier.jsxSingleQuote": false, // 在jsx中使用单引号代替双引号
    "prettier.parser": "babylon", // 格式化的解析器，默认是babylon
    "prettier.requireConfig": false, // Require a 'prettierconfig' to format prettier
    "prettier.stylelintIntegration": false, //不让prettier使用stylelint的代码格式进行校验
    "prettier.trailingComma": "es5", // 在对象或数组最后一个元素后面是否加逗号（在ES5中加尾逗号）
    "prettier.tslintIntegration": false // 不让prettier使用tslint的代码格式进行校验
}
```