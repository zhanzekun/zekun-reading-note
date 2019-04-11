# 第二章 ：JavaScript里的面向对象编程

- 这一章就讲了2件事情：封装、继承

- 我们约定，一个类，命名的时候首字母是大写

- 封装

  - 在JavaScript里的面向对象封装也有私有属性/方法、共有属性/方法、保护方法等

    ```JavaScript
    var Book = function(id, name, price){
        // 私有属性
        var num = 1;
        
        // 私有方法
        function checkId(){
            // TODO
        }
        
        // 特权方法：即赋值在this上的方法,为什么叫特权方法呢，因为它们可获取到这个类内的私有属性和方法；换言之，Book类实例化之后，我们不能直接访问到book.num。只能通过特权方法，就像Java里的封装接口，或者称为构造器；
        this.getName = function(){};
        this.setName = funtion(){}
        
        // 对象共有属性
        this.id = id；
        
        // 对象公有方法，这里的意思大概就是特权方法访问到了私有属性/方法，但是这里没有；
        this.copy = function(){}
    }
    
    // 这个叫类静态共有属性/方法，其实例不能访问到这个属性/方法
    Book.isChinese = true
    // 但是这样写就可以访问到
    Book.prototype = {
        // 公有属性
        isJSBook： false,
        // 公有方法
        display: function(){}
    }
    ```

  - 💕 这里讲到一个很重要的概念（也就是为什么通过.符号添加类静态共有属性/方法，其实例无法访问到的原因）

    - > 通过new关键字创建的对象 实质是 对新对象this的不断赋值， 并将prototype指向类的prototype所指向的对象；而类的构造函数外面通过点语法定义的属性方法是不会添加到新创建的对象上的。

  - 而我们常常将类的静态变量通过**闭包**来实现，这个就很好理解了，比如我需要统计这个类一共有多少个实例的时候就需要一个totalNum的类静态变量；

    ```JavaScript
    // 闭包实现
    var Book = (function(){
        // 静态私有变量
        var bookNum = 0;
        // 静态私有方法
        funciton checkBook(name){}
        
        //重点来了，这里要返回一个类构造函数！！
        // 定义类构造函数
        function _book（newId, newName, newPrice）{
            // 私有变量
            var name, price；
            // 私有方法
            function checkID(id){}
            // 特权方法
            this.getName = function(){};
        	this.setName = funtion(){}
            //公有属性/方法
        	this.id = id；
        	this.copy = function(){}
            
            bookNum++;
            if(bookNum > 100){
                throw new Error('数量超过100')；
            }
            
            // 静态公有属性
    		_book.prototype = {
                // 公有属性
        		isJSBook： false,
        		// 公有方法
        		display: function(){}
    		}
    	}
        // 返回这个构造函数
        return _book;
    })();
    ```

  - 使用new关键字来创建新实例 和 直接var a = Book()有什么区别？

    - 后者是全局执行Book（）构造函数，此时this指针指向window全局

    - 如何避免这个错误（如何保证同事都用new关键字来创建新实例？）

      - 使用 Instanceof

        ```
        // 使用this instanceof 类构造函数
        // 比如
        
        var Book = funtion(title, time, type){
            // 判断执行过程中的this是否是当前这个对象，如果是则说明是用new创建的
            if(this instanceof){
            	// 执行正常逻辑
                this.title = title
            }else{
            	// 重新用New 创建新实例
                return new Book(title, time, type)
            }
        }
        ```

    - new关键字的作用可以看作是对当前对象的this属性不停地复制，而如果我们写var a = Book()，此时相当于全局作用域中执行了构造函数，那么this指针指向的就是全局变量。

- 继承

  - 类式继承：就是通过原型链继承，将父类的一个实例赋值给子类的prototype上；通过该方法继承的属性/方法共享

    ```javascript
    SubClass.prototype = new SuperClass();
    
    // 注意 子类不是父类的实例，子类的实例/子类的原型对象才是父类的实例
    subClass_instance instanceof SuperClass —— true
    subClass_instance instanceof SubClass —— true
    subClass instanceof SuperClass —— true  -- false
    subClass.prototype instanceof SuperClass —— true
    ```

  - 构造函数继承，用call（），这种方法不涉及原型链，不会继承父类原型链的方法/属性。这个方法的本质就是将子类的方法在父类中执行一遍，由于父类中写的是this绑定属性，因此子类自然也继承了父类的公有属性。通过该方法继承的属性/方法不被共享，互不打扰

    ```JavaScript
    // 父类
    funciton SuperClass(id){
        this.id = id
    }
    // 父类声明原型方法
    SuperClass.prototype.showId = function(){}
    
    // 子类
    function SubClass(id){
        SuperClass.call(this, id);
    }
    ```

  - 组合式继承，将上面两种方法结合在一起，优点是我们可以把共享/非共享的变量/方法通过不同的继承方法来继承；缺点就是我们使用构造函数继承的时候执行了一次父类的构造函数，而在类式继承中又执行了一次父类构造函数，所以父类构造函数调用了2次，有时候意味着开销很大；

    ### 那么为了解决上述这个问题

  - 原型式继承：写了一个过渡函数对象，来封装类式继承

    ```JavaScript
    function inheritObject(o){
        function F();
        F.prototype = o;
        return new F()
    }
    
    var book = {
        name: 'js book'
        alikeBook: ["css book", "html book"]
    }
    
    var newBook = inheritObject(book);
    newBook.name // js book
    ```

    - 这里作者提了一下Object.creat()这个方法，看到一篇文章讲的不错，可以理解为就是上面这个InheritObject函数，相当于复制了一个父类，但是并没有复制父类的原型链

      - > <https://juejin.im/post/5acd8ced6fb9a028d444ee4e>

  - 寄生式继承：简单的说就是在原型继承的二次封装，其效果和原型链继承模式一样

    - > <https://www.zhihu.com/question/22232912> 这里讲的还挺不错的，黑猫的答案

    - ```javascript
      function inherit_book(o) {
          function F(){}
          F.prototype = o;
          return new F()
      }
      
      var book = {
          alike: ['css', 'html']
      }
      
      function createBook(obj) {
          var o = new inherit_book(obj)  // 这个new 关键字好像加不加都是一样的
          o.get_alike = function(){
              console.log(o.alike) 
          }
          o.push_sth = function(sth){
              o.alike.push(sth);
          }
          return o
      }
      
      var new_book_1 = new createBook(book);  // 这个new 关键字好像加不加都是一样的
      new_book_1.push_sth('vue')
      new_book_1.get_alike()  // 输出css html vue
      var new_book_2 = new createBook(book);  // 这个new 关键字好像加不加都是一样的
      new_book_2.push_sth('react')
      new_book_2.get_alike() // 输出css html vue react
      
      // 可见寄生式继承的效果和原型继承是一模一样的
      
      ```

    - 这里这个new关键字要弄懂啊

  - 寄生组合式继承（终极形态）,通过这种方式，就避免了在组合继承方式里父类构造函数被执行2次的问题

    ```JavaScript
    function inheritPrototype(subClass, superClass){
        // 复制一份父类的原型副本保存在变量中
        var p = inheritObject(superClass.prototype)
        // 修正因为重写子类原型导致子类的constructor属性被修改，如果不这么写，那么subClass.prototype.construtor就会指向superClass，而这是不对的，prototype.construtor应该指向自身。
        p.construtor = subClass
        // 设置子类的原型
        subClass.prototype = p;
    }
    
    // 子类如果还想添加原型方法必须通过prototype. 的方法添加，如果直接赋予一个对象就会覆盖掉从父类原型继承的对象了。👇
    var a.prototype = {
        // 这个方法不被允许
    }
    ```

- JavaScript里的多继承

  - JavaScript原生不支持多继承，因为JavaScript只有一条原型链；但是我们可以通过复制（extend）的方法复制多个对象的原型链然后来完成类似的多继承；只要遍历要多继承的对象们的各个属性，然后复制进prototype就可以

- JavaScript里的多态：就是对入参进行判断（判断个数/类型etc）然后做出不同的反应形式

  - ```JavaScript
    // 假设一个add方法，不传参数返回0，传1个参数返回10+参数，传2个参数返回2个参数相加的结果。
    
    // 多态类
    function Add(){
        function zero(){
            return 0
        }
        
        function one(num){
            return 10+num
        }
        
        function two(num1, num2){
            return num1+num2
        }
        
        this.add = function(){
            var arg = arguments
        	var length = arg.length
        	swithch(len){
                case 0:
                	return zero();
                case 1:
                	return one(arg[0]);
                case 2:
                	return two(arg[0], arg[1])
                	return two(...arg)
                // ES6里的...是扩展运算符，比如a = [1,2,3],那么 ...a 就是 3个数字 1 2 3
        	}
        }
    }
    
    // 实例化
    var A = new Add();
    A.add() // 0
    A.add(5) // 15
    A.add(2, 3) // 5
    
    // 多态类就直接编写,无需实例化
    function add(){
        var arg = arguments
        var length = arg.length
        swithch(len){
                case 0:
                	return 0
                case 1:
                	return 10 + arg[0]
                case 2:
                	return arg[0]+arg[1]
        	}
    }
    ```

    

- ES6中的class关键字和extends

  - > <http://es6.ruanyifeng.com/?search=...&x=0&y=0#docs/class-extends>
    >
    > 这里参考阮一峰的ES6入门资料

  - ES6中的class关键字在大部分时候都可以直接看成是ES5的语法糖，一个完整的ES6类应该是

    ```JavaScript
    class ColorPoint extends Point {
      // 这个name的写法是实例属性的新写法，和在constructor里写this.name是等价的
      name = 'zekun';
      constructor(x, y, color) {
        super(x, y); // 调用父类的constructor(x, y)
        this.color = color;
      }
    
      toString() {
        return this.color + ' ' + super.toString(); // 调用父类的toString()
      }
        
      // 这个成为类的静态方法，它不会被实例继承，但是会被子类继承，它应该是直接在类上被调用
      // 注意，如果静态方法包含this关键字，这个this指的是类，而不是实例。
      // 静态方法也是可以从super对象上调用的。
      // 但是ES6只有静态方法！没有静态属性！也就是说没有static a = 'zekun'这种写法，但是这个写法已经被提上提案了。
      static classMethod() {
        return 'hello';
      }
    }
    
    // 现在ES6支持的静态属性只能这么写
    ColorPoint.zoom = '401'
    
    
    ES6中类必须要有construtor，如果没有显式的声明，那么会默认声明一个，这个construtor其实就是
    a.prototype.construtor，是严格相等的===！所以它返回的就是实例对象
    
    construtor里的this指向的是实例对象
    
    在Class类中写的方法，不用写function ,方法之间不需要逗号分隔，加了会报错
    
    Class中的方法是写在原型链上的，在类的实例上调用方法，实际上就是调用原型上的方法
    也就是说与 ES5 一样，实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上）。
    
    在ES6中，类内定义的方法是不可枚举的(non-enumerable)，而在es5中，我们在prototype上用.运算符添加方法，这个方法是可以被枚举的
    ```

  - 使用class需要注意的点：

    - 类/模块的内部默认就是严格模式，ES6 实际上把整个语言升级到了严格模式。

    - 不存在提升（变量/函数提升

    - 也有ES5中的name属性

    - **this指向**，这个真的非常重要。类的方法内部如果含有`this`，它默认指向类的实例。但是，必须非常小心，一旦单独使用该方法，很可能报错。

      - 一个比较简单的解决方法是，在构造方法中绑定`this`，这样就不会找不到`print`方法了。

        ```JavaScript
        class Logger {
          constructor() {
            this.printName = this.printName.bind(this);
          }
        
          // ...
        }
        ```

        

      - 另一种解决方法是使用箭头函数。

        ```JavaScript
        class Obj {
          constructor() {
            this.getThis = () => this;
          }
        }
        
        const myObj = new Obj();
        myObj.getThis() === myObj // true
        ```

    - 在Class里没有原生支持私有方法和私有属性，这里提供变通方法

      - > <http://es6.ruanyifeng.com/#docs/class#%E7%A7%81%E6%9C%89%E6%96%B9%E6%B3%95%E5%92%8C%E7%A7%81%E6%9C%89%E5%B1%9E%E6%80%A7>

  - ##### 继承：Class 可以通过`extends`关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。

    - ```JavaScript
      class Point {
        constructor(x, y) {
          this.x = x;
          this.y = y;
        }
      }
      
      class ColorPoint extends Point {
         // 不管有没有显式定义，每个子类一定有一个construtor对象；你没有显式写也会帮你自动添加一个
         // 只有在constuctor里调用了super（）之后才能使用this关键字。否则会报错。这是因为子类实例的构建，基于父类实例，只有super方法才能调用父类实例。
        constructor(x, y, color) {
          this.color = color; // ReferenceError
          super(x, y);
          this.color = color; // 正确
        }
      }
      
      👇
      Object.getPrototypeOf() 可以用来从子类上获取父类。
      Object.getPrototypeOf(subClass) === superClass // true
      ```

    - 还有一些其他的细节，这里不赘述了…太多了要用再看

- 部分测试代码：

  ```javascript
  function a(){
      this.a_value = 1;
      this.get_a_value_in_constuctor = function(){
          return this.a_value + '!!!!';
      }
      this.add_a_value = function(){
          this.a_value++;
      }
  }
  
  a.prototype.get_a_value = function(){
      return this.a_value;
  }
  
  function b(){
      this.b_value = 2;
  }
  
  b.prototype = new a();
  b.prototype.get_b_value = function(){
      return this.b_value;
  }
  
  var i = new b();
  var j = new b();
  
  j.add_a_value();
  console.log(i.get_a_value())
  // console.log(i.get_b_value())
  console.log(i.get_a_value_in_constuctor())
  console.log(i)
  ```

- 总结：这一章就讲了2件事情：封装和继承

  - 封装就讲了在JavaScript对象里常见的面向对象封装都是怎么实现的，包括私有属性/方法，静态属性/方法，公有属性/方法，接口等概念是怎么实现的
  - 继承是本章重点，首先是最简单的构造函数继承啊，用this关键字，然后在子类中call（）或者apply（），这种继承的方法/属性各个实例之间不可共享；然后是原型链继承（类继承），用prototype关键字，这种继承的方法/属性全部实例共享；2种方法组合起来成为组合继承，但是会有调用2次父类构造函数的缺陷；然后就引入了原型继承，做一个过渡函数，这种继承的作用相当于类继承；再引入寄生式继承，相当于对原型函数的二次封装，这种继承的作用相当于原型链继承，最后组合起来就是寄生组合式继承，也是终极形态；然后拓展一下在ES6中class和extends是怎么回事，有哪些方便的语法糖以及与ES5的不同之处
  - 多态就是根据入参的不同做不同的反应，这里不赘述



