#### Map
- 用于存取频繁的情况
- 对象object的键名必须是字符串，所有的对象设置为key都将转换为‘[objectObject]’，数字也都变string
- 构造函数传入二维数组，每个子item是一个键值对（为了保证存之前不被进行转换才用数组的方式传入
- Weak Map：一些库中会用自定义的对象保存对DOM的引用，适合使用weakmap, weak必须用对象作为key
- 构造函数中的私有属性，没有主动管理这些伴随着实例产生的属性时，不知道实例合适销毁，这些私有属性也不能自动的销毁，使用WeakMap解决这个问题，用this作为健名，这样当this这个实例被销毁，定义的属性也一同被销毁（原理是失去了对key的引用没办法使用get方法，等到回收时就一起清理了）
- map方法：has, delete, clear, size, get, forEach


### Set
- 内部用 **Object.is()** 实现去重
- map中key不会进行转换，数字5和'5'代表不同的item，不同的对象也代表独立的元素
- to array：[...set]
- Set实例存储对象，会一直保留引用，浪费内存，手动将对象设置为null仍然可以在set实例中取回引用，引入了Weak Set
- Weak Set不接受非对象值，add和构造函数传入的数组中不能包含非对象值
- Weak Set不可迭代
- 只需要跟踪对象引用时适合Weak Set


