#### 事件模式 回调模式

- click 事件 readfile 回调， 回调更灵活，但是会产生回调陷阱，难以阅读和理解
- 回调的局限性，在同时执行两个并行的异步操作时，谁先执行完的时机问题


#### promise 概念

- 生命周期： pending --- unsettled --- settled --- fulfilled/rejected
- then() 接收两个参数，成功时的回调函数，失败的回调，catch方法和只传失败的回调等效
- 一定要给promise添加失败的处理函数
- 进入已处理settled状态的promise会被添加到任务队列中
- 每次调用then或catch都会生成一个新的任务，当调用者promise.then的promise被resolve时，这些任务就进入微任务栈里去


#### Promise 构造函数创建尚未处理的promise

- 接受一个执行函数，函数参数是resolve和reject，分别在执行器内部包裹的代码成功或失败时执行
- 执行器本人会被立即执行
- 当resolve被执行到时，触发一个异步操作，传入then或catch方法的函数会被放到任务队列中，也就是then中永远在执行器的执行栈后面


#### Promise.resolve() 创建已处理的promise

- 接受一个参数并返回一个完成态的promise，没有任务编排
- Promise.resolve(2)  拒绝处理函数永远不会被调用 .then((value) => //value = 2) 
- Promise.reject() 这两个方法传入一个Promise都会被直接返回

#### 全局Promise拒绝处理

- nodejs： process上绑定的unhandledRejection 和 rejectionHandled监听
- 浏览器： DOM0级事件监听，onunhandledrejection onrejectionhandled


#### 串联promise

- 串联的promise必须在末尾catch错误处理所有可能的错误


#### promise链的返回值

- 上游给下游传数据，指定return的值，在then和catch中都可以return custom的值到下一个promise处理函数中
- 返回的是promise时，如果返回的promise已经被完成，则进入第二个then的处理函数，被拒接则进入catch
- 如果在第一个then的处理函数中new一个新的promise并返回，那么后续的then处理函数只有在这个new的promise被完成resolve之后才会开始进入


#### 多个promise

- Promise.all()  参数是一个数组，数组内所有promise都被完成后返回的all()返回的pormise才会被完成，而只要有一个被拒绝，all()返回的promise立即被拒绝，catch到的是拒绝的那个promise reject的value
- Promise.race()  参数是一个数组，其中任一个promise被完成，race()返回的pormise就被完成，返回值就是最先resolve的那个value， Promise.resolve(42)创建时就已经完成那么这个是最先完成的，相对于new Promise()， 如果这个先解决的promise是一个reject的promise，那么race()返回的就是拒绝的value，其他完成的promise结果被忽略


#### 继承


#### 异步任务z执行


