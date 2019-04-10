# 第一章：灵活的JavaScript

- 首先直接声明函数将会创建全局变量的函数，很容易函数名重复造成代码覆盖和污染，所以我们要抽象到类里；

- 这里约定一下

  - var a = b() 这种构建方式成为声明对象构建
  - var a = new b() 称为用类创建对象

- 第一种将方法写在类的方式,也是我们最自然的写法是

  ```javascript
  var some_object = {
      a: function(){
          // balabala
      },
      b:function(){
          // balabala
      }
  }
  
  // 这种方法等价于
  var some_object = function(){};
  some_object.a = function(){
      //balabala
  }
  some_object.b = funciton(){
      // balabala
  }
  
  // 但是这2种方法的问题在于，在复用的时候，如果我写
  var new_object = new some_object()；
  那么这个时候new_object继承不到a b方法，也就是说不能通过new关键字来复用
  ```

- 第二种方法是return 一个对象

  ```javascript
  var some_object = function(){
      return {
          a: function(){
              // balabala
          },
          b: function(){
              // balabala
          }
      }
  }
  
  这时候我们可以通过new或者var new_object = some_object()的方式继承到a b方法
  而每一次我们在调用这个“构造函数”的时候返回的都是一个全新的对象，所以每个人的使用互不影响
  ```

- 第三种方法是用this关键字

  ```javascript
  var some_object = funtion(){
      this.a = function(){
          // balabala
      }
      this.b = function(){
          // balaba
      }
  }
  
  这种方法也可以被继承，但是只能通过new关键字，通过创建类对象来使用
  被继承的方法属性互不影响，其实与第二种方式差不多
  ```

- 第四种方法是原型继承，用原型链

  ```javascript
  var some_object = function(){}；
  some_object.prototype = {
      a:function(){
          // balabala
      },
      b:function(){
          // balabala
      }
  }
  
  原型链上的方法是共享的；
  ```

- 简单的说，如果我们创建的类对象下的方法属性，需要共享，那么用原型链继承；如果希望方法之间是互不打扰的，用this关键字/return方法。（这个感觉还挺常考的）

- 如何做到链式调用

  - return this 即可

- 有空可以看看prototype.js，该库对原生JavaScript内的许多对象添加了许多拓展方法。