# Koa框架基础学习笔记（未完待续）

![](https://www.ruanyifeng.com/blogimg/asset/2017/bg2017080801.png)

## 理论简介：

koa 是由 Express 原班人马打造的，致力于成为一个更小、更富有表现力、更健壮的 Web 框架。使用 koa 编写 web 应用，**通过组合不同的 generator，可以免除重复繁琐的回调函数嵌套**，并极大地提升错误处理的效率。koa 不在内核方法中绑定任何中间件，它仅仅提供了一个轻量优雅的函数库，使得编写 Web 应用变得得心应手。



## 快速入门Koa框架：

**创建Koa框架整体分为四步：**

1. 导入Koa包
2. 实例化基于koa的对象app
3. 编写相应中间件（配置路由）
4. 开启服务器监听



> **快速入门案例：**



```js
const Koa=require("koa");//导入koa包


const app=new Koa();//实例app对象

app.use((ctx)=>{
    ctx.body="Hello,I am Koa3";
})//编写中间件




app.listen(7005,()=>{
    console.log("Koa框架服务器已经启动-----7005");
})
```





## 初步认识中间件与洋葱圈模型：



> **洋葱圈模型概念：先由最外层中间件逐层深入，执行next()之前的代码**
>
> **最后递归后回调，由最深层推出至最外层中间件，执行next()之后的代码**



![](https://tse3-mm.cn.bing.net/th/id/OIP-C.cGJ5OCf6sMr9egqC6smZ7AHaGv?pid=ImgDet&rs=1)

### **相关代码小案例：（洋葱圈模型的代码还未上传）**

```js
const Koa=require("koa");

const app=new Koa();

app.use((ctx,next)=>{·
    console.log("我是第一中间件");
    next();
})
.use((ctx,next)=>{
    console.log("我是第二中间件");
    next();
})
.use((ctx,next)=>{
    console.log("我是第三中间件");
    ctx.body="hahaha";
})


app.listen(7005,()=>{
    console.log("启动成功");
})
```





### 异步数据处理（相较于express最大的差异点）

> **重点：可以使用Async await 关键字进行异步等待操作**





### **小案例：**



```js

const { default: axios } = require("axios");
const Koa=require("koa");

const app=new Koa();

app.use(async(ctx,next)=>{
    ctx.score=100;
    await next();
})
.use(async(ctx,next)=>{
    await axios.get("https://apii.hfliu.com/default/default/getArticleList")//个人博客网站的后端接口
    .then((val)=>{
        ctx.list=val.data
        console.log(val.data);
    })
    next();
})//进行数据请求或与数据库的交互操作


.use(async(ctx,next)=>{

    console.log(1111);
     ctx.body=ctx.list

})
//中间件的编写支持链式调用

app.listen(7005,()=>{
    console.log("启动成功");
})
```





## 路由配置（更优雅的处理路由问题）



**操作：**

- 下载并导入koa-router这个包
- 直接调用koa-router的相应api即可配置相应的get/post请求的接口
- 引入router文件进行路由的统一管理



### 使用原生自写的形式（很不优雅）：

```js
const Koa=require("koa");

const app=new Koa();

app.use(async(ctx,next)=>{


    console.log(ctx.request.url);



    if(ctx.request.url=="/"){
        ctx.response.body="首页"
    }


    else if(ctx.request.url=="/user"){

        if(ctx.request.method=="GET"){
            ctx.response.body="用户页面"
        }

        else if(ctx.request.method=="POST"){
            ctx.response.body="创建用户"
        }
        else{
            ctx.response.status=405
        }
    }

    else{
        ctx.response.status=404
    }

})



app.listen(7005,()=>{
    console.log("启动成功");
})
```





### 使用Koa-router包的形式（优雅很多）：

```js

//入口文件------main.js

const Koa=require("koa");
const Router=require("koa-router");//导入koa-router的包


const app=new Koa();
const router=require("../router/userRoute")




app.use(router.routes())//注册路由中间件
app.use(router.allowedMethods())//用来自动url路径返回405与501的错误信息给前台

app.listen(7006,()=>{
    console.log("启动成功");
})



//路由文件--------- userRoute.js


const Router=require("koa-router");//导入koa-router的包

const router=new Router();

router.get("/",(ctx)=>{
    ctx.body="hahaha"
})

router.get("/user",(ctx)=>{
    ctx.body="用户页hahaha"
})

router.post("/user",(ctx)=>{
    console.log(ctx.request);
    ctx.body="创建用户hahahha"
})

router.post("/user/modify",(ctx)=>{
    ctx.body="修改用户"
})

module.exports=router;




```







## 数据请求处理：

**三种最常用解析：**

- **GET方式的params参数（ctx.params）**
- **GET方式的query参数（ctx.query）**
- **POST方式（ctx.request.body----------需要引入koa-body）**



### 小案例：

```js

//入口文件------main.js


 const Koa=require("koa");
 const {koaBody}=require("koa-body");
 const router=require("../router/second")
 
 
 const app=new Koa();
 app.use(koaBody());//注册body的全局中间件

 
 
 app.use(router.routes())//注册路由中间件
 app.use(router.allowedMethods())//用来自动url路径返回405与501的错误信息给前台
 
 app.listen(7006,()=>{
     console.log("启动成功");
 })




//路由文件--------- second.js


const Router=require("koa-router");//导入koa-router的包

const router=new Router();


const modDB=[
    {id:1,userName:"张三",age:11},
    {id:2,userName:"李四",age:12},
    {id:3,userName:"王五",age:13},
    {id:4,userName:"老六",age:14},
    {id:5,userName:"吴七",age:15},
    {id:6,userName:"老八",age:18},
    {id:7,userName:"9999",age:19},
]

router.get("/all",(ctx)=>{
    ctx.body=modDB
})

router.get("/:id",(ctx)=>{

      const {id}=ctx.params;

      let target=modDB.filter((val)=>{
        return val.id==id
      })
      ctx.body=target;
})


router.get("/",(ctx)=>{
    console.log(ctx.query);
    const {id}=ctx.query;
    let target=modDB.filter((val)=>{
        return val.id==id
      })
      if(target.length==0){
        ctx.throw(404)//数据越界，抛出404异常
      }
    ctx.body=target
})

router.post("/user",(ctx)=>{
    console.log(ctx.request.body);
    ctx.body="创建用户hahahha"
})

router.post("/user/modify",(ctx)=>{
    ctx.body="修改用户"
})

module.exports=router;


```



## 错误处理机制：

### 原生形式：

```js
//第一种类型：请求参数错误

        ctx.throw(422,"参数不合理！！！")//直接使用koa提供的抛出机制


//第二种类型：服务器处理时的运行时异常

        try().............catch{}//直接使用try....catch错误检验机制
```



### 社区中间件形式（koa-json-error）：

**利用该中间件可以直接展示出Restful形式的json数据：**

```js

//进行中间件的搭载与配置

 const error=require("koa-json-error");
app.use(error({
    format:(error)=>{
        return {status:error.status,message:error.message}
    }//设置传送错的json格式，具体结果不能传递给前端
 }));//注册错误传送中间件


 ctx.throw(422,"参数不合理！！！")//使用错误级别中间件


```



```json
{
	"status": 422,
	"message": "参数不合理！！！"
}
//前端接收到的后台数据（restful风格）
```



## 跨域处理机制：

### 最简单的办法：

**直接使用koa-cors中间件**

```js
 const cors=require("koa-cors");//导入koa-cors中间件
 
 
  app.use(cors());//注册跨域问题解决的中间件
```





