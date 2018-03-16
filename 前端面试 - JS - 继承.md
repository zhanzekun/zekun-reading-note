#### 红宝书6.3 继承

继承是OO语言中的常见概念，通常分为接口继承和实现继承。而ECMAscript**<u>只支持实现继承</u>**，实现继承主要依靠**<u>原型链</u>**实现



##### 16.3.1 原型链（实现继承的主要方法）

> 实例的构造函数属性（constructor）指向构造函数。
>
> 原型对象（Person.prototype）是 构造函数（Person）的一个实例。
>
> ​    function Fn() {}// Fn为构造函数
> ​    var f1 = new Fn();//f1是Fn构造函数创建出来的对象
> ​    构造函数的prototype属性值就是对象原型。（Fn.prototype就是对象的原型）
> ​    构造函数的prototype属性值的类型就是对象  typeof Fn.prototype===object. 
> ​    对象原型中的constructor属性指向构造函数 （Fn.prototype.constructor===Fn)
> ​    对象的__proto__属性值就是对象的原型。（f1.__proto__就是对象原型）
> ​    Fn.prototype===f1.__proto__ 其实它们两个就是同一个对象---对象的原型。
> ​    所有Fn.prototype.__proto__===Object.prototype
> ​    typeof Object.prototype ===object。
> ​    Object.prototype.__proto__===null。



##### 如何确定原型和实例的关系

​	用instanceof 操作符 或者 isPrototypeOf() 方法



##### 原型链有哪些问题

1. ​	包含引用类型值的原型属性会被所有实例共享，这也是为什么我们要在构造函数中，而不是原型对象中定义属性的原因



