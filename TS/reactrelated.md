### create react app

- 可以通过create来直接创建app with ts
- 也可以单独安装 typescript @types/node @types/react @types/react-dom @types/jest
- 通过create创建的packagejson中需要的在单独创建时都需要安装
- <T,>， generic的issue
- react中component的类型是**JSX.Element**，可以通过.d.ts文件查看解释
- react ts cheatsheets， 有很多react使用ts需要的type和例子
- 在定义react组件是，使用React.FC明确这是一个react的函数组件，这样做的好处是，ts帮忙添加children的props，这个函数的意义和前面函数返回一个JSX.Element的概念完全不同了
- props可以用interface或type来定义包含哪些 props: Propsinterface or {person}: Propsinterface
- 组建的props都可以提取出去，需要考虑好规划文件结构
- useState，如果像string这样的ts可以帮忙给type的就不用定义setState的type，但是如果是一个数组，就不能给一个初始值为[]， 这样会以为是永远是一个空数组
- useState是一个generic，接受Type，对于[]可以useState<Item[]>([])将这个state定义为item的数组
- react中事件回调函数，传入的event的type在react都有定义，React.FormEvent等
- useRef也是一个generic，需要提供ref使用的那个element的type，input或者buttonelement 
- useRef在使用时，input.current?.vlaue 只有在current存在时调用value




