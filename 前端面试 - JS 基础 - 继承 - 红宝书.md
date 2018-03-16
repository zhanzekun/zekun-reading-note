## 红宝书第六章笔记

- #### 理解对象

  - 属性类型

    - 数据属性

      - 数据属性有4个描述行为的值
        - [[Configurable]]，表示能否通过delete删除属性从而重新定义/修改属性，或者能否把属性修改为访问器属性，默认为true
        - [[Enumerable]]，表示能否通过for-in循环这个属性，默认为true
        - [[Writable]]，表示能否修改，默认为true
        - [[Value]]，表示这个属性的数据值，默认为Undefined
      - 修改这四个值只能用object.defineProperty（）方法

    - 访问器属性

      - 访问器属性也有4个特性值

        - [[Configurable]]，表示能否通过delete删除属性从而重新定义/修改属性，或者能否把属性修改为访问器属性，默认为true
        - [[Enumerable]]，表示能否通过for-in循环这个属性，默认为true
        - [[Get]]：在读取属性的时候调用的函数，默认undefined
        - [[set]]：不多说

      - 访问器属性不能直接定义，也必须用object.defineProperty（），也可以通过这个函数定义多个属性

        ```
        var book ={
            _year: 2004；
        }；

        ovject.defineProperty(book,"year",{
            get: function(){
                return this._year;
            },
            set: function(newValue){
          		this._year = newValue
        	}
        });
        ```

  - 读取属性的特性：可以用object.getOwnPropertyDescritor（）获得给定属性的描述符

- 创建对象

  - 工厂模式

    - 就是封装起来,这个模式不被使用，因为无法解决对象识别问题，就是不知道这个对象的类型

      ```
      function createPerson(name){
          var o = new Object();
          o.name = name
          return o;
      }

      var person1 = createPerson("zekun")
      ```

  - 构造函数模式：用this指针

    ```
    function Person(name){
        this.name = name
        this.sayName = function{
            alert(this.name)
        }
    }

    var person1 = new Person("zekun")

    //这个做法没有显式创建对象，而是直接将属性和方法赋给了this对象，没有return语句
    //但是这个做法会让每一个对象的函数（如sayName方法）有不同的作用域链和标识符解析
    //所以可以

    function Person(name){
        this.name = name
        this.sayName = sayName
    }

    function sayName(){
        alert(this.name)
    }

    var person1 = new Person("zekun")

    //但是这个方法让sayName（）是全局函数，如果很多个全局函数不好，而且没有封装性可言
    ```

  - 原型模式：将方法赋给prototype中，问题在于所有的实例会共享原型中的引用类数据，某个实例修改了数据会影响其他实例

    ```
    function Person(){}

    Person.prototype.name = "zekun"
    Person.prototype.sayName = function(){
    	alert(this.name);
    }

    var person1 = new Person()
    ```

    - construct属性指向一个创建他的prototype
    - isPrototypeOf()方法可以确定对象与prototype的关系
    - ES5新增 object.getPrototypeOf()的方法，返回[[prototype]]的值
    - 代码读取某个对象的某个属性值，会沿着原型链往object方向查找，找到就停止
    - 可以使用hasOwnProperty（）来确定某个属性是在实例中还是prototype中
    - 尽量不要用字面量来定义一个原型

  - 组合使用构造函数和原型模式

    - 构造函数定义实例的属性

    - 原型模式定义方法和共享属性

      ```
      function Person(name){
          this.name = name;
      }

      Person.prototype = {
          constuctor : Person,
          sayName : function(){
              alert(this.name);
          }
      }

      var person1 = new Person("zekun")
      ```

      ​

  - 动态原型模式：就是加一个判断，如果想用原型模式给prototype加个什么东西，先判断下这个东西在不在

  - 寄生构造函数模式

  - 稳妥构造函数模式

- 继承

  - 原型链，让原型对象等于另外一个类型的实例，实现继承

    - ```
      subType.prototype = new SuperType();
      ```

    - 对象的默认方法都保存在Object.prototype里

    - 测试原型和实例的关系：用instanceof

    - 原型链的问题，和原型继承一样，所有引用类型值的原形属性会被所有实例共享。

  - 借用构造函数（又叫经典继承），用子类的构造函数的内部调用apply（）和call（），还可以传递参数

    - ```
      function SuperType(){
          this.colors = ["red","blue"]；
      }

      function SubType(){
          SuperTpye.call(this);
      }

      var instance1 = new SubType()
      ```

    - 借用构造函数的问题在于：没办法函数复用

  - 组合继承

    - 将原型链和借用构造函数组合到一块
    - 原型链实现对**<u>原型</u>**属性和方法的继承
    - 借用构造函数四线对实**<u>例</u>**属性的继承

  - 原型式继承，不必预先定义构造函数实现继承，本质是执行对给定对象的浅复制，然后对副本进行进一步改造

    - ES5新增了object.create()方法规范化了原型式继承

  - 寄生式继承

  - 寄生组合式继承

- 小结