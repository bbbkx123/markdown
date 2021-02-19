

```js
// 1. promise构造函数
// 2. then方法实现
// 3. then异步 ?
// 4. then微任务实现
// 5. then链式调用(难点)
// 6. then值穿透
// 7. catch实现
// 8. 内部错误处理
// 9. Promise.resolve(错误较多) / Promise.reject 实现
// 10. Promise.all / Promise.race 实现 (错误较多)

// 出现的问题: 
// 1. instanceof 不能通过单元测试, 尽量使用typeof进行判断
// 2. all / race

// npm install -g promises-aplus-tests
// promises-aplus-tests xxxxxx.js

const Pending = Symbol("Pending");
const Fulfilled = Symbol("Fulfilled");
const Rejected = Symbol("Rejected");
const handleValue = (promise, x, resolve, reject) => {
  //
  if (promise === x) {
    return reject(new TypeError(""));
  }
  let once = false;
  //
  if ((x !== null && typeof x === "object") || typeof x === "function") {
    try {
      let then = x.then;
      //
      if (typeof then === "function") {
        then.call(
          x,
          (y) => {
            //
            if (once) return;
            once = true;
            //
            handleValue(promise, y, resolve, reject);
          },
          (y) => {
            if (once) return;
            once = true;
            reject(y);
          }
        );
      } else {
        resolve(x);
      }
    } // !!! 
    catch (err) {
      if (once) return;
      once = true;
      reject(err);
    }
  } else {
    resolve(x);
  }
};

class MyPromise {
  constructor(executor) {
    this.status = Pending;
    this.value = null;
    this.reason = null;
    // 发布 - 订阅模式, Fulfilled状态订阅收集器
    this.onFulfilled = [];
    // Rejected状态订阅收集器
    this.onRejected = [];

    const resolve = (value) => {
      if (this.status === Pending) {
        this.status = Fulfilled;
        this.value = value;
        // 执行订阅收集器中的回调
        this.onFulfilled.forEach((fn) => fn());
      }
    };

    const reject = (reason) => {
      if (this.status === Pending) {
        this.status = Rejected;
        this.reason = reason;
        // 执行订阅收集器中的回调
        this.onRejected.forEach((fn) => fn());
      }
    };

    try {
      executor(resolve, reject);
    } catch (err) {
      reject(err);
    }
  }

  then(onFulfilled, onRejected) {
    //
    onFulfilled = typeof onFulfilled === "function" ? onFulfilled : (v) => v;
    onRejected =
      typeof onRejected === "function"
        ? onRejected
        : (err) => {
            throw err;
          };
    let promise = new MyPromise((resolve, reject) => {
      if (this.status === Fulfilled) {
        setTimeout(() => {
          // !!! try应该放在定时器里
          try {
            let x = onFulfilled(this.value);
            handleValue(promise, x, resolve, reject);
          } catch (err) {
            reject(err);
          }
        }, 0);
      }

      if (this.status === Rejected) {
        if (onRejected && typeof onRejected === "function") {
          setTimeout(() => {
            try {
              let x = onRejected(this.reason);
              handleValue(promise, x, resolve, reject);
            } catch (err) {
              reject(err);
            }
          }, 0);
        }
      }

      if (this.status === Pending) {
        this.onFulfilled.push(() => {
          setTimeout(() => {
            try {
              let x = onFulfilled(this.value);
              handleValue(promise, x, resolve, reject);
            } catch (err) {
              reject(err);
            }
          }, 0);
        });

        if (onRejected && typeof onRejected === "function") {
          this.onRejected.push(() => {
            setTimeout(() => {
              try {
                let x = onRejected(this.reason);
                handleValue(promise, x, resolve, reject);
              } catch (err) {
                reject(err);
              }
            }, 0);
          });
        }
      }
    });
    return promise;
  }

  catch(onRejected) {
    this.then(null, onRejected);
  }

  // ?
  static resolve(param) {
    if (param instanceof MyPromise) {
      return param;
    }

    // !!! 判断thenable类型的对象
    if (
      param &&
      Object.prototype.toString.call(param) === "[object Object]" &&
      typeof param.then === "function"
    ) {
      return new MyPromise((resolve, reject) => {
        param.then(resolve, reject);
      });
    }

    return new Promise((resolve, reject) => {
      resolve(param);
    });
  }

  static reject(param) {
    return new Promise((resolve, reject) => {
      reject(param);
    });
  }

   // !!! 
  static all(promises) {
    promises = Array.from(promises);
   
    return new MyPromise((resolve, reject) => {
      let length = promises.length;
      let values = Array.apply(null, { length });
      let temp = []
      if (length) {
        for (let i = 0; i < length; i++) {
          // !!!
          MyPromise.resolve(promises[i]).then(
            (res) => {
              values[i] = res;
              temp.push(res)
              if (temp.length === length) {
                resolve(values);
                return;
              }
            },
            (err) => {
              reject(err);
              return;
            }
          );
        }
      } else {
        resolve(promises);
      }
    });
  }

   // !!! 
  static race(promises) {
    promises = Array.from(promises);
    let length = promises.length;
    for (let i = 0; i < length; i++) {
      MyPromise.resolve(promises[i]).then(
        (res) => {
          resolve(res);
           // ???
          return;
        },
        (err) => {
          reject(err);
          return;
        }
      );
    }
  }


  // 用于测试的
  static defer() {
    let dfd = {};
    dfd.promise = new MyPromise((resolve, reject) => {
      dfd.resolve = resolve;
      dfd.reject = reject;
    });
    return dfd;
  }

  static deferred() {
    let dfd = {};
    dfd.promise = new MyPromise((resolve, reject) => {
      dfd.resolve = resolve;
      dfd.reject = reject;
    });
    return dfd;
  }
}

module.exports = MyPromise;

// new MyPromise((resolve, reject) => {
//   setTimeout(() => {
//     resolve('成功!!!')
//   }, 1000)
// }).then(res => {
//   console.log(res);
// })

```
