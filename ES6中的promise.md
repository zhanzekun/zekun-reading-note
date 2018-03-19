### ES6中的promise

- 对象的状态不受外界影响，Promise对象代表一个异步操作，有三种状态：
  - pending（进行中）
  - fulfilled（已成功）
  - rejected（已失败）
  - 只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。
- 一旦状态改变，就不会再变，任何时候都可以得到这个结果
- 基本用法

```
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

- Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。
  - ```
    promise.then(function(value) {
      // success
    }, function(error) {
      // failure
    });
    ```

- 举个例子

  ```
  function timeout(ms) {
    return new Promise((resolve, reject) => {
      setTimeout(resolve, ms, 'done');
    });
  }

  timeout(100).then((value) => {
    console.log(value);
  });
  ```

  ​

