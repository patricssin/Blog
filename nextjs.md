
App router: 2023-05, 新版本不从server获取js文件， 增加layout
Page router: old version since 2016, 已有项目建议迁移到approuter


Static side generator: html和js等一起build成静态文件部署到server上
Full-stack framework: 在nodejs server上run project，static page SSR revalidation，serve动态生成a page 当user请求某一个page

在node serve上run，既可以支持部分static side generator的feature，如果需要还可以切换到SSR-R


手动安装依赖
App/layout.jsx — 所有的html文件 ，layout可以理解为一个template应用到所有page里面
ls node_modules/.bin/ 查看可用npx 命令next —help
在app文件夹中的page.jsx自动加载
转ts直接改文件后缀，run dev自动添加tsconfig

reviews用来放所有的pages， 每个页面都命名为page.jsx, review变成router
原来的pagerouter是在pages文件下的文件名变索引router，新版的approuter是用文件夹作为router
新版的每个文件夹都可以有自己的layout文件来定义子集的layout

nextjs linked


Prerender: 返回的html，更快，不支持js的浏览器中可以显示page，原来的都是通过js塞进去div里面去，seo就没有那么好，而prerender拿到的是html，更好搜索

client端的方法不能放在server端，可以用’’use client’’来标注【注意问题】

server的component是默认的，code不会传到浏览器，只在server运行，client的component在server和浏览器都renderer

DEV和PROD的区别
dev每次刷新浏览器，server都rerender一次
next build+start，build得到所有static文件和js文件chunks，run start启动项目，浏览器刷新不会重新rerender server，

Vercel —— nodejs server 用来部署prod

A标签会从新load所有的html的docs从server，所以使用nextjs自带的link标签，click的时候没有任何请求，可以理解成只replace一些components内部的，而没有请求文件

在dev模式下，hover到link标签上，会prerender一些文件

在prod模式，hover 加载的一些文件，在prod中全部加载，而不是等hover，这些作为extra data，那么如果某一个page非常大，如果一起preload会影响，可以通过这时prefetch false来阻止加载，但是如果没有preload，那么在prod模式下click就会发起请求， 默认是都prefetch的

切换link时，layout不动，只有main里面的children改变


style
安装tailwind，根据使用jsx和tsx修改配置文件，按需，引入base component到globalcss中

alias，使用components时简化引入索引，jsconfig.json， compilerOptions


google font

next会在build的时候下载，并且用static的形式放在项目里, nextjs会自动引入，已变量的形式添加类名，

可以利用tailwind，定义fontfaimly，传入数组变量名，和对应的变量值，变量值可以再font.js里面自定义variable


read md files

marked dependency — 从md文件读取为html文件，传入article，

使用tailwind的prose 来自动定义样式

还可以用gray matter来获取对象形式的信息

改原来的具体的page 为slug 并可以从props中获取具体的page slug，获取md文件也用对应page的slug name

带有slug的build之后，打包文件将不再是static的⭕️，而是λ， which means这些page都是runtime加载 这不是我们想要的

在slug的page里面添加generateStaticPrams，并返回数组中含有slug对应的名字，就可以将λ在build之后变成○，which means static html，这样之后浏览器不会在runtime加载，都是静态文件


metadata

title— 自动应用在每个page  

nextjs支持icon img 


client component

有一些交互的行为，component需要“use client”，html文件会提前加载，同时返回带有client component中交互的js code，所有使用了client的组件内部含有的组件都是client component

但是使用了server上的方法 的组件是不能使用use client的，因为这些方法都运行在nodejs上

Nest client component 


deploy

静态的包可以放在任何server上

nextjs需要nodejs server，Vercel，Netlify， AWS Amplify。。。

使用vercel：链接git 部署 update code 直接rebuild

STRAPI

content management system
可以自定义文章内容，并通过设置api获取相应的文章列表及内容

类似后台管理系统，可以通过添加collection来自定义各部分内容

Fetch data from cms system


图片的优化
用nextjs的image component， render的size会被优化srcset属性用来设置， 同时对格式进行优化，


分页

pagination传参中可以添加除了pagesize之外第二个page的参数，来进行分页获取数据


client component
如果cc中需要load数据，可以在servver component中引入cc，然后在sc中call 获取数据并传递给cc，以props的形式
实际上 在html中以json的格式，所有数据props 传递给client的都是

但是如果是上千万的就会导致html很大


如果做到query时发出请求获取数据,可以根据参数控住api返回数量 比前面全部加到html中要好控制

useEffect(() => {
    If (query.length>1) {
        (Async () => {
            Const reveiws = await getreviews()
            Setreveiw(reviews)
        })()
    }
}, [])


但是此时发请求是1337的strapi server不是从前端的url访问的 可以利用server-only这个插件 将reveiew限制在server 而在前端组件中 getreviews直接通过fetch获取


Deploy

Env 变量  nextjs中的.env文件可以配置环境变量，在code中使用

























