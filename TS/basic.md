### TS可以帮助在程序运行之前发现errors


#### 安装和设置

- npm install -g typescript


#### 变量

- let strTest: string = "test"，在ts中设定的type不可以在后面进行类型变换，js中用let可以再赋值为number等，ts中不允许；
- 同理，numTest: number, booTest: boolean， 都不可以再修改type；


#### 编译.ts文件

- 安装ts之后，tsc file.ts 进行编译output相应的js文件


#### type interface

- ts可以帮助我们默默定义type，如果没有在变量后给type，但是赋值为一个string，ts帮忙设定为string type；
- 同理，number和boolean


#### any type

- let anything: any = 'any thing', 设置为any的type的变量可以进行任意type的赋值；
- let anything; 这样方式定义的变量其实也是any type， 后面可以赋值任意type多次
- 但是any的多次赋值，ts没有报错，这也就会出现js中因为没有对类型进行控制，而出现的不同类型上方法引用的errors，所以**尽量不使用any type在ts中**；
- 或者对于明确知道type的变量，即使是提前声明也带上type，let anythingStr: string;


#### Function

##### 函数参数

- 函数的参数类型，跟在参数身后(paramnum: number, paramstr: string) {}， 箭头函数同理；
- ts中定义了的参数个数也必须遵守，调用时必须传入相同数量的参数；
- 默认参数和js中一样，参数后加type之后 = 默认参数，(strparam: string = 'Default') {}

##### 函数返回

- 函数返回类型，跟在()后面表示 (): number {}， 表示函数内返回的是number类型；
- 当函数内没有返回时，(): void {}，返回的是void；
- 当没有自定义返回的type，而函数内有return，ts会帮忙根据return来定义函数返回的类型；
- 而当函数内有if条件语句，return不同的类型，ts也会根据多个return，帮忙定义函数的返回是一个union；

##### 匿名函数

- 数组的自有方法有些传入匿名函数时，ts会根据外部数组中的item类型来定义匿名函数的传入参数类型[1,2,3].map(item => {})，这里item会被ts自动定义为number类型

##### void type(只在function的特有类型)

- 未设置return的function都默认为void type
- 手动设置void，就是告诉ts不能再function中有return，否则就报错


##### never type(没有任何机会修改的类型为never)

- const [num, setNum] = useState([])，这里num就是never，ts认为是一个空数组，永远都是不能修改
- 在函数中，函数返回类型是never，表示函数中不能出现return




#### Object

- 可以在定义一个对象时定义其类型，let obj: {x: number} = {x: 90}
- 这个对象既可以是单独的一个变量也可以作为函数的参数或者返回值，function getStrLen(obj: {para: string}): {y: number}
- 注意： 定义好传入obj的函数，在调用时要注意，如果调用时传入的是字面量，那这个字面量必须和声明函数时定义的type保持一致，而调用时传入的是一个提前定义好的变量，再调用时传入，这个提前定义的变量对象，只要其中含有函数入参的属性名就够了，ts会从这个变量里找出来需要的property；
- 例如 function getAB(ab:{a: string, b: string})，如果使用时getAB({a: 'testa', b: 'testb'}),但是 const abc = {a: 'a', b: 'b', c: 'c'}, getAB(abc) 不会报错，因为abc中包含了a和b；


##### object type

- object字面量太长了，一般都提出来type或者interface
- type Person {name: string, age: number}
- let newperson: Person = {name: 'new', age: 29}
- function newPerson(param: Person): Person {这里需要return和Person相同type的obj}
- 在实际项目中，数据库返回的数据集中每个item都有自己的type 包含有所有需要的信息，需要提前定义
- 如果对象中某一个属性不是必须的，optional用 **?** 表示{x: number, y?: string}
- 如果某一个属性不可变，用 **readonly** {readonly id: 23, name: string}

--------------------------------------------
- combine多个types作为一个对象的type 用 **&** 直接拿type来combine也可再加上自己的 &{}


#### Array

- const emptyArr: [] = []，这样子的表示定义成一个空数组了，不能再修改，push shift等
- const stringArr: string[] = []， 正确的定义array type的方式，且只能加入string的item
- string[] 等价于 **Array<string>** , 同理number，boolean
- 同理对于自定义的type和string一样可以定义数组的item，const persons = Person[], const points = Point[]...这样定义的arrays只能接受对应type的item进行push
- string[] 只是一维数组，每个item是string，多维数组的定义  **string[][]** 后面的数组接受的item是以string为item的数组


#### Union

- 定义多个类型时，可以用| 表示union types，let unionType: string | number = 45；unionType可以赋值为string，也可以是number
- 自定义的types都可以用来union使用，const unionCustom: Person | Point = {两个type和合集}
- 对于union type的修改都必须复合其中某一个的类型至少
- 对于union可以是变量，对象或者函数参数，当作为函数参数时，因为type不同，在函数内使用时就需要对type进行条件判断 **typeof** 防止类型使用错误，而ts帮我们做的就是在只有两个union时，只需要对其中一个做if 判断

##### union Array

- union应用在数组中，const stuff: (string | number)[] = [1, 2, 't'], 数组中可以是string或者number
- 另一种写法 stuff: string[] | string[] = [], 数组内全是string或者全是number
- 自定义types也可以像上面那样应用在数组的union， const unionArr: (Person | Point)[] = [{person item}]

##### literal type

- 用一个collection容纳所有的values，变量或参数必须是这个collections里面的某一个item，let zero = 0, 这里zero被定义为0 forever
- type DayOfWeek = 1|2|3|4|5, let today: DayOfWeek = 1, today = 't'  t并不在collections中会报错

------------------------
[参考](https://github.com/mqyqingfeng/Blog/issues/229)

字面量类型本身并没有什么太大用，如果结合联合类型，就显得有用多了。举个例子，当函数只能传入一些固定的字符串时：
```
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}
printText("Hello, world", "left");
printText("G'day, mate", "centre");
// Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'.
```
数字字面量类型也是一样的：
```
function compare(a: string, b: string): -1 | 0 | 1 {
  return a === b ? 0 : a > b ? 1 : -1;
}
```
当然了，你也可以跟非字面量类型联合：
```
interface Options {
  width: number;
}
function configure(x: Options | "auto") {
  // ...
}
configure({ width: 100 });
configure("auto");
configure("automatic");

// Argument of type '"automatic"' is not assignable to parameter of type 'Options | "auto"'.
```
还有一种字面量类型，布尔字面量。因为只有两种布尔字面量类型， true 和 false ，类型 boolean 实际上就是联合类型 true | false 的别名。

#### 字面量推断（Literal Inference）
当你初始化变量为一个对象的时候，TypeScript 会假设这个对象的属性的值未来会被修改，举个例子，如果你写下这样的代码：
```
const obj = { counter: 0 };
if (someCondition) {
  obj.counter = 1;
}
```
TypeScript 并不会认为 obj.counter 之前是 0， 现在被赋值为 1 是一个错误。换句话说，obj.counter 必须是 number 类型，但不要求一定是 0，因为类型可以决定读写行为。

这也同样应用于字符串:
```
declare function handleRequest(url: string, method: "GET" | "POST"): void;

const req = { url: "https://example.com", method: "GET" };
handleRequest(req.url, req.method);

// Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'.
```
在上面这个例子里，req.method 被推断为 string ，而不是 "GET"，因为在创建 req 和 调用 handleRequest 函数之间，可能还有其他的代码，或许会将 req.method 赋值一个新字符串比如 "Guess" 。所以 TypeScript 就报错了。

有两种方式可以解决：

- 添加一个类型断言改变推断结果：
```
// Change 1:
const req = { url: "https://example.com", method: "GET" as "GET" };
// Change 2
handleRequest(req.url, req.method as "GET");
```
修改 1 表示“我有意让 req.method 的类型为字面量类型 "GET"，这会阻止未来可能赋值为 "GUESS" 等字段”。修改 2 表示“我知道 req.method 的值是 "GET"”.

- 你也可以使用 as const 把整个对象转为一个类型字面量：
```
const req = { url: "https://example.com", method: "GET" } as const;
handleRequest(req.url, req.method);
```
as const 效果跟 const 类似，但是对类型系统而言，它可以确保所有的属性都被赋予一个字面量类型，而不是一个更通用的类型比如 string 或者 number 。

----------------------------

#### Tuples

- tuples是数组类型，但是是固定数组中每个index对应的item的type按照固定的type来填入
- let tuple: [number, string] = [], tuple = [2, 'test']只有这样才可以赋值成功
- tuples的用处，比如rga由三个数值组成，const color: [number, number, number] = [255, 255, 0]
- 另一种用处， 对api返回的状态码和message，type HTTPResponse = [number, string], const goodRes: HTTPResponse = [200, 'ok'], 注意 ： 利用tuples定义的数组，初始赋值时遵守tuple的type，后面可以对数组进行任何方法操作（bug）
- 以上问题是tuple带有的，所以推荐用object type来明确包含的property更合理


#### Enums

- 其他语言也有enums，常量的集合，比如键盘的方向，一个商品的状态合集等等，对于这个的使用都通过enums的索引值来访问，可以理解成避免使用 **dummy number** 而使用的别名
- ,  在后面的使用中都通过这个enum的key来指代原来定义的PENDING本身的value, enum可以用来作为参数的type

```
const PENDING = 1...
enum Status {
    PENDING, 
    ACTIVE, 
    INACTIVE
}
function isDelivered(status: Status) {
    return status === Status.PENDING
}

isDelivered(Status.ACTIVE)

```

- 在ts中使用enum定义的，在js文件中都直接将key对应的value直接放在这个enum对象中



#### Interface 只给object使用

- 前面的type对于变量，数组，函数都可以用，interface只是给对象用的

```
type Point = {}
interface Point {
    x: number,
    y: number,
    z?: number,
    getLo: () => string;
    getLo(x: number): string;
}

```

- readonly 的不可以修改，只有optional的? 可以被delete也可以被修改
- 添加方法有两种方式，箭头函数后指定方法返回的类型，也可以方法名+括号后加返回的type

-----------------------------
- interface和types的区别： interface如果前面定义一个，后面有同名的，那么两个会合并在一起，这个interface包含两个的内容，而type不能重复，在使用第三方库的时候interface更适合引用和自定义
- interface之间可以互相extands，就像js的class一样继承，而type的方法更像union一样用 **&** 来合并不同的type
- extands 不同的interface用逗号，隔开，再加上自己的property



#### 编译TS

- tsc init, 生成tsconfig.json文件用来配置ts
- tsc index.ts, 生成编译之后的index.js文件
- tsc --watch, 打开一个终端观察ts文件的change
- tsc, 编译所有ts文件(config里面配置的)
- tsc -w, 观察所有文件


#### configs

- files，和compileroptions同级别，如果有设置，只有在files:[]中的文件才会被include
- include, 另一种指定方式，默认**表示包含所有ts文件， ['src']表示src文件夹下面的所有ts文件，也可以是'src/filename'来制定文件夹下的文件
- exclude，默认的是node_modules，通常吧test文件exclude出去，['**.test.ts'], 注意的是如果设置了exclude那别忘了把默认的加上去，因为会覆盖

------------------------
在complieroption中的config
- outDir，输出的目录，'./dist'
- target，通常是'es5'
- strict，一个捷径设置，不用自己手动去设置很多东东，直接设置为true，如果是false，那么会有下面多个控制对于一些错误使用是选择报错还是warning
- noImplicitAny，如果你没有指定一个类型，TypeScript 也不能从上下文推断出它的类型，编译器就会默认设置为 any 类型。如果你总是想避免这种情况，毕竟 TypeScript 对 any 不做类型检查，你可以开启编译项 noImplicitAny，当被隐式推断为 any 时，TypeScript 就会报错。
- strickNullChecks，把null undefined向其他变量赋值时提醒or not


