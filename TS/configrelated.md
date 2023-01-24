### type declarations 


- XX.d.ts， 包含所有的type的定义和解释
- 项目中使用的第三方库，都有自己的declarations，解释和定义接口，不知道怎么定义自己的type可以参考各种第三方库的写法
- 比如axios，get方法是一个generic，调用时传的type将决定api返回的类型也就是res.data的类型将被定义成传过去的type
- 如果第三方库没有自带的declarations，通常都需要单独安装，这种库的package.json中通常没有配置types 
- 比如 loadsh， npm install --save-dev @types/lodash


#### **DefinitelyTyped**

- types 文件夹中有上千个库的types， 可以通过@type/XX来安装具体的库需要的type



### Modules

- 未使用modules之前，所有ts文件共享一个作用域，变量名重复问题，打包出来的单独的js文件存在引用顺序问题等等；
- 设置了modules之后再html中引入js文件的script标签要定义type为modules（都是单独的文件还没有使用webpack打包成bundle）
- tsconfig中的modules配置有很多选择，commonjs对应的exports，modules，而ES6对应的import export
- 使用import时，type和interface保险使用加上一个type关键字


```
import type {Pet} from ''
import {type Pet, 其他不是type的} from ''
```


### Webpack

- webpack把多个js文件打包成几个，这样在浏览器加载时不需要一个一个加载
- 需要安装的

```
webpack
webpack-cli
typescript

ts-loader (中间人，帮忙run tsc将ts转为js再打包成bundle，除了ts文件还有其他jsx等需要对应的loader)
```

- webpack有很多loader用来帮忙打包，babel等


#### webpack config basic

- module.exports = {}
- entry: 入口文件
- module: {rules: [{}]}， 每个{}中是希望将什么文件用什么转换成什么

```
{
    test: /\.tsx?$/  ?optional,
    use: 'ts-loader',
    exclude: /node_modules/
}
```

以ts或者tsx结尾的文件，用ts-loader编译


- resolve: {extensions:['.tsx', '.tx']}， 数组中的问题件类型会被resolve
- output:{filename:'',path: **path.resolve**(..dirname, 'dist')}，
- path.resolve是require的path包，传入的当前目录，和dist文件夹，当前目录是 node中的使用方法

------------

- sourceMap，方便本地debug，否则bundle文件是丑化的不方便debug
- devTool: 'inline-source-map'
- mode: development, 开发模式不会进行代码的合并简化，production是简化后的code
- dev-server， webpack自带了server，不需要lite-server，直接安装npm install webpack-dev-server
- npm run webpack serve, 或者加一个script serve: webpack serve
- 添加了server之后要加一个publicPath: '/dist'在output配置中，因为run serve时并不打包出bundle文件，而在内存中更新和本地开发，只有run build才会真正输出文件结果


- mode: production， 另外创建一个prod的configjs文件，给prod的script run时调用

```
scripts: {
    build: webpack -config webpack.prod.js
}
```

- prod打包出来的是乱码之后的js文件
- 当有hash时，每次打包出的都是新文件，需要一个即使clean的脚本，clear-webpack-plugin
- npm install clear-webpack-plugin --save-dev
- plugins: [new CleanWebpackPlugin()],这里的plugin可以添加多个webpack的plugin


