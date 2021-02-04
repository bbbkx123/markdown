# 1. 什么是虚拟 DOM
用 JavaScript 对象描述真实的 DOM 结构，就是虚拟 DOM。

# 2. 为什么要使用虚拟 DOM？
原因1: DOM 操作太慢了。这里的慢有两方面，一是性能低，页面慢。二是手动操作 DOM，效率低，从而导致开发节奏慢.

原因2: 真实 DOM 与浏览器强相关，而虚拟 DOM 本质上是 JavaScript 对象，JavaScript 对象能够更方便地进行跨平台操作。如服务端渲染，移动端开发等等.

# 3. 虚拟 DOM 实现

## 项目创建
使用 ``` Parcel Bundler ```, 一个无需配置的小型项目打包器。安装后可以启动一个 dev-server，并具有热更新功能。

创建```index.html```

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="root"></div>
  <script src="./main.js"></script>
</body>
</html>
```

创建 ``` main.js```

```js
import h from "./createElement";
import render from "./render";
import mount from "./mount";

const node = h(
  "div", 
  { id: "div1" }, 
  [
  h("span", { id: "span1" }, "kiana"),
  h("span", { id: "span2" }, "rita"),
]);

let ele = render(node);

mount(ele, document.getElementById("root"));
```

创建 ```createElement.js ```
```js
// 创建虚拟dom, 用js对象进行描述
export default (tagName, props, children) => {
  let vElement = Object.create(null)
  Object.assign(vElement, {
    tagName,
    props,
    children
  })
  return vElement
}
```


创建 ``` render.js```
```js
/**
 * 
 * @param {object} vNode 虚拟dom 
 */
const render = (vNode) => {
  let {tagName, props, children} = vNode
  if (typeof vNode === 'string') return document.createTextNode(vNode)
  // 创建对应标签
  let $el = document.createElement(tagName)

  // 添加属性
  for (let key in props) {
    $el.setAttribute(key, props[key])
  }

  // 添加子元素, 递归进行
  for (let child of children) {
    $el.appendChild(render(child))
  }

  return $el
}

export default render
```


创建 ``` mount.js```
```js
export default ($el, $target) => {
  return $target.appendChild($el)
}
```
