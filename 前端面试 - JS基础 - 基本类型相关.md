### 基本类型相关的面试题



##### 如何判断JS数据类型

- typeOf

  ```
  alert(typeof a)

  //typeOf返回的都是字符串
  ```

- instanceoOf，用来判断已知对象类型的方法

  ```
  alert(f instanceof Function) ------------> true
  alert(f instanceof function) ------------> false
  ```

-  constructor

  ```
  alert(c.constructor === Array) ----------> true
  alert(d.constructor === Date) -----------> true
  alert(e.constructor === Function) -------> true
  注意： constructor 在类继承时会出错
  eg：
        function A(){};
        function B(){};
        A.prototype = new B(); //A继承自B
        var aObj = new A();
        alert(aobj.constructor === B) -----------> true;
        alert(aobj.constructor === A) -----------> false;
  而instanceof方法不会出现该问题，对象直接继承和间接继承的都会报true：
        alert(aobj instanceof B) ----------------> true;
        alert(aobj instanceof B) ----------------> true;
  言归正传，解决construtor的问题通常是让对象的constructor手动指向自己：
        aobj.constructor = A; //将自己的类赋值给对象的constructor属性
        alert(aobj.constructor === A) -----------> true;
        alert(aobj.constructor === B) -----------> false; //基类不会报true了;
  ```

  ​

-  prototype，通用但很繁琐

  ```
  alert(Object.prototype.toString.call(b) === ‘[object Number]’) -------> true;
  // 或者

  Object.prototype.toString.call(b）
  ```



##### JS的深拷贝和浅拷贝

- 只有基本数据类型的拷贝是直接深拷贝，其他都是浅拷贝
- 手动实现深拷贝只要将要拷贝的东西一步步解构为基本数据类型然后拷贝就好了



