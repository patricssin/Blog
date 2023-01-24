#### Array.of()

- new Array(2) 当传入数值时，length被设置为数值，多个参数都是item
- 使用of()方法则没有上面的情况，传入参数永远是items
- 注意： Array.of()不使用Symbol.species属性决定返回值的类型，只使用当前构造函数，也就是of方法中的this


#### Array.from()

- 没有from之前，类数组通过遍历或Array.prototype.slice.call(arraylike)来实现
- 和of一样也是通过方法中的this确定返回值类型
- 第二个参数是一个方法，修改item，如果方法中使用了this，第三个参数是指定this的值


#### find findIndex

#### fill copyWithin

- fill（）数值型的数组
- copyWithin 第一个参数是开始改变的索引，第二个是从数组中开始复制的索引，第三个是复制截止不包含的索引


