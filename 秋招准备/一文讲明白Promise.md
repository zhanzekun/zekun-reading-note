### 一文讲明白Promise

- Promise对象的特点

  - 对象不受外界影响

  - 一旦状态改变，就不会再变

  - Promise返回的永远是一个Promise对象，传递值只能通过resolve和reject

  - 注意，调用`resolve`或`reject`并不会终结 Promise 的参数函数的执行。

    - 

      ```javascript
      new Promise((resolve, reject) => {
        resolve(1);
        console.log(2);
      }).then(r => {
        console.log(r);
      });
      // 2
      // 1
      ```

    - 所以正确最好这么写：

      ```javascript
      new Promise((resolve, reject) => {
        return resolve(1);
        // 后面的语句不会执行
        console.log(2);
      })
      ```

- 完整调用形式

  ```JavaScript
  function p(){
      return new Promise((resolve,reject) =>{
          // todo
      }).then(result =>{
          console.log(result)
      },reject  =>{
          console.log('reject'+reject)
      }).catch(e){
          // to catch error
      }
  }
  ```

- 关于then，catch：他们都返回promise对象，因为所有的promise返回的都是一个promise，所以promise才可以链式调用。

- Then方法：

  - Promise 实例具有`then`方法，也就是说，`then`方法是定义在原型对象`Promise.prototype`上的。
  - `then`方法返回的是一个新的`Promise`实例（注意，不是原来那个`Promise`实例）。因此可以采用链式写法，即`then`方法后面再调用另一个`then`方法。
  - `then`方法可以接受两个回调函数作为参数。第一个回调函数是`Promise`对象的状态变为`resolved`时调用，第二个回调函数是`Promise`对象的状态变为`rejected`时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受`Promise`对象传出的值作为参数。
  - 如果调用`resolve`函数和`reject`函数时带有参数，那么它们的参数会被传递给then中的回调函数。`reject`函数的参数通常是`Error`对象的实例，表示抛出的错误；`resolve`函数的参数除了正常的值以外，还可能是另一个 Promise 实例，比如像下面这样。

- Catch方法

  - `Promise.prototype.catch`方法是`.then(null, rejection)`的别名，用于指定发生错误时的回调函数。

  - Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个`catch`语句捕获。

    - 所以不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），而是在Promise调用链的结尾使用catch方法。

      ```javascript
      // bad
      promise
        .then(function(data) {
          // success
        }, function(err) {
          // error
        });
      
      // good
      promise
        .then(function(data) { //cb
          // success
        })
        .catch(function(err) {
          // error
        });
      ```

  - 跟传统的`try/catch`代码块不同的是，如果没有使用`catch`方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。但是在Node里有一个`unhandledRejection`事件，专门监听未捕获的`reject`错误

    ```javascript
    process.on('unhandledRejection', function (err, p) {
      throw err;
    });
    ```

    - 注意，Node 有计划在未来废除`unhandledRejection`事件。如果 Promise 内部有未捕获的错误，会直接终止进程，并且进程的退出码不为 0。

- Promise.all([这是若干个promise组成的数组])

  - ```javascript
    const p = Promise.all([p1, p2, p3]);
    ```

  - `Promise.all`方法接受一个数组作为参数，`p1`、`p2`、`p3`都是 Promise 实例，如果不是，就会先调用下面讲到的`Promise.resolve`方法，将参数转为 Promise 实例，再进一步处理。（`Promise.all`方法的参数可以不是数组，但必须具有 Iterator（遍历） 接口，且返回的每个成员都是 Promise 实例。）

  - 只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。

    只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

- Promise.race（）

  - 其实和Promise.all（）差不多，就是p1,p2,p3有一个状态变成resolve，p就变成resolve然后传递值

- Promise.resolve（）

  - 有时需要将现有对象转为 Promise 对象，`Promise.resolve`方法就起到这个作用。这个方法的参数分为4中情况

    - **参数是一个 Promise 实例**

      - 直接返回

    - **参数是一个thenable对象**，指的是具有`then`方法的对象，比如下面这个对象。

      ```javascript
      let thenable = {
        then: function(resolve, reject) {
          resolve(42);
        }
      };
      
      // Promise.resolve方法会将这个对象转为 Promise 对象
      // 然后就立即执行thenable对象的then方法。
      
      //上面代码中，thenable对象的then方法执行后，对象p1的状态就变为resolved
      // 从而立即执行最后那个then方法指定的回调函数，输出 42。
      ```

     - **参数不是具有then方法的对象，或根本就不是对象**

        - 如果参数是一个原始值，或者是一个不具有`then`方法的对象，则`Promise.resolve`方法返回一个新的 Promise 对象，状态为`resolved`。

          ```javascript
          const p = Promise.resolve('Hello');
          
          p.then(function (s){
            console.log(s)
          });
          // Hello
          
          //上面代码生成一个新的 Promise 对象的实例p。由于字符串Hello不属于异步操作（判断方法是字符串对象不具有 then 方法），返回 Promise 实例的状态从一生成就是resolved，所以回调函数会立即执行。Promise.resolve方法的参数，会同时传给回调函数。
          ```

     - **不带有任何参数**：`Promise.resolve`方法允许调用时不带参数，直接返回一个`resolved`状态的 Promise 对象。

  - 所以，如果希望得到一个 Promise 对象，比较方便的方法就是直接调用`Promise.resolve`方法。

- Promise.reject（）和resolve（）方法

- Generator函数与Promise的结合

  - 大概意思就是在我们使用generator的时候，遇到yield一个异步操作，一般返回的是promise对象




