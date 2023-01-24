
> 大部分面向对象语言都有类和类的继承


#### 声明类

```
class PersionClass {
    constructor(name) {
        this.name = name
    }
    
    sayName() {
        log(this.name)
    }
}

```

- 类中的constructor是构造函数， 类中使用简洁语法定义方法
- 自由属性是实例中的属性，不出现在原型上（自由属性都放在构造函数中）
- sayName实际上是定义在原型对象上的方法
- 注意： 类属性不可以被赋予新值，不像函数，persionClass.prototype就是这样的雷属性
- class类声明和let类似
- class内部代码在严格模式下执行
- 原来的自定义函数类型可以手动defineProperty修改方法为不可枚举，在class中所有方法都是不可枚举的
- class内部访问class的名字，相当于一个绑定，类似通过const定义的常量，修改会报错


#### 表达式类

- let PersonClass = class {}
- 和函数一样表达式不需要class和function后面的标识符


#### 访问器属性

- get 标识符 === getter访问器，同理setter


#### 迭代器属性

- 如果类是一个集合，可以通过定义默认迭代器

```
class Collections {
    *[Symbol.interaor]() {
        yield *this.items.values()
    }
}

```

- 实例均可以通过for-of循环或者用展开运算符操作


#### 静态方法

-  除了construct，其他方法或访问器属性都可以通过static 设置为静态方法，静态方法只能在类上访问，实例中不能访问


#### super

- 相当于调用父级类并指向当前构造函数，super传入的参数是需要改写的父类的属性
- 只能在派生类的构造函数中使用super
- 凑早函数内访问this之前就要调用super，初始化this
- 一定不适用super时，必须让构造函数返回一个对象



#### 类方法覆盖

- 继承类定义重复名的方法，会覆盖基类的方法，如果一定要调用基类中的同名方法，可以用super.functionname的方式
- 基类中的静态方法会继承到extends类中


#### extends强大的继承功能

- 凡是有[[construct]] 属性和原型的函数都可以使用extends 来继承

#### Symbol.species

- 用于定义返回函数的静态访问器属性  static get[Symbol.species]
- 被返回的函数是一个构造函数， this或者一个类（Array）
- 当在实例的方法（slice）中创建类的实例，也就是新建一个实例时，就会调用这个Symbol.specis返回的构造函数
- Array ArrayBuffer Map Set Promise RegExp 每个类都有这个属性，返回this
- 在继承类的内部重新定义static get这个属性，会覆盖原本的返回this，而使用重新定义的返回结果

- MyPromise.resolve()和MyPromise.reject()


#### 类的构造函数中的new.target

- 在类的构造函数中重新定义new.target属性，当通过new 调用时
- 通过new调用继承的类extands时，new.target指向的是继承的类
- 如果想通过一种方式，使得基类永远不能被new 而继承类可以new，就可以通过在基类中判断new.target 在等于基类时return

