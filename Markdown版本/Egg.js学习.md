## Egg.js学习

## 1.项目与环境搭建

```javascript
//使用npm进行初始化：
/*
$ mkdir egg-example && cd egg-example
$ npm init egg --type=simple
$ npm i
$ npm run dev
$ open http://localhost:7001


//使用yarn进行


mkdir egg-example && cd egg-example
npm i yarn -g
yarn create egg --type=simple
yarn install
yarn dev(start启动不具备实时刷新的效果，dev启动可以)
*/
```

<img src="C:/Users/M/AppData/Roaming/Typora/typora-user-images/image-20230115170054432.png" alt="image-20230115170054432" style="zoom:50%;" />





## 2.项目相关环境文件

```javascript
/*
- app                        - 项目开发的主目录，工作中的代码几乎都写在这里面
-- controller                -- 控制器目录，所有的控制器都写在这个里面
-- router.js                 -- 项目的路由文件
- config                     - 项目配置目录，比如插件相关的配置
-- config.default.js         -- 系统默认配置文件
-- plugin.js                 -- 插件配置文件
- logs                       -- 项目启动后的日志文件夹
- node_modules               - 项目的运行/开发依赖包，都会放到这个文件夹下面
- test                       - 项目测试/单元测试时使用的目录
- run                        - 项目启动后生成的临时文件，用于保证项目正确运行
- typings                    - TypeScript配置目录，说明项目可以使用TS开发
- .eslintignore              - ESLint配置文件
- .eslintrc                  - ESLint配置文件，语法规则的详细配置文件
- .gitignore                 - git相关配置文件，比如那些文件归于Git管理，那些不需要
- jsconfig.js                - js配置文件，可以对所在目录下的所有JS代码个性化支持
- package.json               - 项目管理文件，包含包管理文件和命令管理文件
- README.MD                  - 项目描述文件  
*/
```



**yarn命令中dev与start的关系与差别**

- dev : 开发环境中使用，不用重启服务器，只要刷新。修改内容就会更改。

- start：生产环境中使用，也就是开发完成，正式运营之后。以服务的方式运行。修改后要停止`（yarn stop）`和重启后才会发生改变。

- 

- 

  **官方解析：**

  ```javascript
  "scripts": {
      "start": "egg-scripts start --daemon --title=egg-server-egg",
      "stop": "egg-scripts stop --title=egg-server-egg",
      "dev": "egg-bin dev",
      "debug": "egg-bin debug",
      "test": "npm run lint -- --fix && npm run test-local",
      "test-local": "egg-bin test",
      "cov": "egg-bin cov",
      "lint": "eslint .",
      "ci": "npm run lint && npm run cov",
      "autod": "autod"
    },
  ```



## 3.controller  控制器的使用：



原始环境中的样子（添加了一个简单的）

```javascript

// app/controller/home.js（.controller部分）
const Controller = require('egg').Controller;

class HomeController extends Controller {
  async index() {
    this.ctx.body = 'Hello world';
  }
   async hello() {
    this.ctx.body = 'Hello egg.js';
  }
}

module.exports = HomeController;

// app/router.js（路由映射部分）
module.exports = (app) => {
  const { router, controller } = app;
  router.get('/', controller.home.index);
  router.get('/', controller.home.index);//配置相应的路由映射关系
    
};
```



**创建一个简单的controller（属于html类型的）**

```javascript
"use strict"

const Controller=require("egg").Controller

class hfLiuController extends Controller {
    async index() {
      const { ctx } = this;
      ctx.body = '<h1>这里是一个controller，属于html类型的</h1>';
    }//创建一个异步的index方法
  }//创建一个controller（属于根据url，返回资源的形式）

module.exports=hfLiuController;
  
  
  
  'use strict';

/**
 * @param {Egg.Application} app - egg application
 */
module.exports = app => {
  const { router, controller } = app;
  router.get('/', controller.home.index);
  router.get('/hfliu', controller.home.hfliu);
  router.get("/new",controller.hfLiu.index);
};

```







### controller'层次的理论：

**`ctx.query`：URL 中的请求参数（忽略重复 key）**

`ctx.quries`：URL 中的请求参数（重复的 key 被放入数组中）

`ctx.params`：Router 上的命名参数

**`ctx.request.body`：HTTP 请求体中的内容**

**`ctx.request.files`：前端上传的文件对象**

`ctx.getFileStream()`：获取上传的文件流

**`ctx.multipart()`：获取 `multipart/form-data` 数据**

**`ctx.cookies`：读取和设置 cookie**

**`ctx.session`：读取和设置 session**

**`ctx.service.xxx`：获取指定 service 对象的实例（懒加载）**

`ctx.status`：设置状态码

`ctx.body`：设置响应体

`ctx.set`：设置响应头

`ctx.redirect(url)`：重定向

**`ctx.render(template)`：渲染模板**



## 4.单元测试的编写

**每写完一个小的模块demo，就要进行相应的单元测试：**



**一个简单的普通类型单元测试：**

```javascript
//对上面写的hfLiu.js进行测试


'use strict';

const { app } = require('egg-mock/bootstrap');

describe('hfLiu.js的测试', () => {

  it('每个方法测试的描述', () => {
    return app.httpRequest()
      .get('/new')// 测试的位置
      .expect('<h1>这里是一个controller，属于html类型的</h1>')// 期望得到的结果
      .expect(200);// 期望得到的返回验证码
  });
});

```





异步的单元测试案例：

```javascript
//先编写一个异步的测试方法：
  async asyncWays(){
    const { ctx } = this;

    await new Promise(resolve => {
        setTimeout(()=>{
          resolve(ctx.body="<h1>这是一个异步的方法，延时时间5秒</h1>")
        },5000)
    })

  }


//在配置相应的路由映射
router.get('/asyncs', controller.hfLiu.asyncWays);


//随后，编写相应的测试程序
'use strict';

const { app } = require('egg-mock/bootstrap');

describe('hfLiu.js的测试', () => {

  it('每个方法测试的描述', () => {
    return app.httpRequest()
      .get('/new')// 测试的位置
      .expect('<h1>这里是一个controller，属于html类型的</h1>')// 期望得到的结果
      .expect(200);// 期望得到的返回验证码
  });


  it('每个方法测试的描述', async () => {//编写async关键字，说明本次的测试方法是一个异步测试
    return app.httpRequest()
      .get('/asyncs')// 测试的位置
      .expect('<h1>这是一个异步的方法，延时时间5秒</h1>')// 期望得到的结果
      .expect(200);// 期望得到的返回验证码
  });
});

```







## 5.GET请求与参数传递

```javascript
//自由传参的形式
  async testGet() {
    const { ctx } = this;
    ctx.body=ctx.query;
    console.log(ctx.body);
  }


  //严格传参的形式
  async testGetStrict() {
    const { ctx } = this;
    ctx.body=`这是一个Object,接受到的name为${ctx.params.name},接受到的age为${ctx.params.age}`
    console.log(ctx.body);
  }


  router.get('/testGet', controller.hfLiu.testGet);
  router.get('/testGetStrict/:name/:age', controller.hfLiu.testGetStrict);

//请求的时候直接追加query参数即可
```



## 6.post请求

```javascript
   
   async testPost() {
        const { ctx } = this;
        ctx.body={
            status:200,
            data:{name:"小明",age:18}
        };
        console.log(ctx.request.body);//获取post的请求体数据
        console.log(ctx.request.query);//获取post的一般url参数
      }
      

        router.post('/testPost', controller.hfLiu.testPost);

//post的body里面的不管说明类型，egg里面都已经封装好了，可以直接将数据解析
```





## 7.server层基础使用：



**server抽离出来的层专门作与数据库操作相关的方法**



简单来说，就是把业务逻辑代码进一步细化和分类，所以和数据库交互的代码都放到Service中。这样作有三个明显的好处。

- 保持Controller中的逻辑更加简介。
- 保持业务逻辑的独立性，抽象出来的Service可以被多个Controller调用。
- 将逻辑和展现分离，更容易编写测试用例。

个人建议只要是和数据库的交互操作，都写在Service里，用了Egg框架，就要遵守它的约定。



```javascript
//service层面封装的
'use strict';

const Service = require("egg").Service;

class hfLiuServiceTest extends Service{
    async getWay(id){
        //因为没有真实连接数据库，所以模拟数据
        return {
            id:id,
            name:'小张',
            age:19
        }

    }
}

module.exports =hfLiuServiceTest;//该文件将hfLiuServiceTest中所有的操作方法



//controsellor方面的
      async testServie() {
        const { ctx } = this;
        console.log(ctx.query);
        let res=await ctx.service.hfLiuService.getWay(ctx.request.query.idx);//hfLiuservice是那个service的文件名，
        ctx.body=res;
      }
      
      
      
      
  //路由方面：
    router.get("/testServie",controller.hfLiu.testServie)

```



## **8.View层模板引擎的基本使用：**

### **理论与原因：**

### 基本使用：

**使用ejs模板引擎，设置相关config设置文件：**

```

```

**先要在App内部创建一个view层（用于放置相关html页面）**

```html
创建一个testViewOne.html文件：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Hello,I am LLLLLLLLLLLKKKKKKKKK</h1>
</body>
</html>
```

```javascript
//具体方法相关的编写：
      async testViewOne() {
        const { ctx } = this;
        await ctx.render("ViewOne.html");
      }


//路由的设置与一般一样
  router.get("/testViewOne",controller.hfLiu.testViewOne)
```



## **9.ejs相关语法与向html传入数据：**

```javascript
//方法的编写

      async testViewOne() {
        const { ctx } = this;
        await ctx.render("ViewOne.html",{
          userName:"lfff",
          password:"14235432",
          codeNum:[888,666,444],
        });
      }

```

```html
<!-- html页面的编写  -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Hello,I am LLLLLLLLLLLKKKKKKKKK</h1>
    <h2>接受到了数据有  用户名： <%= userName %>  密码：<%= password%></h2>
    <h3>还接收到了一串数字：</h3>
    <ul>
        <% for(var i=0;i<codeNum.length;i++){%>
            <li><%= codeNum[i]%></li>
            <% }%>
    </ul>
</body>
</html>

<!-- 注意，等号必须放在简括号的旁边  <=    变量隔开没关系-->
```





### view层中html公共数据引入：

```html
<!-- 编写一个新的公共页面 header.html -->
<h1>Hello,World</h1>


<!-- 应用html页面中加入这一行话 -->
    <%- include("header.html") -%>
    <h1>Hello,I am LLLLLLLLLLLKKKKKKKKK</h1>
```



静态资源的使用（css js png与在一般html页面引入都是一样的）

```css
在public文件夹中创建一个One.css文件
body{
    color: aquamarine;
}
```



```html
在html界面中引入
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="../public/One.css">
    <title>Document</title>
```





## **10.cookie的相关编写：**





### 理论：

Cookie由变量名和值组成，类似Javascript变量。其属性里既有标准的Cookie变量，也有用户自己创建的变量，属性中变量是用“变量=值”形式来保存。

根据Netscape公司的规定，Cookie格式如下：
**Set-cookie: NAME=VALUE Expires/Max-age=DATE Path=PATH Domain=DOMAIN_NAME SECURE**

参数意义:

**NAME: cookie的名字。**

**VALUE: cookie的值。**

**Expires: cookie的过期时间。**

**Path: cookie作用的路径。**

**Domain: cookie作用的域名。**

**SECURE:是否只在https协议下起作用**





### cookie的一般增删改查操作：

```javascript
      //添加cookie
      async addCookie() {
        const { ctx } = this;
        ctx.cookies.set("user","hf12345.com");
        console.log(ctx.cookies.get("user"));
        ctx.body={
          status:200,
          data:"cookie返回成功！"
        }
      }


      //删除cookie
      async delCookie() {
        const { ctx } = this;
        ctx.cookies.set("user",null);
        ctx.body={
          status:200,
          data:"cookie删除成功！"
        }
      }

      //修改cookie
      async ediCookie() {
        const { ctx } = this;
        ctx.cookies.set("user","react.com");
        ctx.body={
          status:200,
          data:"cookie修改成功！"
        }
      }

      //查看cookie
      async showCookie() {
        const { ctx } = this;

        let value=ctx.cookies.get("user");
        console.log(ctx.request.body.name);
        console.log(value);
      }




  router.post("/addCookie",controller.hfLiu.addCookie)
  router.post("/delCookie",controller.hfLiu.delCookie)
  router.post("/ediCookie",controller.hfLiu.ediCookie)
  router.post("/showCookie",controller.hfLiu.showCookie)
```





### cookie的其他编写（时限，httponly 是否加密...）

```javascript
      //添加cookie
      async addCookie() {
        const { ctx } = this;
        ctx.cookies.set("user","hf12345.com",{
          maxAge:1000*600,//最长时限：1000毫秒（1秒）乘600
          httpOnly:true,//是否仅仅允许http进行请求，true后客户端不能进行请求，一般都直接true
          encrypt:true,//是否进行加密
        });
        ctx.body={
          status:200,
          data:"cookie返回成功！"
        }
      }




      //查看cookie
      async showCookie() {
        const { ctx } = this;

        let value=ctx.cookies.get("user",{
          encrypt:true//展示加密的cookie需要启动该属性
        });
        console.log(value);
        ctx.body={
          status:200,
          data:"cookie查看成功！"
        }
      }
```



## **11.session的相关编写：**

**简单的增删改查：**

```javascript
        ctx.session.id="4321trew";//添加session//会自动进行加密
        ctx.session.id=null;//删除session
        ctx.session.id="react";
        const sessionData=ctx.session.id;//会自动进行解密
```



**相关的属性设置：******（需要到config文件中进行修改）****

```javascript

  config.session={
    key:"Feng_SESS",
    maxAge:1000*600,
    httpOnly:true,
    renew:true//用户操作后，系统重新计时1000*600
    //(进而实现用户在无操作10分钟之后才会重新登录)
  }

```



**11.中间件的相关使用：**



**1.首先，必须要在APP文件夹中创建middleware文件夹**

### 全局中间件：

```javascript
//建立countAll.js文件
module.exports=(option)=>{
    return async (ctx,next)=>{
        if(ctx.session.count){
            ctx.session.count++;
        }
        else{
            ctx.session.count=1;
        }
        console.log("有人请求服务器了  这是启动以来的第"+ctx.session.count+"次");
        await next();
    }
}


//congfig文件中进行设置
  config.middleware = ["counterAll"];//配置了countAll这个全局中间件
```



### 局部的路由中间件：

```javascript
//建立countAdd.js文件
module.exports=(option)=>{
    return async (ctx,next)=>{
        if(ctx.session.countAdd){
            ctx.session.countAdd++;
        }
        else{
            ctx.session.countAdd=1;
        }
        console.log("有人请求Add服务器了  这是启动以来的第"+ctx.session.countAdd+"次");
        await next();
    }
}



//在router.js中
  const countAdds=app.middleware.countAdd();//配置countAdd这个路由中间件，下面要去相应的路由应用中间件
    router.post("/addCookie",countAdds,controller.hfLiu.addCookie)//第二个参数即为要配置的路由中间件
```







## 12.对象拓展：

**首先，都必须在extend文件夹中进行编写**

<img src="C:/Users/M/Desktop/Egg.js-Extends.png" alt="Egg.js-Extends" style="zoom: 50%;" />





**application的应用拓展：**



```javascript
//application.js中的文件
module.exports={
    getTime(){
        return getNowTime();
    },//拓展app的方法

    get systemTime(){
        return getNowTime();
    }//拓展app的属性

}

function getNowTime(){
    let  now = new Date();
    let year = now.getFullYear(); //得到年份
    let  month = now.getMonth()+1;//得到月份
    let date = now.getDate();//得到日期
    let  hour= now.getHours();//得到小时数
    let minute= now.getMinutes();//得到分钟数
    let second= now.getSeconds();//得到秒数
    let nowTime = year+'年'+month+'月'+date+'日 '+hour+':'+minute+':'+second;
    return nowTime;
}



//全局任何地方都可以直接使用
  const { ctx,app } = this;
  console.log("现在的时间为：",app.getTime());
  console.log("现在的系统时间为：",app.systemTime);
  
```

 







## 13.定时任务的编写：



**首先创建schedule文件夹**

```javascript
//一个简单的定时任务：
const Subscription=require("egg").Subscription

class getTime extends Subscription{
    static get schedule(){
        return{
            interval:"2s",
            type:"worker"
        }
    }//配置定时相关信息

    async subscribe(){
        console.log(Date.now());
    }//编写你定时需要执行的任务
}
module.exports=getTime;




//复杂的时间
//使用cron属性进行设置
```





## 14.数据库的连接与基本操作：

### 连接mysql数据库：

```javascript
//在plugin文件中进行配置：
exports.mysql={
  enable:true,
  package:"egg-mysql",
}

//在default Config文件中进行配置：

  config.mysql ={
    app:true,     //是否挂载到app下面
    agent:false,  //是否挂载到代理下面
    client:{
      host:'127.0.0.1',      // 数据库地址
      prot:'3306',           // 端口
      user:'root',           // 用户名
      password:'111root',    // 密码
      database:'testegg'    // 连接的数据库名称
    }
  }
```





### 对数据库的基本操作：

```javascript

//service中的testdb文件：
"use strict";

const Controller = require("egg").Controller;

class listManage extends Controller {
  async addList() {
    const { ctx } = this;
    let params={
        name:"小刘",age:"22"
    }
    let res=await ctx.service.testdb.addList(params);
    console.log(res);
    ctx.body = "添加列表成功";
  }

  async delList() {
    const { ctx } = this;
    let id={id:3};
    let res=ctx.service.testdb.delList(id);


    if(res){
      ctx.body="删除列表成功"
    }
    else{
      ctx.bod="删除列表失败"
    }
  }

  async updateList() {
    const { ctx } = this;
    const params={id:1,name:"lccccc",age:120}
    let res=ctx.service.testdb.updateList(params);
    if(res){
      ctx.body="修改列表成功"
    }
    else{
      ctx.bod="修改列表失败"
    }

  }

  async getList() {
    const { ctx } = this;
    let res=await ctx.service.testdb.getList();

    ctx.body = res;
  }
}

module.exports = listManage;





//在controller文件夹中的managedb1.js文件：
//两个文件信息反了

"use strict";

const Controller = require("egg").Controller;

class listManage extends Controller {
  async addList() {
    const { ctx } = this;
    let params={
        name:"小刘",age:"22"
    }
    let res=await ctx.service.testdb.addList(params);
    console.log(res);
    ctx.body = "添加列表成功";
  }

  async delList() {
    const { ctx } = this;
    let id={id:3};
    let res=ctx.service.testdb.delList(id);


    if(res){
      ctx.body="删除列表成功"
    }
    else{
      ctx.bod="删除列表失败"
    }
  }

  async updateList() {
    const { ctx } = this;
    const params={id:1,name:"lccccc",age:120}
    let res=ctx.service.testdb.updateList(params);
    if(res){
      ctx.body="修改列表成功"
    }
    else{
      ctx.bod="修改列表失败"
    }

  }

  async getList() {
    const { ctx } = this;
    let res=await ctx.service.testdb.getList();

    ctx.body = res;
  }
}

module.exports = listManage;




//进行路由映射的配置：

  router.get("/addList",controller.mangedb1.addList)
  router.get("/delList",controller.mangedb1.delList)
  router.get("/updateList",controller.mangedb1.updateList)
  router.get("/getList",controller.mangedb1.getList)
```









## 15.整体总结：





**一个完整的egg项目整体结构：**



```javascript
/*

egg-project
├── package.json
├── app.js (可选)
├── agent.js (可选)
├── app/
|   ├── router.js # 用于配置 URL 路由规则
│   ├── controller/ # 用于存放控制器（解析用户的输入、加工处理、返回结果）
│   ├── model/ (可选) # 用于存放数据库模型
│   ├── service/ (可选) # 用于编写业务逻辑层
│   ├── middleware/ (可选) # 用于编写中间件
│   ├── schedule/ (可选) # 用于设置定时任务
│   ├── public/ (可选) # 用于放置静态资源
│   ├── view/ (可选) # 用于放置模板文件
│   └── extend/ (可选) # 用于框架的扩展
│       ├── helper.js (可选)
│       ├── request.js (可选)
│       ├── response.js (可选)
│       ├── context.js (可选)
│       ├── application.js (可选)
│       └── agent.js (可选)
├── config/
|   ├── plugin.js # 用于配置需要加载的插件
|   ├── config.{env}.js # 用于编写配置文件（env 可以是 default,prod,test,local,unittest）

*/
```







**controller层次的一般需求**

- 接收、校验、处理 HTTP 请求参数
- 向下调用服务（Service）处理业务
- 通过 HTTP 将结果响应给用户（包含设置cookie session token令牌等等）

**service层次的一般需求：**

- 处理复杂业务逻辑
- 调用数据库或第三方服务（例如 GitHub 信息获取等）

**定时任务的可能需求：**

- 每天检查一下是否有用户过生日，自动发送生日祝福
- 每天备份一下数据库，防止操作不当导致数据丢失
- 每周删除一次临时文件，释放磁盘空间
- 定时从远程接口获取数据，更新本地缓存





参考文章：

1. 

