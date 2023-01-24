#### 分类

- 普通对象： 具有所有对象的默认内部行为
- 特异对象： 具有某些和默认行为不一样的行为
- 标准对象： 规范中定义的对象，Array Date等，标准对象可以使**普通**也可以是**特异**
- 内建对象： JS执行环境中的对象，所有标准对象都是内建对象 


#### 字面量

- 对象方法简写： 不用键值对，直接写函数体，这样定义的方法有一个name属性，值为函数名
- 新方法： 
    1. Object.is() ---- 校正了NaN不等于NaN在===中的bug，还有+0和-0不等
    2. Object.assign() ---- 浅拷贝，value为对象时，复制的是引用，修改其中一个另一个也会改变
    3. 源对象的**访问器属性**（get）并不会被assign 复制到复制的对象中，而是作为一个**数据属性**
    4. getOwnPropertyDescriptor ---- 返回给定的属性的描述符，包括configurable, enumerable, get, set...

#### 自有属性 枚举顺序

- 所有数字按升序，字符串和Symbol按照被加入对象的顺序
- getOwnPropertyNames（）返回的数组中的顺序
- 注意： 在对象中的数值键是随意的，但是在枚举的时候会被重新组合和排序，字符串在其后面
- for-in循环的枚举顺序，不同厂商不同，所以尚未统一

#### 增强对象原型

ES6增加了对原型的控制力， ES5提供了getPrototypeOf，ES6提供了setPrototypeOf

- 对象原型在被实例化后保持不变，现在使用setprototypeof方法可以修改
- 另一种修改原型的方法：ES6引入的Super引用特性
    1. 未使用super引用时，重写对象实例的方法时，保证指向正确的写法 
    ```
    greeting() {
        return Object.getPrototypeOf(this).greeting.call(this);
    }
    
    // 使用super
    
    greeting() {
        return super.greeting();
    }
    
    super引用可以调用原型上所有方法
     
    ```
    
    2. super必须在简写的方法中使用
    3. 不使用super，在多重继承的case会循环递归溢出报错，使用super可以保证永远指向根原型对象
    4. super是通过[[homeObject]]这个属性先调用getprototypeof找到原型的引用，然后找出同名函数，最后设置this绑定并调用方法，只有定义在对象中的方法才有[[]]这个属性，外部定义的函数本身是没有这个属性的，这也就解释了为什么friend的super.greeting（）等价于 其原型 person.greeting.call(this)



    

