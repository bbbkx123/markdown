https://mp.weixin.qq.com/s/YLkyMoTNIiK6KghW4QZLDQ

```ts
  let numArr: number[] = [1, 2, 3, 4, 5];
  let objArr: obj1[] = [
    { name: 'kiana', age: 18 },
    { name: 'rita', age: 20 },
  ];

  interface obj1 {
    age: number;
    name: string;
  }

  function forEach1(arr: unknown[], cb: Function) {
    if (arr.length === 0) return;
    for (let i = 0; i < arr.length; i++) {
      cb(arr[i], i, arr);
    }
  }

  // forEach1(objArr, (value: any) => {
  //   let _age = (value as obj1).age
  //   console.log(_age);
  //   // console.log(value);

  // });

  /**
  * map
  */

  function map1<T>(arr: T[], cb: Function) {
    let temp: T[] = [];
    forEach1(arr, (item: T, index: number, arr: T[]) => {
      let res = cb(item, index, arr);
      temp.push(res);
    });
    return temp;
  }

  // console.log( map1<obj1>(objArr, (item: obj1, i: number) => item.age));

  /**
  * reduce
  */

  function reduce1<T, U>(arr: T[], cb: Function, initial: U) {
    let prev: U = initial;
    forEach1(arr, (item: T, i: number) => {
      prev = cb(prev, item, i, arr);
    });
    return prev;
  }

  // let res = reduce1<obj1, number>(objArr, (prev: number , cur: obj1, index: number) => prev + cur.age, 0)
  // console.log(res);

  /**
  * .filter
  */

  function filter1<T>(arr: T[], cb: Function) {
    let temp: T[] = [];
    forEach1(arr, (item: T, index: number, arr: T[]) => {
      if (cb(item, index, arr)) {
        temp.push(arr[index]);
      }
    });
    return temp;
  }

  // let arr = filter1<number>(numArr, (value: number) => value >= 2)
  // console.log(arr);

  /**
  * findIndex
  */

  function findIndex<T>(arr: T[], cb: Function) {
    for (let i = 0; i < arr.length; i++) {
      if (cb(arr[i])) {
        return i;
      }
    }
  }

  // let res = numArr.findIndex((value) => value > 3)
  // let res1 = findIndex(numArr, (value: any) => value > 3)
  // console.log(res1);

  /**
  * find
  */

  function find1<T>(arr: T[], cb: Function) {
    for (let i = 0; i < arr.length; i++) {
      if (cb(arr[i])) {
        return arr[i];
      }
    }
  }

  /**
  * indexOf
  */

  function indexOf(arr: any[], value: any) {
    for (let i = 0; i < arr.length; i++) {
      if (value === arr[i]) {
        return i;
      }
    }
  }

  // let res = numArr.indexOf(4)
  // let res1 = indexOf(numArr, 4)
  // console.log(res1);

  /**
  * every
  */

  function every1 (arr: any[], cb: Function) {
    for (let i = 0; i < arr.length; i++) {
      if (!cb(arr[i], i, arr)) {
        return false
      }
    }
    return true
  }

  // let res = numArr.every((item: any, index) => item > 0)
  // let res1 = every1(numArr, (item: any) => item > 3)
  // console.log(res1);


  /**
  * some
  */

  function some1 (arr: any[], cb: Function) {
    for (let i = 0; i < arr.length; i++) {
      if (cb(arr[i], i, arr)) {
        return true
      }
    }
    return false
  }

  // let res = numArr.some((item: any, index) => item > 0)
  // let res1 = some1(numArr, (item: any) => item > 3)
  // console.log(res1);


  /**
  * includes
  */

  function includes1 (arr: any[], value: any) {
    for (let i = 0; i < arr.length; i++) {
      if (arr[i] === value) {
        return true
      }
    }
    return false
  }

  // let res = numArr.includes(4)
  // let res1 = includes1(numArr, 4)
  // console.log(res1, res);

 

  /**
  * reverse
  */

  function reverse1 (arr: any[]) {
    let temp: any[] = []
    temp.length = arr.length
    forEach1(arr, (item: any, i: number, arr: any[]) => {
      let idx = temp.length - 1 - i
      temp[idx] = item
    })
    return temp
  }

// let res = numArr.reverse()
  let res1 = reverse1(numArr)
  console.log(res1);


  /**
    * concat
    */
  
  function concat1 (arr: any[], ...values: any) {
    let temp = [...arr]
    let {length} = values
    for (let i = 0; i < length; i++) {
      if (Array.isArray(values[i])) {
        temp.push(...values[i])
      } else {
        temp.push(values[i])
      }
    }
    return temp
  }

  let res = arr.concat(arr1, 6, arr2)
  let res1 = concat1(arr, arr1, 6, arr2)
  console.log(res);
  console.log(res1);

  
```