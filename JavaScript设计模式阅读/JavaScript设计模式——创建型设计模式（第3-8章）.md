### JavaScript设计模式——创建型设计模式（第三章to第八章）

- 简单工厂模式

  - 可以简单的理解为就做了2件事情
    1. 定义好各个类的构造函数F*（）
    2. 在工厂函数里根据入参选择返回return new F*（） 

  - 第一种方法，通过类实例化创建，子例之间方法公用。

    ```javascript
    function A(){
        // 需要构造函数继承的内容（非后代/同阶子类共享内容）
        this.alpha = 'a';
    }
    A.prototype.getAlpha = function(){
       console.log(this.alpha);
    }
    
    function B(){
        this.alpha = 'b';
    }
    
    function createAlpha(alpha){
        switch(alpha){
            case 'a':
              return new A();
            case 'b':
              return new B();
        }
    }
    
    var new_a = createAlpha('a');
    new_a.getAlpha()
    ```

   - 第二种方法，用类似寄生式继承的方法，创建一个新对象再增强属性/功能来实现，方法不能共用。

      ```javascript
      function createAlpha(alpha){
          var temp_object = new Object();
          // 相同方法
          temp_object.show = function(){
              console.log(this.alpha)
          }
          // 对入参的差异化创建
          if(alpha == 'a'){
              temp_object.alpha = 'A'
          }
          if(alpha == 'b'){
              temp_object.alpha = 'B'
          }
          return temp_object;
      }

      var new_a = createAlpha('a');
      var new_b = createAlpha('b');
      new_a.show();
      ```


- ❤ 工厂方法模式（这个会比较常用了）


  - 这是对简单工厂方法的改进，改进之后更像插件化的代码风格，创建者拿着要创建的数据对象的“id”去工厂里查找有没有对应的构造函数方法


    - 在工厂原型中设置所有类型对象的基类构造函数
    - 创建时，搜索是否工厂函数的原型上有这个基类的构造函数，如果有就创建/没有则报错。而在需求变更的时候，只需要在工厂函数的原型上新增/删除一次就可以

  - 安全的工厂方法：首先用instance of 判断当前创造的这个实例是不是用new创建的,然后类似寄生式继承一样创建新对象,不要忘记new

    ```javascript
    var Factory_book = function (type, content) {
        // 安全工厂模式，保证使用者是用new关键字来创建的；不是也是
        if (this instanceof Factory_book) {
            // 不要忘了这个new啊!!!!
            var temp_object = new this[type](content)
            temp_object.show = function () {
                console.log(this.content)
            }
            return temp_object
        } else {
            return new Factory_book(type, content)
        }
    }
    
    Factory_book.prototype.java = function (content) {
        this.content = content
    }
    Factory_book.prototype.php = function (content) {
        this.content = content
    }
    Factory_book.prototype.ui = function (content) {
        this.content = content
    }
    
    var a_java_book = Factory_book('java', 'this is a java book');
    a_java_book.show()
    ```

    

- 抽象工厂模式

  - 这个模式的重点就是创建类似C++的虚函数一样的东西,父类原型链上写出方法,但是不实现,且如果被调用则手动抛出错误.

  - ```javascript
    var VehicleFactory = function(subType, superType){
        if(typeof VehicleFactory[superType] === 'function'){
            function F(){};
            F.prototype = new VehicleFactory[superType]();
            subType.constructor = subType;
            subType.prototype = new F()
        }else{
            throw new Error('未创建该抽象类')
        }
    }
    
    VehicleFactory.student = function(){
        this.type = 'student'
    }
    VehicleFactory.student.prototype = {
        show_info: function(){
            throw new Error('这是抽象方法，不可调用')
        }
    }
    
    var a_student = function(name){
        this.name = name;
    }
    VehicleFactory(a_student,'student');
    a_student.prototype.show_info = function(){
        console.log(this.name)
    }
    
    var duanwei = new a_student('duanwei');
    duanwei.show_info();  // duanwei
    var o = new VehicleFactory.student();
    o.show_info();
    ```

- 建造者模式

  - 工厂模式不需要关心对象创建过程;而建造者模式关心:通过把创建类的过程模块化, 名字是一个模块,工作技能是个模块, 个人信息是个模块,所有模块拼起来成为这个员工的类实例.

  - ```javascript
    var Human = function(param){
        this.is_marry = param && param.is_marry || '保密';
        this.age = param && param.age || '保密';
    }
    
    Human.prototype.show_basic_info = function(){
        console.log('是否已婚：'+this.is_marry)
        console.log('年龄：'+this.age)
    }
    
    var Name = function(name){
        var _this = this;
        (function(name, _this){
            _this.name = name;
            // 如果这人名字有空格
            if(name.indexOf(' ') > -1){
                _this.first_name = name.slice(0,name.indexOf(' '));
                _this.second_name = name.slice(name.indexOf(' ')+1)
            }
        })(name, _this)
    }
    
    Name.prototype.show_name = function(){
        console.log(this.name)
    }
    Name.prototype.show_first_name = function(){
        console.log(this.first_name && this.first_name != '' ? this.first_name:'没有first name！')
    }
    Name.prototype.show_second_name = function(){
        console.log(this.second_name && this.second_name != '' ? this.second_name:'没有second name！')
    }
    
    var Work = function(work){
        var _this = this;
        (function(work, _this){
            switch(work){
                case 'front-end':
                    _this.work = '前端工程师'
                    _this.slogan = 'node.js牛逼！'
                    break;
                case 'back-end':
                    _this.work = '后端工程师'
                    _this.slogan = 'java宇宙第一'
            }
        })(work, _this)
    }
    
    Work.prototype.show_work_info = function(){
        console.log(this.work);
        console.log(this.slogan);
    }
    
    var worker_996 = function(name, work){
        var person = new Human();
        person.name = new Name(name);
        person.work = new Work(work);
        return person;
    }
    
    var duanwei = new worker_996('duan wei','front-end')
    duanwei.show_basic_info();
    duanwei.name.show_first_name();
    duanwei.name.show_name();
    duanwei.name.show_second_name();
    duanwei.work.show_work_info();
    ```

- 原型模式

  - 这个很好理解啊,就不写代码了,我们经常碰到基类非常复杂的情况，如果我们还是每个子类都去无脑继承基类，那么会让基类不断执行构造函数，拖慢效率。那么把能共享/公用的方法写到原型链上就好

- 单例模式（只允许实例化一次的对象类）

  - 很简单，在外面包裹一层名字和大括号就可以，比如这里裹上Utils

    ```javascript
    var Util = {
        ajax: function(){
            console.log('ajax！')
        },
        change_alpha:function(){
            console.log('change_alpha!')
        }
    }
    ```

  - 单例模式下的静态变量（一般约定静态变量全大写）

    - 普通单例静态变量，用立即执行表达式，函数内定义好的私有变量，只返回取值器~

      ```javascript
      var Conf = (function(){
          // 私有静态变量
          var conf = {
              MAX_NUM:1000,
              MIN_NUM:1
          }
          return {
              get: function(name){
                  return conf[name] ? conf[name] : null;
              }
          }
      })()
      ```

    - 惰性单例（延迟创建）

      ```javascript
      var Conf = (function(){
          var _instance = null
          function Single(){
              // 定义私有属性and私有方法
              return {
                  publicMethod: function(){},
                  publicProperty: '1.0'
              }
          }
          return function(){
          	// 如果当前没有实例则创建实例
              if(!_instance){
                  _instance = Single()
              }
              // 有的话直接返回
              return _instance
          }
      })()
      ```

      

- 总结:什么时候我们要用什么模式呢?

