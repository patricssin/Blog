#### 默认参数

- 只有在不传或者手动传入undefined时才会使用默认参数
- null作为一个合法值，传入时将不适用默认参数，其值为null
- 命名参数在ES5非严格模式下，参数变化会更新到arguments对象中去
- 影响arguments对象： ES6中，默认参数值的存在，使得arguments对象和命名参数分离， arguments是没有设置默认值得参数
- 默认参数表达式： function add(first, second = getV()) {}, getV只有在add被调用的时候才会执行
- 先定义的默认参数，可以被后面的参数使用（first, second = first + 1）, 这里也是临时死区， 和let一样定义参数会创建一个标识符绑定参数，只有初始化之后才可以访问引用，调用函数时传入值或使用默认参数都是进行了初始化


```

function add(first = second, second) {}

add(undefined, 1)

let first = second;
let second = 1;

```

- 函数**参数有自己的作用域和临时死区**，和函数体的不同，参数的默认值不可以访问函数体内生命的变量


#### 不定参数

- 函数的lenth属性，返回的是函数命名参数的数量， ...rest不算
- rest之后不能有其他参数
- 不定参数写法不能用于setter，对象字面量setter只可以有一个参数
- 不影响arguments对象，arguments包含所有的传入参数，rest也在其中


#### 展开运算符

```
let values = [-12, -2, -34]

Math.max(...values, 0) // 去掉负数保证传入的参数至少不为负

```

#### 函数的多重用途

- JS函数有两个不同的内部方法，

```
[[Call]] [[Construct]]
```

- 当通过new 关键字调用函数执行的construct，创建一个新的对象实例，再执行函数体，再把this指向实例；不通过new 则执行call，直接执行函数体，有construct的都是构造函数
- 箭头函数因为没有construct，所以不能作为构造函数，不可以new
- 怎么知道函数是不是通过new 执行的 ---- new.target元属性
- 元属性是非对象的属性，提供非对象目标的额外信息
- new 执行函数时，会将new.target赋值为new操作的目标（新创建的实例），相反调用call方法时，会被赋值为undefined，就可以用new.target来判断是不是通过new调用函数


#### 箭头函数

- 没有this super arguments new.target
- 不能通过new
- 没有原型 没有prototype 即用即弃
- 创造箭头函数是为了引擎更好的优化，因为限制了this的指向问题使得代码简化优化起来更轻松，上面的差异减少了错误和含糊不清的地方
- 箭头函数返回对象用括号包裹 () => ({a: 1})
- 箭头函数的this === 函数外部非箭头函数的this值
- 箭头函数没有arguments的绑定，所以可以访问外层函数的arguments


#### 尾调用

- 在最后一句return 一个函数(任意)，尾调用的函数不再create 新的栈帧直接用当前作用域的栈帧 防止了调用栈变得过大而造成性能问题（省内存）
- 必须是返回函数的执行 不能有其他运算操作
- 注意： 返回函数不能是闭包，也就是不能使用当前作用域变量，这样当前作用域将无法被释放，不能达到优化的作用
 

