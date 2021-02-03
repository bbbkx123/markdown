## API
| 方法                            | 适用范围                       | 描述                                                   |
| ------------------------------- | ------------------------------ | ------------------------------------------------------ |
| `for in `                       | 数组, 对象                     | **获取可枚举的实例属性和原型属性, 所以不适合遍历数组** |
| `for of `                       | 可迭代对象(有 iterator 的对象) |                                                        |
| `Object.keys() `                | 数组, 对象                     | **返回可枚举的实例属性名组成的数组**                   |
| `Object.getOwnPropertyNames() ` | 数组, 对象                     | **返回除原型属性以外的所有属性名组成的数组**           |

## for in

```js
// 不适合遍历数组
Array.prototype.searchItem = function (value) {
  //函数已被简化
  return right;
};

var a = [1, 2, 3, 4];
for (var i in a) {
  console.log(a[i]);
}
```
## for of

## Object.keys()

## Symbol.iterator

### 什么是 iterator?
迭代器，它是用于访问集合类的标准访问方法, 在`Symbol.iterator`出现后 JavaScript 可以定义自己的迭代器, es6 中有三类结构生来就具有 Iterator 接口：**数组**、**类数组对象**、**Map 和 Set 结构**。

```js
let arr = [1, 2, 3, 4];
let iterator = arr[Symbol.iterator]();
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

### 自定义迭代器

```js
var p = {
  name: "kevin",
  age: 2,
  sex: "male",
};

Object.defineProperty(p, Symbol.iterator, {
  writable: true,
  enumerable: true,
  configurable: true,
  value: function () {
    let _this = this;
    let index = -1;
    let keys = Object.keys(_this);
    return {
      next: () => {
        index++;
        return {
          value: _this[keys[index]],
          done: index + 1 > keys.length, // true代表完成
        };
      },
    };
  },
});

for (let v of p) {
  console.log(v);
}
```

### 测试

找出所有具有 Symbol.iterator 的原生对象，并且看看它们的 for of 遍历行为。

```js
let keys = Object.getOwnPropertyNames(window).filter((prop) => {
  return (
    window[prop] &&
    window[prop].prototype &&
    window[prop].prototype[Symbol.iterator]
  );
});
console.log(keys);
```
