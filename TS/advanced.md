### JS Classes

- js中的class中，私有属性#staticProperty只可以在class中使用，而在new出来的实例中没有私有属性，不管是变量还是方法；
- 但是，可以通过定义一个方法来获取某一个私有属性因为定义的方法在class中，在func中可以通过this获取#sth
- js class中有constructor，方法， field，private field(不想让所有实例都有只在class中私有)

```
class Player {
    #count = 20;
    score = 0;
    numLives = 10;
    constructor() {}
    taunt() {}
    getCount() {return this.#count}
}
```

#### getter & setter

- get 接的是属性名，不是方法，尝试获取这个属性名时会调用get后接的作用域 内逻辑
- get可以用来帮忙获取私有属性，而get后接的属性名，这样在使用时更make sense
- set在尝试给属性赋值时，会被调用set后接的逻辑，接受的参数就是赋值的值
- 在set中可以根据传入value来更新其他field


#### static

- static 定义的property是通知js，这个属性只在本class上，不在子实例上
- 静态字段


#### extands

- 继承class用extands
- super，当子集class想要有自己的constructor，在里面定义自己的property，在调用的时候会报错提示使用super
- super在这里做的初始化this，在没有调用super之前，尚未初始化this，所有对this的操作都会报错，也就不能再constructor中定义自己的property了
- ES6规定子集的constructor必须call super first
- super还可以直接调用父级class上的static property/function
- super做的就是call 了父级calss的constructor，传入的参数也都会传给父级的constructor



### TS Classes

- ts不允许在constructor中还没指定属性类型就对属性进行赋值，所以ts中的class的属性都要先定类型

```
class Player {
    first: string;
    score: number = 0;
    constructor (first: string) {
        this.first = first;
    }

}
```

- js中没有，ts可以设置readonly的属性，在定义类型前加上readonly


#### public & private

- ts中的property前加上public的属性，在class内外都可以访问
- private的属性和js中的#score一样只在class内部可以访问到，private method() {}也可以，子类和实例都不能访问
- public和private编制成js之后并不会出现
- 使用public和private在constructor定义时，可以不用提前声明对应属性的type，在传参时就定义了属性时public的什么类型or private的什么类型， 编译成js之后都是this.XX在constructor中

```
class Player {
    //first: string; 这里不再需要，当使用了public
    score: number = 0;
    constructor (public first: string) {
        this.first = first;
    }

}
```


#### get & set TS

- ts中如果只设置了get没有设置set，表示是一个readonly的property，不能赋值修改
- 和js中get set相同，可以指定函数返回的类型，set可以指定传参的类型

##### protected

- 因为public在哪都可以访问，private只能在父级class中访问，protected类型可以在子集中访问



#### Interface TS

- ts中可以定义一个class，让这个class follow某一个interface，并且follow其中的属性和方法
- class Bike **impletements** Color
- 比如interface中有的属性，在定义class的constructor时都应该保证和interface中一样声明出来


```
{
    color: string = 'red'
    
    or
    
    constructor(public color: string){}
}
```

- 当然也可以在自己的class中声明其他属性属于自己
- 当有impletements多个interface时，这个class就要在定义时follow多个interface的属性和方法


#### TS abstract class

- ts特有的
- abstract class Employee {}
- 当某个类是abstract，那么所有继承这个类的子类都必须有一个属性或方法和父级内部abstract 属性对应

```
abstract class Employee {
  constructor(public first: string, public last: string) {}
  abstract getPay(): number;
  greet() {
    console.log("HELLO!");
  }
}

class FullTimeEmployee extends Employee {
  constructor(first: string, last: string, private salary: number) {
    super(first, last);
  }
  getPay(): number {
    return this.salary;
  }
}

class PartTimeEmployee extends Employee {
  constructor(
    first: string,
    last: string,
    private hourlyRate: number,
    private hoursWorked: number
  ) {
    super(first, last);
  }
  getPay(): number {
    return this.hourlyRate * this.hoursWorked;
  }
}

const betty = new FullTimeEmployee("Betty", "White", 95000);
console.log(betty.getPay());

const bill = new PartTimeEmployee("Bill", "Billerson", 24, 1100);

console.log(bill.getPay());
```


### Generic

在array的type时，有两种方式string[]和Array<string>, 这里的Array其实是一个generic，后面接上的是他的type，可以理解为你给我什么类型我就按照你在使用时传来的type来follow返回的type，Array<string>返回一个string type的数组

同样的， document.querySelector<HTMLInputElement>('#id') 这里的querySelector就是一个内建的generic，不传后面的type时默认是HTMLElement，这里传入的inputtype的element， 返回的就是input类型可以在返回值上获取value属性，而默认的HTMLElement就没有value

- 以上两种Array，querySelector都是generic，后面的<>中是什么类型就在这个函数传参和返回中使用这种类型，就像一个type的传参声明
- 没有generic就要每一种类型创建一个function，用generic就灵活很多

```
// With A Generic...Super Reusable!
function identity<T>(item: T): T {
  return item;
}

identity<number>(7);
identity<string>("hello");
```

这里的type在函数传参时用来规定参数是这个类型的数组，就像sting[]，在使用时如果没有传type但是后面的数组是[1,2,3], ts会帮助辨别

```
function getRandomElement<T>(list: T[]): T {
  const randIdx = Math.floor(Math.random() * list.length);
  return list[randIdx];
}

console.log(getRandomElement<string>(["a", "b", "c"]));
getRandomElement<number>([5, 6, 21, 354, 567, 234, 654]);
getRandomElement([1, 2, 3, 4]);

```


#### react+ts

- react中<T>可能被认为是标签，解决办法就是 **<T,>**



#### 各种类型的generic

- 当某个函数接受两中type的对象，这样的generic不止需要一种时可以用，隔开<T, U...>， 后面用哪种类型自己取就可以function merge<T, U>(obj1: T, obj2: U){}
- 在调用merge时，不需要先传一个对象的type {name: string}， 直接调用并传参数的object内容，ts就能帮助将这个参数object的type猜出来并应用起来，应用在函数的返回上
- <T, U> 如果必须明确是object，可以 T extands object来固定type，保证调用时必须传对象，否则报错
- 用...解构其他类型都得到空对象

##### 用interface实现extands

- function printLen<T extands InterfaceLength>(thing: T): number {}
- 也可以直接用interface作为参数的type，而不用generic function printLen(thing: InterfaceLength): number {}

##### 默认类型的generic

- dom element的generic没有给类型时是有默认的HTMLElement
- 对于已经定义的gengeric，调用时不传type，也没有通过参数传值过去，这时候ts帮忙把type设置为 **unkonw**
- generic的默认类型是 <T = number>

##### generic的classes

- 和函数一样的，class如果根据类型不同定义不同的类，就要写很多个，如果可以通过generic的方式灵活根据传入的Type来决定
- generic定义的class，在new生成实例时，需要对应着传入Type或者Interface，来定义实例中带有的内容(也就是interface中的属性内容)


```
// A Generic Class Example
interface Song {
  title: string;
  artist: string;
}
interface Video {
  title: string;
  creator: string;
  resolution: string;
}

class Playlist<T> {
  public queue: T[] = [];
  add(el: T) {
    this.queue.push(el);
  }
}

const songs = new Playlist<Song>();
const videos = new Playlist<Video>();


```


### Guards


- 当有多个type时，函数内部要有个判断条件，不同type下使用相对应的方法，否则可能报错


#### ts的聪明之处

- 两种type时，if条件其中一种另一种ts就处理了
- 获取dom节点，if(elem)条件中ts就定义为HTMLElement，else里就是null没找到
- 如果有optional的参数，函数中对这个参数有使用，那么就可能为null，就要对参数进行if(oiptionalpara)是否存在
- 当两个参数都是多个type的union，其中有一种type是一样的，(x: string | number, y: string | boolean)，这种情况下，if(x===y)ts会帮忙处理只有两个都是string才会走入的逻辑
- **in** 通过某一个type或interface的某一个属性来做条件，找出多个参数中的某一个，在条件里进行操作
- **instanceof** 和in差不多的方式，instanceof某个class，比如后端返回的时间可能是字符串或时间Date实例，转换时就要根据不同case来处理

```
// Instanceof Narrowing:
function printFullDate(date: string | Date) {
  if (date instanceof Date) {
    console.log(date.toUTCString());
  } else {
    console.log(new Date(date).toUTCString());
  }
}

```

- **断言** 我们经常写一个方法来判断是不是某一种类型，通过返回某一种类型中的某个属性是不是undefined，但是这样直接返回boolean的function，ts并不会知道这个函数到底是哪个类型，因为返回的只是一个布尔值，ts认为还可能是定义时的那几种type
- 那么这个判断函数，就要用一种方式告诉ts，如果是true那么返回的就是某一个type, **: somthing is InterfaceTrue** something
就是这个判断函数传入的那个需要判断的东东

```
interface Cat {
  name: string;
  numLives: number;
}
interface Dog {
  name: string;
  breed: string;
}

function isCat(animal: Cat | Dog): animal is Cat {
  return (animal as Cat).numLives !== undefined;
}

function makeNoise(animal: Cat | Dog): string {
  if (isCat(animal)) {
    animal;
    return "Meow";
  } else {
    animal;
    return "Woof!";
  }
}
```

------------

- **discriminated union**  当有多个interface时，我们手动加一个属性，然后用这个属性来帮助ts判断类型，因为有时候interface有很多很多属性，使用in去找不合理，自己主动掌控是最有效的方式
- 手动加一个属性__kind， 在条件中根据这个属性switch根据不同的固定string内容来决定是啥type or interface
- **never** 用never来处理default，因为默认是步走进的，在default中对一个nevertype进行赋值会提示，当新加一个interface到union type时，但是在某个function 没有handle这个新type的switch case __kind 的值时会报错






