# 配置webpack

## 使用 ``` npm run eject```
不推荐使用  
1. 该命令是不可逆的。也就是一旦执行了此命令。webpack.config.js 文件就永久的暴露出来
2. 如果只是修改一个很小的配置项, 不使用也能够配置 webpack

## 使用 react-app-rewired 和  customize-cra  

1. 用 ```react-app-rewired``` 命令代替 ```react-scripts```  

```json
 "scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-app-rewired eject"
  },
```

2. ```customize-cra``` 

```js
// config-overrides.js 中
const {
  override,
  // antd-mobile按需引入 (配置babel-plugin-import)
  fixBabelImports,
  // 使用ES7的装饰器
  addDecoratorsLegacy,
  // 安装less和less-loader：npm i less less-loader --save-dev
  addLessLoader,
  // 添加别名
  addWebpackAlias,
} = require("customize-cra");
const path = require("path");

module.exports = override(
  fixBabelImports("import", {
    libraryName: "antd",
    libraryDirectory: "es",
    style: true,
  }),

  addWebpackAlias({
    "@": path.resolve(__dirname, "./src")
  }),

  // addDecoratorsLegacy(),

  addLessLoader({
    javascriptEnabled: true,
    modifyVars: {
      '@primary-color': '#1DA57A'
    }
  })
);
```

3. 根目录添加文件 ```paths.json```

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

4.  ```tsconfig.json``` 中添加 ```"extends": "./paths.json",```