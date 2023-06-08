# React脚手架与V5路由

## 1----React脚手架：

### 1.1基本使用（创建一个脚手架项目）

首先你需要确保你安装了 `Node >= 8.10` 和 `npm >= 5.6`，详细环境搭建请详见[前端环境搭建](https://link.juejin.cn?target=..%2F%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7%E7%AF%87%2F%E5%89%8D%E7%AB%AF%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA)

然后在需要创建项目的文件夹打开命令行，执行

```shell
npx create-react-app my-app
cd my-app
npm start
```

> **注意：** 第一行的 npx 不是拼写错误 —— 它是 npm 5.2+ 附带的 package 运行工具。

此处的 **`my-app`就是项目的名称**，可以自己替换，但是**不能出现中文和大写字母**。创建完成之后会创建一个名为 `my-app` 的文件夹，进入这个文件夹之后，**执行 `npm start` 就可以将项目跑起来了。**





### 1.2脚手架目录结构：

```javascript
my-app
├─ README.md // readme说明文档
├─ package.json // 对整个应用程序的描述：包括应用名称、版本号、一些依赖包、以及项目的启动、打包等等（node管理项目必备文件）
├─ public
│    ├─ favicon.ico // 应用程序顶部的icon图标
│    ├─ index.html // 应用的index.html入口文件
│    ├─ logo192.png // 被在manifest.json中使用
│    ├─ logo512.png // 被在manifest.json中使用
│    ├─ manifest.json // 和Web app配置相关
│    └─ robots.txt // 指定搜索引擎可以或者无法爬取哪些文件
├─ src // 基本所有开发都在这个文件夹中进行
│    ├─ App.css // App组件相关的样式
│    ├─ App.js // App组件的代码文件
│    ├─ App.test.js // App组件的测试代码文件
│    ├─ index.css // 全局的样式文件
│    ├─ index.js // 整个应用程序的入口文件
│    ├─ logo.svg // 刚才启动项目，所看到的React图标
│    ├─ serviceWorker.js // 默认帮助我们写好的注册PWA相关的代码
│    └─ setupTests.js // 测试初始化文件
├─ node_modules // 所有依赖安装文件夹
└─ yarn.lock // 依赖本地下载版本管理文件
```

### 1.3包管理工具概述：



我们之前提到过，脚手架的其中一个作用就是帮助我们管理第三方依赖，那么我们该如何在我们的项目中对第三方依赖进行管理呢？我们使用包管理工具来进行统一管理。

前端有两个包管理工具可以使用，一个是 `npm`，一个是 `yarn`。我们先介绍 `npm`。

**npm**------------------------------------------------------------------

`npm` 的全称是 `Node Package Manager` ，是 `node` 的包管理工具，它在你安装 `node` 环境时就已经自动安装了。

`npm` 的作用是帮助我们管理一下依赖的工具包，它会将你需要的依赖以及需要的版本号记录在 `package.json` 里，所以每次传输时就不需要反复传输 `node_modules`，只需要在使用的时候使用 `npm install` 对依赖进行安装就可以使用了。

**yarn**-------------------------------------------------------------------

`Yarn` 是由`Facebook`、`Google`、`Exponent` 和 `Tilde` 联合推出了一个新的 `JS` 包管理工具。

因为早期 `npm` 存在许多缺陷，比如安装依赖速度慢，版本混乱，所以上面这些公司联合推出了 `yarn` 来解决这些问题。

如今 `npm` 在推出 5.0 版本后已经解决了许多问题，两者并没有特别明显的区别。不过 `React` 脚手架默认使用 `yarn` 进行管理，所以我们建议你使用 `yarn` 进行依赖管理。

**常用指令**-----------------------------------------------------------------

| 功能     | npm                 | yarn              |
| -------- | ------------------- | ----------------- |
| 安装依赖 | npm install         | yarn              |
| 新增依赖 | npm install react   | yarn add react    |
| 卸载依赖 | npm uninstall react | yarn remove react |
| 执行命令 | npm run serve       | yarn serve        |

链接：https://juejin.cn/post/6974973679985917966



## 2----基本操作（一个Hello组件）

代码：

**index.html代码：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>React脚手架初步</title>
    <link rel="icon" href="./favicon.ico" />
    <meta name="theme-color" content="#000000" />
</head>
<body>
    <div id="root"></div>
</body>
</html>
```



**App.jsx的代码：**

```jsx
import React from "react";
import Hello from "./Components/hello/Hello";
import Welcome from "./Components/welcome/Welcome"

class App extends React.Component{
    render(){
        return(
            <div>
                <Hello/>
                <hr/>
                <Welcome/>
            </div>
        )
    }
}


export default App;//采用默认暴露的形式暴露App组件
```

**index.jsx的代码：**

```jsx
import React from "react";//安装React核心库
import ReactDOM from "react-dom/client";//安装ReactDOM库文件
import App from "./App.js";//引入App函数组件

//ReactDOM.render(<App/>,document.getElementById("root"));//渲染组件到容器身上
const container = document.getElementById('root');
const root = ReactDOM.createRoot(container);
root.render(<App/>);//react18下需要这样将组件渲染到容器
```

**components文件夹下的hello组件的jsx代码：**

```jsx
import React from "react";
import './Hello.css'


class Hello extends React.Component{
    render(){
        return(
            <h2 className="hello_M">Hello_React!!!</h2>
        )
    }
}

export default Hello
```

**components文件夹下的welcome组件的jsx代码：**

```jsx
import React from "react";
import './Welcome.css'


class Welcome extends React.Component{
    render(){
        return(
            <h2 className="title">Welcome to you</h2>
        )
    }
}

export default Welcome

```





## 3---小案例1（ToDoList案例）

```jsx
//App.jsx文件：
import React, { Component } from 'react'
import "./App.css";
import Footer from './components/Footer/index';
import Header from './components/Header/index';
import List from './components/List/index';
import { nanoid } from 'nanoid';
export default class App extends Component {
    state={todos:[
        {id:1,things:"吃饭",done:false,time:[8,20]},
        {id:2,things:"写作业",done:false,time:[11,20]},
        {id:3,things:"睡觉",done:true,time:[20,30]},
        {id:4,things:"出去",done:true,time:[10,20]},

    ]}

    getData=(data,timeDate)=>{
        const nowData={id:nanoid(),things:data,done:false,time:timeDate};
        this.setState({todos:[nowData,...this.state.todos]});
    }
    getIsAllFin=(flag)=>{
        let {todos}=this.state;

        let newTodos=todos.map((todoObj)=>{
            return {...todoObj,done:flag};
        })
       
        this.setState({todos:newTodos});
    }
    getDeleteId=(ids)=>{
        const {todos}=this.state;
        todos.forEach((value)=>{
            if(value.id===ids){
                let flagDelete=window.confirm(`你确定要删除  ${value.things}  么？`);
                if(flagDelete===true){
                    let newTodos=todos.filter((value)=>{

                        if(flagDelete===true){}
                        return value.id!==ids;
                    })
                    this.setState({todos:newTodos})
                }
                else{
                    return;
                }
            }
        })
    }
    deleteAllPassed=()=>{
        const {todos}=this.state;
        let flagDelete=window.confirm("你确定要删除已完成的项目么？");
        if(flagDelete===true){
            let newTodos=todos.filter((value)=>{
                return value.done!==true;
            });
            this.setState({todos:newTodos});
        }
        else{
            return;
        }
    }
    getChecked=(ids,target)=>{
         let {todos}=this.state;
        // todos.forEach((value)=>{
        //     if(value.id===ids){
        //         value.done=target;
        //         return 
        //     }
        // })

        let newTodos=todos.map((value)=>{
            if(value.id===ids){
                return {...value,done:target}
            }
            else{
                return value
            }
        })
        this.setState({todos:[...newTodos]})
    }
  render() {
    return (
        <div className="todo-container">
            <div className="todo-wrap">
                <Header getData={this.getData}/>
                <List todos={this.state.todos} getChecked={this.getChecked} getDeleteId={this.getDeleteId}/>
                <Footer todos={this.state.todos} getIsAllFin={this.getIsAllFin} deleteAllPassed={this.deleteAllPassed}/>
            </div>
        </div>
    )
  }
}

//List组件的index.jsx文件：
import React, { Component } from 'react'
import PropTypes from "prop-types"
import Item from '../item/index'
import "./index.css"

export default class List extends Component {
    static propTypes={
        getChecked:PropTypes.func.isRequired,
        getDeleteId:PropTypes.func.isRequired,
        todos:PropTypes.array.isRequired,
        
    }
  render() {
    const {todos}=this.props;
    return (
            <ul className="todo-main">
                {todos.map((values,index)=>{
                    return <Item key={values.id} values={values} getChecked={this.props.getChecked} getDeleteId={this.props.getDeleteId}/>;
                })}
            </ul>
    )
  }
}

//Item组件的index.jsx文件：
import React, { Component } from 'react'
import './index.css'

export default class Item extends Component {

	state = {mouse:false} //标识鼠标移入、移出

	//鼠标移入、移出的回调
	handleMouse = (flag)=>{
		return ()=>{
			this.setState({mouse:flag})
		}
	}

	//勾选、取消勾选某一个todo的回调
	handleCheck = (id)=>{
		return (event)=>{
			this.props.updateTodo(id,event.target.checked)
		}
	}

	//删除一个todo的回调
	handleDelete = (id)=>{
		if(window.confirm('确定删除吗？')){
			this.props.deleteTodo(id)
		}
	}


	render() {
		const {id,name,done} = this.props
		const {mouse} = this.state
		return (
			<li style={{backgroundColor:mouse ? '#ddd' : 'white'}} onMouseEnter={this.handleMouse(true)} onMouseLeave={this.handleMouse(false)}>
				<label>
					<input type="checkbox" checked={done} onChange={this.handleCheck(id)}/>
					<span>{name}</span>
				</label>
				<button onClick={()=> this.handleDelete(id) } className="btn btn-danger" style={{display:mouse?'block':'none'}}>删除</button>
			</li>
		)
	}
}


```

```react

//Header组件的index.jsx文件：
import React, { Component } from 'react'
import PropTypes from "prop-types"
import "./index.css"
import { TimePicker } from 'antd';

export default class Header extends Component {

    static propTypes={
        getData:PropTypes.func.isRequired,
        
    }

    state={time:[]};

    getTime = (time,timeString) => {

        let timearr=[time.$H,time.$m];
        this.setState({time:timearr})

      };
    handleKeyUpThings=(event)=>{

        if(event.keyCode!==13){
            return;
        }
        else{
            const thingsStr=event.target.value.trim();
            if(thingsStr===""){
                alert("输入不能为空！！！");
                return;
            }
            else if(this.state.time.length===0){
                alert("时间不能为空！！！");
                return;

            }
            else{
            this.props.getData(thingsStr,this.state.time);
            event.target.value="";
            }
        }
    }

    handleAdding=()=>{
        const thingsStr=this.ipt.value.trim();
        if(thingsStr===""){
            alert("输入不能为空！！！");
            return;
        }
        else{
        this.props.getData(thingsStr,this.state.time);
        this.ipt.value="";
        }
    }


  render() {
    const format = 'HH:mm';
    return (
        <div className="todo-header">
            <input ref={(c)=>{this.ipt=c;}} onKeyUp={this.handleKeyUpThings} type="text" placeholder="请输入你的任务名称，按回车键确认"/>
            <div className='timeSelect'><TimePicker onChange={this.getTime} placeholder="开始时间" format={format}  ref={n=>this.timeSelect=n}/></div>
            <button onClick={this.handleAdding}>添加事件</button>
        </div>
    )
  }
}


//Footer组件的jsx文件
import React, { Component } from 'react'
import "./index.css"

export default class Footer extends Component {

  handleAllFinished=(event)=>{
    this.props.getIsAllFin(event.target.checked)
  }
  deleteAllPass=()=>{
    this.props.deleteAllPassed();
  }
  render() {

    let finishedNum=this.props.todos.reduce((preAll,cur)=>{
      if(cur.done===true){
        return preAll+1;
      }
      else{
        return preAll;
      }
    },0)

    let total=this.props.todos.length;
    return (
        <div className="todo-footer">
            <label>
            <input type="checkbox" onChange={this.handleAllFinished} checked={finishedNum===total&&total!==0 ? true:false} />
            </label>
            <span>
            已完成{finishedNum}&nbsp;/&nbsp;全部{total}
            </span>
            <button className="btn btn-danger" onClick={this.deleteAllPass}>清除已完成任务</button>
        </div>
    )
  }
}

```





相关CSS文件：

```css
//APP.css
/*base*/
/* @media screen and (max-width:){
  
} */


@media (max-width:748px) {
  .todo-header input{
    width: 70%!important;
  }
  .todo-header button{
    height: 42px!important;
  }
@media (max-width:540px) {
  .todo-header input::placeholder {
    font-size: 12px;
  }
  .todo-header input {
    width: 60%!important;
    padding: 2px!important;
  }
  .todo-header button {
    height: 36px!important;
  }
  .todo-header button{
    width: 20%!important;
    font-size: 12px!important;
  }
  .todo-footer label {
    font-size: 14px!important;
    margin-right: 5px!important;
  }
  .todo-footer span {
    font-size: 14px!important;
  }
  .todo-footer button{
    width: 115px!important;
    font-size: 14px!important;
    padding: 2px!important;
    margin-top: 7px!important;
  }
}
}
body {
    background: #fff;
  }
  
  .btn {
    display: inline-block;
    padding: 4px 12px;
    margin-bottom: 0;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
    border-radius: 4px;
  }
  
  .btn-danger {
    color: #fff;
    background-color: #da4f49;
    border: 1px solid #bd362f;
  }
  
  .btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
  }
  
  .btn:focus {
    outline: none;
  }
  
  .todo-container {
    width: 80%;
    margin: 0 auto;
    position: relative;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  


//List组建的
 /*main*/
 .todo-main {
    margin-left: 0px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding: 0px;
  }
  
  .todo-empty {
    height: 40px;
    line-height: 40px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding-left: 5px;
    margin-top: 10px;
  }


//Item组件的


  /*item*/
  li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
  }
  
  li label {
    float: left;
    cursor: pointer;
  }
  
  li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
  }
  
  li button {
    float: right;
    display: none;
    margin-top: 3px;
  }
  
  li:before {
    content: initial;
  }
  
  li:last-child {
    border-bottom: none;
  }
  .infoBtn{
    float: right;
    margin-top: 3px;
    margin-right: 6px;
  }
  #imp{
    color: #1677ff !important;
    font-size: 17px!important;
  }


//Header组件的：
  /*header*/
  .todo-header input {
    float: left;
    width: 60%;
    height: 28px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 4px 7px;
    margin-right: 5%;
  }
  
  
  .todo-header input:focus {
    outline: none;
    border-color: rgba(82, 168, 236, 0.8);
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
  }
  .todo-header button{
    position: absolute;
    right: 15px;
    top: 10px;
    width: 100px;
    height: 39px;
    border-radius: 6px;
    border: 1px solid #ccc;
    background-color: orange;
    color: aliceblue;
    box-sizing: border-box;
    font-size: 14px;
  }
  .timeSelect{
    float: left;
    box-sizing: border-box;
    height: 38px!important;
  }
  .timeSelect .ant-picker{
    height: 100%;
  }

  .todo-header{
    overflow: auto;
  }
  .ant-picker-ok{
    margin: 0px!important;
    margin-top: 4px!important;
  }


//Footer组建的：
 
  /*footer*/
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }
  
  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }
  
  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }
  
  .todo-footer button {
    float: right;
    margin-top: 5px;
  }
  
 
```





**本程序需要注意的相关知识点：**

```javascript
	/*
	1.拆分组件、实现静态组件，注意：className、style的写法
		2.动态初始化列表，如何确定将数据放在哪个组件的state中？
					——某个组件使用：放在其自身的state中
					——某些组件使用：放在他们共同的父组件state中（官方称此操作为：状态提升）
		3.关于父子之间通信：
				1.【父组件】给【子组件】传递数据：通过props传递
				2.【子组件】给【父组件】传递数据：通过props传递，要求父提前给子传递一个函数
		4.注意defaultChecked 和 checked的区别，类似的还有：defaultValue 和 value
		5.状态在哪里，操作状态的方法就在哪里
		
		*/
```







## 4---React发送Axios请求与代理问题

老师的笔记和相关点：

#### react脚手架配置代理总结



**方法一**

> 在package.json中追加如下配置

```json
"proxy":"http://localhost:5000"
```

说明：

1. 优点：配置简单，前端请求资源时可以不加任何前缀。
2. 缺点：不能配置多个代理。
3. 工作方式：上述方式配置代理，当请求了3000不存在的资源时，那么该请求会转发给5000 （优先匹配前端资源）



**方法二**

1. 第一步：创建代理配置文件

   ```
   在src下创建配置文件：src/setupProxy.js
   ```

2. 编写setupProxy.js配置具体代理规则：

   ```js
   const proxy = require('http-proxy-middleware')
   
   module.exports = function(app) {
     app.use(
       proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
         target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
         changeOrigin: true, //控制服务器接收到的请求头中host字段的值
         /*
          changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
          changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
          changeOrigin默认值为false，但我们一般将changeOrigin值设为true
         */
         pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
       }),
       proxy('/api2', { 
         target: 'http://localhost:5001',
         changeOrigin: true,
         pathRewrite: {'^/api2': ''}
       })
     )
   }
   ```

说明：

1. 优点：可以配置多个代理，可以灵活的控制请求是否走代理。
2. 缺点：配置繁琐，前端请求资源时必须加前缀。



#### 发送axios请求：



**发起一个get请求（只能事情query的方式）**

```javascript
axios.get('/getUser?id=12345')
  .then(function (response) {
    // handle success
    console.log(response);
 
    // update state or do something
    this.setState({
      // ...
    })
  })
  .catch(function (error) {
    // handle error
    console.log(error);
  })
  .then(function () {
    // always executed
  })
  
  
  
axios.get('/getUser', {
    params: { // 设置参数（URL携带的参数）
      id: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  })
  .then(function () {
    // always executed
  });  


//简化版本：
async function getUser() {
  try {
    const response = await axios.get('/getUser?id=12345');
    console.log(response);
  } catch (error) {
    console.error(error);
  }

    
    
    
    
//最原始的一种：

axios({
  method: 'get',
  url: '/getUser',
  params: {
    id: 12345,
  }
})
  .then(function (response) {
    console.log(response);
  });
    
    
    
```



**发起一个post请求（有query和body形式    注意：body形式有urlencoded，json，formdata的形式）**



```javascript
//通过post发起json参数的请求：


import axios from 'axios'
let data = {"code":"1234","name":"yyyy"};
axios.post(`${this.$url}/test/testRequest`,data)
.then(res=>{
    console.log('res=>',res);            
})



//通过post发起formdata（文件）参数的请求：
import axios from 'axios'
let data = new FormData();
data.append('code','1234');
data.append('name','yyyy');
axios.post(`${this.$url}/test/testRequest`,data)
.then(res=>{
    console.log('res=>',res);            
})

//通过post发起urlencoded）参数的请求：
import axios from 'axios'
import qs from 'Qs'
let data = {"code":"1234","name":"yyyy"};
axios.post(`${this.$url}/test/testRequest`,qs.stringify({
    data
}))
.then(res=>{
    console.log('res=>',res);            
})


//既在query里，又在body里面
 return request({
    method: 'POST',
    url: '/mp/v1_0/articles',
    data,
    params: {
      draft
    }
  })


//或者
axios.post(`${this.$url}/test/testRequest`,data，{params：{id:1,age:30}});
//data是请求体，params是query里面（顺序不能变）

```



**服务器方面解析**

```javascript
//解析query里面的，直接req.query就行
//解析body里面的，需要引入body-parser包


app.use(bodyParser.json());//解析json
app.use(bodyParser.urlencoded({extended:false}))//解析urlencoded

//解析formata的数据，还需要引入
const multipartyMiddleware = multipart()//注册解析formData数据的中间件（body-parser已不支持解析formdata了）
app.get('/pushuser',multipartyMiddleware,(request,response)=>{})
```







## 5---小案例2（github搜索案例）

**老师的笔记和相关点：**

```javascript
   /*
1.设计状态时要考虑全面，例如带有网络请求的组件，要考虑请求失败怎么办。

​    2.ES6小知识点：解构赋值+重命名

​          let obj = {a:{b:1}}

​          const {a} = obj; //传统解构赋值

​          const {a:{b}} = obj; //连续解构赋值

​          const {a:{b:value}} = obj; //连续解构赋值+重命名

​    3.消息订阅与发布机制

​          1.先订阅，再发布（理解：有一种隔空对话的感觉）

​          2.适用于任意组件间通信

​          3.要在组件的componentWillUnmount中取消订阅

​    4.fetch发送请求（关注分离的设计思想）

​          try {

​            const response= await fetch(`/api1/search/users2?q=${keyWord}`)

​            const data = await response.json()

​            console.log(data);

​          } catch (error) {

​            console.log('请求出错',error);

​          }
*/
```

​      

**消息订阅与发布的知识点：**

```jsx
//必须要先订阅再发布，一般是在组件挂载的钩子上开启订阅服务

componentDidMount(){
		this.token = PubSub.subscribe('atguigu',(_,stateObj)=>{
			this.setState(stateObj)
		})
	}
PubSub.publish('atguigu',{isLoading:false,users:response.data.items})
```



## 6---React发送Fetch请求







## 7---前端路由相关原理：

## 8---react路由基本使用：

```jsx
//1，Link的使用
							{/* 原生html中，靠<a>跳转不同的页面 */}
							{/* <a className="list-group-item" href="./about.html">About</a>
							<a className="list-group-item active" href="./home.html">Home</a> */}

							{/* 在React中靠路由链接实现切换组件--编写路由链接 */}
							<Link className="list-group-item" to="/about">About</Link>
							<Link className="list-group-item" to="/home">Home</Link>

//2.route注册路由的使用（大的外面必须加broserRouter或hashRouter）
{/* 注册路由 */}
								<Route path="/about" component={About}/>
								<Route path="/home" component={Home}/>

//3.broserRouter或hashRouter
                                    <BrowserRouter>
                                        <App/>
                                    </BrowserRouter>,
                                        
//4.NavlINK的使用：
<NavLink activeClassName="atguigu" className="list-group-item" to="/about">About</NavLink>
<NavLink activeClassName="atguigu" className="list-group-item" to="/home">Home</NavLink>

//5.封装navlink

//6.Switch的使用
								<Switch>
									<Route path="/about" component={About}/>
									<Route path="/home" component={Home}/>
									<Route path="/home" component={Test}/>
								</Switch>
//避免出现多个home，界面都展示的问题

//7.样式丢失问题：
//8.模糊匹配与精准匹配问题：
								<Switch>
									<Route exact path="/about" component={About}/>
									<Route exact path="/home" component={Home}/>
								</Switch>
//加入exact为开启精准匹配，但一般来说，没有特殊要求，能不不开别开，会影响下一级路由


//9.redirect的使用：
								<Switch>
									<Route path="/about" component={About}/>
									<Route path="/home" component={Home}/>
									<Redirect to="/about"/>
								</Switch>


//10.redirect的使用：
```



## 9---react路由其它问题：

```jsx
//1.嵌套路由（注意不要开启精准匹配，并且写路径的时候，前面的部分也要加上
						<Switch>
							<Route path="/home/news" component={News}/>
							<Route path="/home/message" component={Message}/>
							<Redirect to="/home/news"/>
						</Switch>


//2.路由组件传参（params形式）



//传递方面
							return (
								<li key={msgObj.id}>

									{/* 向路由组件传递params参数 */}
				<Link to={`/home/message/detail/${msgObj.id}/${msgObj.title}`}>{msgObj.title}</Link>

									{/* 向路由组件传递search参数 */}
				<Link to={`/home/message/detail/?id=${msgObj.id}&title=${msgObj.title}`}>{msgObj.title}</Link>

									{/* 向路由组件传递state参数 */}
	<Link to={{pathname:'/home/message/detail',state:{id:msgObj.id,title:msgObj.title}}}>{msgObj.title}</Link>

								</li>
							)
            

//声明接收方面

				{/* 声明接收params参数 */}
				<Route path="/home/message/detail/:id/:title" component={Detail}/>

				{/* search参数无需声明接收，正常注册路由即可 */}
				<Route path="/home/message/detail" component={Detail}/>

				{/* state参数无需声明接收，正常注册路由即可 */}
				<Route path="/home/message/detail" component={Detail}/>


//使用方面

		console.log(this.props);

		// 接收params参数
		const {id,title} = this.props.match.params 

		// 接收search参数
		const {search} = this.props.location
		const {id,title} = qs.parse(search.slice(1))

		// 接收state参数
		const {id,title} = this.props.location.state || {}
        
        
        
        
        
        
//3.push的形式与replace的形式
        					<MyNavLink replace to="/about">About</MyNavLink>
							<MyNavLink replace to="/home">Home</MyNavLink>



//3.编程式路由


	replaceShow = (id,title)=>{
		//replace跳转+携带params参数
		this.props.history.replace(`/home/message/detail/${id}/${title}`)

		//replace跳转+携带search参数
		this.props.history.replace(`/home/message/detail?id=${id}&title=${title}`)

		//replace跳转+携带state参数
		this.props.history.replace(`/home/message/detail`,{id,title})
	}

	pushShow = (id,title)=>{
		//push跳转+携带params参数
		this.props.history.push(`/home/message/detail/${id}/${title}`)

		//push跳转+携带search参数
		this.props.history.push(`/home/message/detail?id=${id}&title=${title}`)

		//push跳转+携带state参数
		this.props.history.push(`/home/message/detail`,{id,title})
		
	}

	back = ()=>{
		this.props.history.goBack()
	}

	forward = ()=>{
		this.props.history.goForward()
	}

	go = ()=>{
		this.props.history.go(-2)
	}
    
    
    
    //接收的时候和之前一样
    				{/* 声明接收params参数 */}
				<Route path="/home/message/detail/:id/:title" component={Detail}/>

				{/* search参数无需声明接收，正常注册路由即可 */}
				<Route path="/home/message/detail" component={Detail}/>

				{/* state参数无需声明接收，正常注册路由即可 */}
				<Route path="/home/message/detail" component={Detail}/>

				<button onClick={this.back}>回退</button>&nbsp;
				<button onClick={this.forward}>前进</button>&nbsp;
				<button onClick={this.go}>go</button>






```





















QOJCIIRTHNJVJLZP







(1673772032740_942)