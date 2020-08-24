# this的五种绑定
* 默认绑定
* 隐式绑定
* 显示绑定
* new绑定
* 箭头函数绑定

## 1. 默认绑定
默认绑定(非严格模式下```this```指向全局对象, 严格模式下```this```会绑定到```undefined```)

**题目1.1**
```js
var a = 1
function foo () {
  var a = 2
  function inner () { 
    console.log(this.a)
  }
  inner()
}

foo()
```

答案:
```
1
```

**题目1.2**
```js
var obj1 = {
  a: 1
}
var obj2 = {
  a: 2,
  foo1: function () {
    console.log(this.a)
  },
  foo2: function () {
    setTimeout(function () {
      console.log(this)
      console.log(this.a)
    }, 0)
  }
}
var a = 3

obj2.foo1()
obj2.foo2()

```
**而对于匿名函数，它里面的this在非严格模式下始终指向的都是window.**


**题目1.3**

```js
var obj1 = {
  a: 1
}
var obj2 = {
  a: 2,
  foo1: function () {
    console.log(this.a)
  },
  foo2: function () {
    function inner () {
      console.log(this)
      console.log(this.a)
    }
    inner()
  }
}
var a = 3
obj2.foo1()
obj2.foo2()
```
**非严格模式, 默认绑定inner指向window**


## 2. 隐式绑定

**不考虑箭头函数, this 永远指向最后调用它的那个对象.**  

**题目2.1**
```js
function foo () {
  console.log(this.a)
}
var obj = { a: 1, foo }
var a = 2
obj.foo()
```
答案:
```
1
```

**隐式绑定丢失**
* 使用另一个变量来给函数取别名
* 将函数作为参数传递时会被隐式赋值，回调函数丢失this绑定

**题目2.2**
使用另一个变量来给函数取别名会发生隐式丢失.
```js
function foo () {
  console.log(this.a)
};
var obj = { a: 1, foo };
var a = 2;
var foo2 = obj.foo;
var obj2 = { a: 3, foo2: obj.foo }

obj.foo();
foo2();
obj2.foo2();
```
答案:
```
1
2
3
```

**题目2.3**
如果你把一个函数当成参数**传递**到另一个函数的时候，也会发生隐式丢失的问题，**且与包裹着它的函数的this指向无关**. 在非严格模式下，**会把该函数的this绑定到window上**，严格模式下绑定到undefined.
```js
function foo () {
  console.log(this.a)
}
function doFoo (fn) {
  console.log(this)
  fn()
}
var obj = { a: 1, foo }
var a = 2
doFoo(obj.foo)
```

答案:
```
window
2
```


**题目 2.4**

如果你把一个函数当成参数**传递**到另一个函数的时候，也会发生隐式丢失的问题，**且与包裹着它的函数的this指向无关**. 在非严格模式下，**会把该函数的this绑定到window上**，严格模式下绑定到undefined.

```js

function foo () {
  console.log(this.a)
}
function doFoo (fn) {
  console.log(this)
  console.log(this.a)
}
var obj = { a: 1, foo }
var a = 2
var obj2 = { a: 3, doFoo }

obj2.doFoo(obj.foo)
```

## 3. 显示绑定
使用: ```call```, ```apply```, ```bind```

**题目3.1**
```js
var obj = {
  a: 1,
  foo: function (b) {
    b = b || this.a
    return function (c) {
      console.log(this.a + b + c)
    }
  }
}
var a = 2
var obj2 = { a: 3 }

obj.foo(a).call(obj2, 1)
obj.foo.call(obj2)(1)
```
答案:
```
6
6
```

**题目3.2**
相信大家对forEach、map、filter都不陌生吧，它们是JS内置的一些函数，但是你知道它们的第二个参数也是能绑定this的吗?
```js
function foo (item) {
  console.log(item, this.a)
}
var obj = {
  a: 'obj'
}
var a = 'window'
var arr = [1, 2, 3]

// arr.forEach(foo, obj)
// arr.map(foo, obj)
arr.filter(function (i) {
  console.log(i, this.a)
  return i > 2
}, obj)
```
答案:
```
1 "obj"
2 "obj"
3 "obj"
```

**总结:**
* this 永远指向最后调用它的那个对象
* 匿名函数的this永远指向window
* 使用.call()或者.apply()的函数是会直接执行的
* bind()是创建一个新的函数，需要手动调用才会执行
* 如果call、apply、bind接收到的第一个参数是空或者null、undefined的话，则会忽略这个参数
* forEach、map、filter函数的第二个参数也是能显式绑定this的


## 4. new绑定

**???题目4.1**
```js
function Person (name) {
  this.name = name
  this.foo1 = function () {
    console.log(this.name)
  }
  this.foo2 = function () {
    return function () {
      // 为什么window.name 为什么输出''
      console.log(this.name, window.name)
    }
  }
}
var person1 = new Person('person1')
person1.foo1()
person1.foo2()()
// 为什么window.name输出 undefined
console.log(window.name)
```

## 5. 箭头函数
* 箭头函数的this指向的是父级作用域的this， 普通函数指向的是它的直接调用者
* 不可以被当作构造函数
* 不可以使用arguments对象, 使用扩展运算符 let f3 = (...arr) => { console.log(arr)}
* 不可以使用yield命令，因此箭头函数不能用作 Generator 函数


**题目5.1**
```js
var name = 'window'
var obj1 = {
  name: 'obj1',
  foo: function () {
    console.log(this.name)
    return function () {
      console.log(this.name)
    }
  }
}
var obj2 = {
  name: 'obj2',
  foo: function () {
    console.log(this.name)
    return () => {
      console.log(this.name)
    }
  }
}
var obj3 = {
  name: 'obj3',
  foo: () => {
    console.log(this.name)
    return function () {
      console.log(this.name)
    }
  }
}
var obj4 = {
  name: 'obj4',
  foo: () => {
    console.log(this.name)
    return () => {
      console.log(this.name)
    }
  }
}

obj1.foo()()
obj2.foo()()
obj3.foo()()
obj4.foo()()
```

**!题目5.2**
箭头函数结合.call的题目

箭头函数的this无法通过bind、call、apply来直接修改，但是可以通过改变作用域中this的指向来间接修改.
```js
var name = 'window'
var obj1 = {
  name: 'obj1',
  foo1: function () {
    console.log(this.name)
    // 创建后才会绑定this
    return () => {
      console.log(this.name)
    }
  },
  foo2: () => {
    console.log(this.name)
    return function () {
      console.log(this.name)
    }
  }
}
var obj2 = {
  name: 'obj2'
}
obj1.foo1.call(obj2)()
obj1.foo1().call(obj2)
obj1.foo2.call(obj2)()
obj1.foo2().call(obj2)
```
在这道题中，obj1.foo1.call(obj2)()就相当于是通过改变作用域间接改变箭头函数内this的指向。

**总结**

* 它里面的this是由外层作用域来决定的，且指向函数定义时的this而非执行时
* 字面量创建的对象，作用域是window，如果里面有箭头函数属性的话，this指向的是window
* 构造函数创建的对象，作用域是可以理解为是这个构造函数，且这个构造函数的this是指向新建的对象的，因此this指向这个对象。
* 箭头函数的this是无法通过bind、call、apply来直接修改，但是可以通过改变作用域中this的指向来间接修改。


## 6. 综合题目

**题目6.1**
```js
var name = 'window'
function Person (name) {
  this.name = name
  this.foo1 = function () {
    console.log(this.name)
  },
  this.foo2 = () => console.log(this.name),
  this.foo3 = function () {
    return function () {
      console.log(this.name)
    }
  },
  this.foo4 = function () {
    return () => {
      console.log(this.name)
    }
  }
}
var person1 = new Person('person1')
var person2 = new Person('person2')

person1.foo1()
person1.foo1.call(person2)

person1.foo2()
person1.foo2.call(person2)

person1.foo3()()
person1.foo3.call(person2)()
person1.foo3().call(person2)

person1.foo4()()
person1.foo4.call(person2)()
person1.foo4().call(person2)
```

**题目6.2**

```js
var name = 'window'
function Person (name) {
  this.name = name
  this.obj = {
    name: 'obj',
    foo1: function () {
      return function () {
        console.log(this.name)
      }
    },
    foo2: function () {
      return () => {
        console.log(this.name)
      }
    }
  }
}
var person1 = new Person('person1')
var person2 = new Person('person2')

person1.obj.foo1()()
person1.obj.foo1.call(person2)()
person1.obj.foo1().call(person2)

person1.obj.foo2()()
person1.obj.foo2.call(person2)()
person1.obj.foo2().call(person2)
```

**题目6.3**

```js
function foo() {
  console.log( this.a );
}
var a = 2;
(function(){
  "use strict";
  foo();
})();
```