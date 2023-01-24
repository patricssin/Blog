#### 代理和反射
- 代理，可以拦截js引擎内部目标的底层对象操作，拦截之后触发响应特定操作的陷阱函数；
- 反射，以Reflect对象的形式出现，对象的默认方法与 相同的底层操作一致，代理可以覆写这些操作，每个代理陷阱函数都对应一个反射方法
- 陷阱覆写js对象的内建特性，可以拦截修改这些特性，如果还需要使用**内建的特性**，可以用相应的反射API方法，创建代理可以让代理和反射的关系变得清楚；


#### 创建代理

- new Proxy(target, {陷阱们})， 不传入任何陷阱，则只是单纯地转发
-  set, 如果代理陷阱中对set进行改写，在set中加入逻辑，还想在某种情况下实现set可以使用Reflect.set。返回true表示set成功；
-  get，
-  in，使用in，在代理中用has可以拦截in操作，根据自定条件返回结果，其他不在自定条件中，按照原来的has可用Reflect.has
-  delete，在代理中用deleteProperty，自定义某个属性被删除时的结果
-  setPrototypeOf和Reflect，返回任何不是false的值都认识set原型成功
-  Object.setPrototypeOf和Reflect.setPrototypeOf的不同是，object更高级，会先对传入进行类型转换为obj，而reflect如果收到不是对象会报错
-  defineProperty，第三个参数不管传什么都只识别enumerabler, configurable, value, writable, get, set
-  getOwnPropertyDescriptor，返回值必须为null undefined或者对象，如果是对象那这个对象自己的属性必须只有上面六种
-  Object.defineProperty和Reflect.definProperty的不同是，object返回传入的对象，reflect返回boolean
-  ownKeys，getOwnPropertyNames keys getOwnPropertySymbols assign都会调用ownKeys这样可以拦截处理不想要的属性

> apply construct
- 代理一个函数
- [函数有两个内部方法]() [[Call]] [[Construct]] ，使用new执行函数会执行Cons，其他方式调用则执行Call，这时会执行apply陷阱，接受代理目标、this、参数；
- 控制函数传参的类型以及能不能使用new来生成实例
> 在你不能控制你要修改行为的函数时，使用代理更有意义

- 遇到一些引用函数或者不能修改的函数时，用代理就很方便了




#### 撤销代理

> Proxy.revocable()

#### 数组

