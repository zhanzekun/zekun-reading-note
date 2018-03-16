#### JavaScript中的作用域：通常情况下，只有函数作用域和全局作用域！只有函数作用域和全局作用域！只有函数作用域和全局作用域！只有函数作用域和全局作用域！



- ES5之前的JS中没有块级作用域。

- js作用域是词法作用域, 不是动态作用域，无论函数在哪里被调用，也无论它如何被调用，它的词法作用域都只由函数被声明时所处的位置决定。

  ```
  function test(){
    var left = {}
    te('left.top=456')  //报错left undefined
    return left
  }

  function te(str){
     eval(str)  
  }

  var a = test();
  ```

- 非严格模式下，如果在函数内部我们给一个未定义的变量赋值，这个变量会转变为一个全局变量；严格模式下会报错

- JS查询标识符：逐级往上查询

  ```
  var color = "blue"

  function getColor(){
      return color;
  }

  alert(getColor());   // “blue”
  ```

  当执行return color的时候先找函数getColor的作用域，没有找到，再去上一级全局里找

- 访问局部变量要比访问全局变量快，因为不用搜索作用域链

- **ES6 提供的`let`,`const`方法声明的标识符都会固定于块中**。常被大家忽略的`try/catch`的`catch`语句也会创建一个块作用域。

- JS也提供动态改变作用域的方法。`eval()`函数和`with`关键字.严格模式下，with（）禁止是哦那个，eval（）保留核心功能

  - eval（），eval() 函数可计算某个字符串，并执行其中的的 JavaScript 代码。

  - with关键字，可以接受一个对象，将这个对象添加到作用域链的前端

    ```
    function addUp(){
        var qs = "222";
        
        with(a){
            var url = temp + qs； // a是一个对象，有temp这个属性
        }
        
        return url；
    }
    ```

- ES5如何保护自己的作用域：**<u>IIFE，立即调用函数表达式</u>**。函数表达式后面加上一个括号后会立即执行。

  ```
  (function(){
  	for(var i =0;i<10;i++){

  	}
  })()

  （function（）{}）（）
  ```

- **<u>变量提升</u>**：JavaScript 中，函数及变量的声明都将被提升到函数的最顶部。变量可以在使用后声明，也就是变量可以先使用再声明

  ```
  a=10;

  (function(){
  console.log(a)  //输出undefined 
  var a=1;
  })();
  ```

  这段代码等价于

  ```
  var a=undefined;
  a=10;

  (function(){
  var a=undefined;
  console.log(a)
  a=1;
  })();
  ```

- ES6中用let，就和正常C++一样了..

- **<u>闭包问题</u>**：当一个函数的返回值是另外一个函数，而返回的那个函数如果调用了其父函数内部的其它变量，如果返回的这个函数在外部被执行，就产生了闭包。**<u>闭包会导致内存泄漏</u>**

  ```
  function foo() {
          var a = 2;
      
          function bar() {
              console.log(a);
          }
          return bar;
      }
      var baz = foo();
      baz(); // 2 —— 这就是闭包的效果。在函数外访问了函数内的标识符
      
      // bar()函数持有对其父作用域的引用，而使得父作用域没有被销毁，这就是闭包
  ```

  - 插播一个小问题

    ```
      for (var i = 1; i <= 5; i++) {
            setTimeout(function timer() {
                console.log(i);
            }, i * 1000);
        }
    // 其实我们想得到的结果是1，2，3，4，5，结果却是每隔一秒输出一个6，一共5个6
    // 因为setTimeout是异步的，for会先执行完

    这个问题有2个解法
    //用闭包来解决：通过一个立即执行函数IIFE，为每次循环创建一个单独的作用域
    for (var i = 1; i <= 5; i++) {
            (function(j) {
                setTimeout(function timer() {
                    console.log(j);
                }, j * 1000);
            })(i);
    ```

	```
    //把var 改为let，let 每次都会创建一个块级作用域
    for (let i = 1; i <= 5; i++) {
            setTimeout(function timer() {
                console.log(i);
            }, i * 1000);
        }
    
    ​```
    
    

  - >模块是如何利用闭包的：
    > 最常见的实现模块模式的方法通常被称为**模块暴露**。
    >
    > 我们来看看如何定义一个模块
    >
    > ```
    >     function CoolModule() {
    >         var something = "cool";
    >         var another = [1, 2, 3];
    >     
    >         function doSomething() {
    >             console.log(something);
    >         }
    >     
    >         function doAnother() {
    >             console.log(another.join(" ! "));
    >         }
    >     
    >     // 返回的是一个对象，对象中可能包含各种函数
    >         return {
    >             doSomething: doSomething,
    >             doAnother: doAnother
    >         };
    >     }
    >
    >     var foo = CoolModule();
    > // 在外面调用返回对象中的方法就形成了闭包
    >     foo.doSomething(); // cool
    >     foo.doAnother(); // 1 ! 2 ! 3
    > ```
    >
    > 模块的两个必要条件：
    >
    > - 必须有外部的封闭函数，该函数必须至少被调用一次
    > - 封闭函数必须返回至少一个内部函数，这样内部函数才能在私有作用域中形成闭包，并且可以访问或者修改私有的状态。

- 函数表达式可以是匿名的，而函数声明则不可以省略函数名——在 JavaScript 的语法中这是非法的。

- IIFE