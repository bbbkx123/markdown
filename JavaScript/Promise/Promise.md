## Promise
### 什么是 Promise?
Promise是异步编程的一种解决方式  
Promise对象只有三种状态：
* ```pendding```: 初始状态，既不是成功，也不是失败状态。
* ```fulfilled```: 意味着操作成功完成。
* ```rejected```: 意味着操作失败。

Promise的状态只能由内部改变，并且$\color{red}只可以改变一次$
<br><br>

### Promise 解决的痛点是什么?
解决回调地狱
<br><br>

### Promise 解决的痛点还有其他方法可以解决吗？如果有，请列举.
generator函数, awiat/async
<br><br>

### Promise 如何使用? 
创建一个Promise实例, 通过then里接收2个方法分别处理resolved和rejected, 异常由catch捕获
<br><br>

### Promise 常用的方法有哪些？它们的作用是什么?
```Promise.resolve()```, ```Promise.reject()```, ```Promise.all()```, ```Promise.race()```, ```then```, ```catch```
<br><br>

### Promise的缺点
* promise一旦新建，就会立即执行，无法取消
* 如果不设置回掉函数，promise内部抛出的错误就不会反应到外部
* 处于pending状态时，是不能知道目前进展到哪个阶段的

### Promise 的业界实现都有哪些？
* promise可以支持多个并发的请求，获取并发请求中的数据
* promise可以解决可读性的问题，异步的嵌套带来的可读性的问题，它是由异步的运行机制引起的，这样的代码读起来会非常吃力

<br><br>

### 能不能手写一个 Promise 的 polyfill?


**题目1**
```js
const promise1 = new Promise((resolve, reject) => {
  console.log('promise1')
  resolve('resolve1')
})
const promise2 = promise1.then(res => {
  console.log(res)
})
console.log('1', promise1);
console.log('2', promise2);
```
**过程分析：**

* 从上至下，先遇到new Promise，执行该构造函数中的代码promise1
* 碰到resolve函数, 将promise1的状态改变为resolved, 并将结果保存下来
* 碰到promise1.then这个微任务，将它放入微任务队列
* promise2是一个新的状态为pending的Promise
* 执行同步代码1， 同时打印出promise1的状态是resolved
* 执行同步代码2，同时打印出promise2的状态是pending
* 宏任务执行完毕，查找微任务队列，发现promise1.then这个微任务且状态为resolved，执行它。

**题目2**
```js
Promise.resolve().then(() => {
  console.log('promise1');
  const timer2 = setTimeout(() => {
    console.log('timer2')
  }, 0)
});
const timer1 = setTimeout(() => {
  console.log('timer1')
  Promise.resolve().then(() => {
    console.log('promise2')
  })
}, 0)
console.log('start');
```
**因此过程分析为：**

* 刚开始整个脚本作为第一次宏任务来执行，我们将它标记为宏1，从上至下执行
* 遇到Promise.resolve().then这个微任务，将then中的内容加入第一次的微任务队列标记为微1
* 遇到定时器timer1，将它加入下一次宏任务的延迟列表，标记为宏2，等待执行(先不管里面是什么内容)
* 执行宏1中的同步代码start
* 第一次宏任务(宏1)执行完毕，检查第一次的微任务队列(微1)，发现有一个promise.then这个微任务需要执行
* 执行打印出微1中同步代码promise1，然后发现定时器timer2，将它加入宏2的后面，标记为宏3
* 第一次微任务队列(微1)执行完毕，执行第二次宏任务(宏2)，首先执行同步代码timer1
* 然后遇到了promise2这个微任务，将它加入此次循环的微任务队列，标记为微2
* 宏2中没有同步代码可执行了，查找本次循环的微任务队列(微2)，发现了promise2，执行它
* 第二轮执行完毕，执行宏3，打印出timer2

**题目3**
```js
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("success");
    console.log("timer1");
  }, 1000);
  console.log("promise1里的内容");
});
const promise2 = promise1.then(() => {
  throw new Error("error!!!");
});
console.log("promise1", promise1);
console.log("promise2", promise2);
setTimeout(() => {
  console.log("timer2");
  console.log("promise1", promise1);
  console.log("promise2", promise2);
}, 2000);
```

**题目4**
```js
const promise = new Promise((resolve, reject) => {
  reject("error");
  resolve("success2");
});
promise
.then(res => {
  console.log("then1: ", res);
}).then(res => {
  console.log("then2: ", res);
}).catch(err => {
  console.log("catch: ", err);
}).then(res => {
  console.log("then3: ", res);
})
```

**题目5**
```js
Promise.resolve(1)
  .then(res => {
    console.log(res);
    return 2;
  })
  .catch(err => {
    return 3;
  })
  .then(res => {
    console.log(res);
  });
```
**答案:**
```
1
2
```
且return 2会被包装成resolve(2).

**题目6**
```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('timer')
    resolve('success')
  }, 1000)
})
const start = Date.now();
promise.then(res => {
  console.log(res, Date.now() - start)
})
promise.then(res => {
  console.log(res, Date.now() - start)
```
Promise 的 .then 或者 .catch 可以被调用多次，但这里 Promise 构造函数只执行一次。或者说 promise 内部状态一经改变，并且有了一个值，那么后续每次调用 .then 或者 .catch 都会直接拿到该值。

**题目7**
```js
const promise = Promise.resolve().then(() => {
  return promise;
})
promise.catch(console.err)
```
.then 或 .catch 返回的值$\color{red}不能$是 promise 本身，否则会造成$\color{red}死循环$。

因此结果会报错：

```Uncaught (in promise) TypeError: Chaining cycle detected for promise```


**题目8**
```js
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .then(console.log)
```
```then``` 或者 ```catch``` 的参数期望是函数，传入非函数则会发生$\color{red}值透传$。

第一个then和第二个then中传入的都不是函数，一个是数字类型，一个是对象类型，因此发生了透传，将resolve(1) 的值直接传到最后一个then里。

所以输出结果为：
```
1
```

**题目8**
```js
async function async1() {
  console.log("async1 start");
  await async2();
  // 相当于在then中执行console
  console.log("async1 end");
}
async function async2() {
  console.log("async2");
}
async1();
console.log('start')

```



**题目9**
```js
async function async1() {
  console.log("async1 start");
  await async2();
  // 相当于在then中执行console
  console.log("async1 end");
}
async function async2() {
  setTimeout(() => {
    console.log('timer')
  }, 0)
  console.log("async2");
}
async1();
console.log("start")
```


**题目10**
> 我们现在要实现一个红绿灯，把一个圆形 div 按照绿色 3 秒，黄色 1 秒，红色 2 秒循环改变背景色
```js
function changeColor (color, duration) {
  return new Promise((resolve, reject) => {
    for (let i = 0; i < duration; i++) {
      setTimeout(() => {
        console.log(color);
        if (i === duration - 1) resolve()
      }, (i + 1) * 1000)
    }
  })
}

async function main () {
  await changeColor('green', 3)
  await changeColor('yellow', 1)
  await changeColor('red', 2)
  run()
}

main()

// 标准实现
// function sleep(duration){
//     return new Promise(function(resolve){
//         setTimeout(resolve, duration);
//     })
// }
// async function changeColor(duration,color){
//     document.getElementById("traffic-light").style.background = color;
//     await sleep(duration);

// }
// async function main(){
//     while(true){
//         await changeColor(3000,"green");
//         await changeColor(1000, "yellow");
//         await changeColor(2000, "red");
//     }
// }
// main()

```