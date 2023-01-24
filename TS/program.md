### 初始化

- ts --init
- tsc -w
这里只能在本地终端对文件进行编译，在浏览器中

- npm init
- npm install lite-server (自动刷新)
- 添加scripts {start: lite-server}
- npm start (运行并自动刷新)


#### 介绍ts中的dom类型
 - console.dir()
 - document的type是Document，ts会给这个类型下的方法和属性的类型，Document.bgColor: string等；
 - lib默认的支持js的API和document的接口，如果手动设置了lib要保证包含多有需要的类型，如DOM，lib.com.d.ts
 - 如果设置了target：es2021，不设置lib，lib默认包含所有
 - 对ts中的dom type， 主动控制dom为更具体的type，相对于ts默认的element type，使用 **as** 将某一个变量作为我们确定的类型来对待

```
let mystery: unkonw = 2
const len = (mystery as string).length

```

- 以上的操作方法可以用在element type上面，当我们确定知道某一些elem是某一种类型的dom elem时，将其 as为具体的element type
- 在ts中只有input type的elementa才有value的属性， 而默认的HTMLElement上是没有的
- button type ele
- 不用as， 也可以在变量前加(<HTMLInputElement>inputVar).value来告诉ts这里的inputVar是一个input elem， 通常用as更清晰


#### dom事件的回调函数

- 在对不同类型dom添加事件时，在addEventListener时，使用匿名函数ts可以帮忙识别出dom的类型，在回调函数中对传入的参数进行type的mapping
- 但是如果回调是一个命名函数，从别处引用，那这个函数ts并不确定只在这里一处使用，所以不会帮忙确定cb函数传入的参数的event type，这就需要手动配置event的类型
- e: SubmitEvent




