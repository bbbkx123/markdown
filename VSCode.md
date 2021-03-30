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