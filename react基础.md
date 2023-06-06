## react基础(面向组件编程)

### 1.1：创建虚拟DOM两种形式

1.使用JS创建虚拟DOM

```javascript
<script type="text/javascript" > 
		
		const VDOM=React.createElement("h1",{id:"test"},"Hello_React");//使用js创建虚拟DOM

		//2.渲染虚拟DOM到页面
		ReactDOM.render(VDOM,document.getElementById('test'))
	</script>
```

2.使用JSX创建虚拟DOM

```react
<script type="text/babel" > /* 此处一定要写babel */
		
		const VDOM=(
			<h1>
				<span>
					Hello_React
				</span>
			</h1>
		)//使用jsx来创建虚拟DOM
		//2.渲染虚拟DOM到页面
		ReactDOM.render(VDOM,document.getElementById('test'))
	</script>
```



### 2.1 虚拟DOM与真实DOM:

```javascript
const VDOM = ( 
			<h1 id="title">
				<span>Hello,React</span>
			</h1>
		)
		const TDOM = document.getElementById('demo')
		console.log('虚拟DOM',VDOM);
		console.log("真实DOM",TDOM);
		/* 
				关于虚拟DOM：
					1.本质是Object类型的对象（一般对象）
					2.虚拟DOM比较“轻”，真实DOM比较“重”，因为虚拟DOM是React内部在用，无需真实DOM上那么多的属性。
					3.虚拟DOM最终会被React转化为真实DOM，呈现在页面上。
		 */
```

### 3.1 JSX语法规范：



案例1展示：

```javascript
<style>
        .title{
            background-color: pink;
            width: 300px;
        }
    </style>
    
    

    <script type="text/babel">

        let idStr="atGUIguCSS";
        let dataStr="hello,react3";

        const VDOM=(
        <div>
            <h2 className="title" id={idStr.toLocaleLowerCase()}>
                <span style={{color:"white",fontSize:"36px"}}>
                    {dataStr.toLowerCase()}
                </span>
            </h2>
            <h2 className="title" id={idStr.toLocaleLowerCase()+"1"}>
                <span style={{color:"red",fontSize:"12px"}}>
                    {dataStr.toLowerCase()}
                </span>
            </h2>
            <input type="text"/>
        </div>
        );

        ReactDOM.render(VDOM,document.getElementById("test"));

    </script>
```





规则：

```javascript
/* 
				jsx语法规则：
						1.定义虚拟DOM时，不要写引号。
						2.标签中混入JS表达式时要用{}。
						3.样式的类名指定不要用class，要用className。
						4.内联样式，要用style={{key:value}}的形式去写。
						5.只有一个根标签
						6.标签必须闭合
						7.标签首字母
								(1).若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错。
								(2).若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错。

		 */
```





### 3.2 JSX小练习：

显示相关界面：

```javascript
<script type="text/babel" >
		/* 
			一定注意区分：【js语句(代码)】与【js表达式】
					1.表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方
								下面这些都是表达式：
										(1). a
										(2). a+b
										(3). demo(1)
										(4). arr.map() 
										(5). function test () {}
					2.语句(代码)：
								下面这些都是语句(代码)：
										(1).if(){}
										(2).for(){}
										(3).switch(){case:xxxx}
		
	 */
		//模拟一些数据
		const data = ['Angular','React','Vue']
		//1.创建虚拟DOM
		const VDOM = (
			<div>
				<h1>前端js框架列表</h1>
				<ul>
					{
						data.map((item,index)=>{
							return <li key={index}>{item}</li>
						})
					}
				</ul>
			</div>
		)
		//2.渲染虚拟DOM到页面
		ReactDOM.render(VDOM,document.getElementById('test'))
	</script>
```

### 4.1函数式组件

```javascript
<script type="text/babel">

        function MyComponent()
        {
            return <h2>这是一个简单的函数式组件</h2>
        }//创建一个函数式组件

        ReactDOM.render(<MyComponent/>,document.getElementById("test"));//渲染函数式组件

</script>

/* 
			执行了ReactDOM.render(<MyComponent/>.......之后，发生了什么？
					1.React解析组件标签，找到了MyComponent组件。
					2.发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中。
					
					注意：
					1.组件名首字母需要是大写
					2.渲染的时候，写的是组件闭合标签名，不是函数名
*/
```

### 4.2 类式组件：

```html
  <script>

      //1.创建类式组件
        class MyComponents extends React.Component{
            render(){
                return <h2>我是一个复杂组件</h2>
            }
        }
      
      //render是放在哪里的？—— MyComponent的原型对象上，供实例使用。
	 //render中的this是谁？—— MyComponent的实例对象 <=> MyComponent组件实例对象。

        ReactDOM.render(<MyComponents/>,document.getElementById('test'))
        //2.渲染组件到页面
        
       /* 
			执行了ReactDOM.render(<MyComponent/>.......之后，发生了什么？
					1.React解析组件标签，找到了MyComponent组件。
					2.发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。
					3.将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。
		*/
    </script>


```

### 5.三大属性：

```javascript
/*

	组件分为两大类：1.简单组件（函数组件）2.复杂组件（类组件）  区别在于是否有state
	
	只有复杂组件具备三大核心属性：state、
*/
```

#### 5.1.1状态初步

```javascript
 <script type="text/babel">

        class MyComponents extends React.Component{
            constructor(props){
                super(props);
                this.state={isHot:true};
            }
            render(){
                console.log(this);
                return <h2>今天天气很{this.state.isHot? "炎热":"寒冷"}</h2>
            }
        }

        ReactDOM.render(<MyComponents/>,document.getElementById('test'))
    </script>
```

#### 5.1.2状态2（React事件绑定）

```javascript
//原生JS的三种事件绑定都可以用，但推荐使用第三种，html中直接写onClick进行

 <script type="text/babel">

        class MyComponents extends React.Component{
            constructor(props){
                super(props);
                this.state={isHot:true};
            }
            render(){
                console.log(this);
                return <h2 onClick={demo}>今天天气很{this.state.isHot? "炎热":"寒冷"}</h2>
            }
        }

        ReactDOM.render(<MyComponents/>,document.getElementById('test'))

        function demo()
        {
            alert("标题被点击了！！！");
        }
    </script>
```



```react
	<script type="text/babel">
		//1.创建组件
		class Weather extends React.Component{
			
			//构造器调用几次？ ———— 1次
			constructor(props){
				console.log('constructor');
				super(props)
				//初始化状态
				this.state = {isHot:false,wind:'微风'}
				//解决changeWeather中this指向问题
				this.changeWeather = this.changeWeather.bind(this)
			}

			//render调用几次？ ———— 1+n次 1是初始化的那次 n是状态更新的次数
			render(){
				console.log('render');
				//读取状态
				const {isHot,wind} = this.state
				return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
			}

			//changeWeather调用几次？ ———— 点几次调几次
			changeWeather(){
				//changeWeather放在哪里？ ———— Weather的原型对象上，供实例使用
				//由于changeWeather是作为onClick的回调，所以不是通过实例调用的，是直接调用
				//类中的方法默认开启了局部的严格模式，所以changeWeather中的this为undefined
				
				console.log('changeWeather');
				//获取原来的isHot值
				const isHot = this.state.isHot
				//严重注意：状态必须通过setState进行更新,且更新是一种合并，不是替换。
				this.setState({isHot:!isHot})
				console.log(this);

				//严重注意：状态(state)不可直接更改，下面这行就是直接更改！！！
				//this.state.isHot = !isHot //这是错误的写法
			}
		}
		//2.渲染组件到页面
		ReactDOM.render(<Weather/>,document.getElementById('test'))
				
	</script>
```





简写形式：

```react
	<script type="text/babel">
		//1.创建组件
		class Weather extends React.Component{
			//初始化状态
			state = {isHot:false,wind:'微风'}

			render(){
				const {isHot,wind} = this.state
				return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
			}

			//自定义方法————要用赋值语句的形式+箭头函数
			changeWeather = ()=>{
				const isHot = this.state.isHot
				this.setState({isHot:!isHot})
			}
		}
		//2.渲染组件到页面
		ReactDOM.render(<Weather/>,document.getElementById('test'))
				
	</script>
```

#### 5.2.1 props基本使用：

```react
    <script type="text/babel">

        class Person extends React.Component{
            render()
            {
                const{name,sex,age}=this.props;
                return (
                    <div>
                        <br/>
                        <ul>
                            <li>姓名:{name}</li>
                            <li>性别:{sex}</li>
                            <li>年龄:{age}</li>
                        </ul>
                    </div>
                )
            }
        }

        ReactDOM.render(<Person name="tom" age="22" sex="男"/>,document.getElementById("test"));
        
        ReactDOM.render(<Person name="xiaoMing" age="19" sex="男"/>,document.getElementById("test2"));
    </script>
```

其中的批量传参

```react
 const pcs={name:"xiaoCC" ,age:33,sex:"男"};
        ReactDOM.render(<Person {...pcs}/>,document.getElementById("test3"));
```

对props进行相应的限制：

```react


         Person.propTypes={
            name:PropTypes.string.isRequired,
            age:PropTypes.number,
            sex:PropTypes.string,
            speak:PropTypes.func,
        }
        Person.defaultProps={
            sex:"不男不女",
            age:0,
        }
        
          ReactDOM.render(<Person  age="22" speak={speak}/>,document.getElementById("test"));
        
        
          function speak()
        {
            console.log("我会说话");
        }
        
        //props属性是只读的，不允许进行写
```







#### 5.2.2. props简写形式：

```react
<script type="text/babel">
		//创建组件
		class Person extends React.Component{

			constructor(props){
				//构造器是否接收props，是否传递给super，取决于：是否希望在构造器中通过this访问props
				// console.log(props);
				super(props)
				console.log('constructor',this.props);
			}

			//对标签属性进行类型、必要性的限制
			static propTypes = {
				name:PropTypes.string.isRequired, //限制name必传，且为字符串
				sex:PropTypes.string,//限制sex为字符串
				age:PropTypes.number,//限制age为数值
			}

			//指定默认标签属性值
			static defaultProps = {
				sex:'男',//sex默认值为男
				age:18 //age默认值为18
			}
			
			render(){
				// console.log(this);
				const {name,age,sex} = this.props
				//props是只读的
				//this.props.name = 'jack' //此行代码会报错，因为props是只读的
				return (
					<ul>
						<li>姓名：{name}</li>
						<li>性别：{sex}</li>
						<li>年龄：{age+1}</li>
					</ul>
				)
			}
		}

		//渲染组件到页面
		ReactDOM.render(<Person name="jerry"/>,document.getElementById('test1'))
	</script>
```

**构造器中的问题：**

```react
constructor(props){
				//构造器是否接收props，是否传递给super，取决于：是否希望在构造器中通过this访问props
				// console.log(props);
				super(props)
				console.log('constructor',this.props);
			}

```



#### 5.2.3 函数式组件中的props

```react
<script type="text/babel">
		//创建组件
		function Person (props){
			const {name,age,sex} = props
			return (
					<ul>
						<li>姓名：{name}</li>
						<li>性别：{sex}</li>
						<li>年龄：{age}</li>
					</ul>
				)
		}
		Person.propTypes = {
			name:PropTypes.string.isRequired, //限制name必传，且为字符串
			sex:PropTypes.string,//限制sex为字符串
			age:PropTypes.number,//限制age为数值
		}

		//指定默认标签属性值
		Person.defaultProps = {
			sex:'男',//sex默认值为男
			age:18 //age默认值为18
		}
		//渲染组件到页面
		ReactDOM.render(<Person name="jerry"/>,document.getElementById('test1'))
	</script>
```

#### 5.3.1 refs的基本使用（字符串形式的refs）

```react
 <script type="text/babel">

        class TextIpt extends React.Component{

            showData=()=>{
                if(this.refs.ipt1.value.trim()!=''){
                    alert(this.refs.ipt1.value)
                }
                else{
                    alert("检索为空！！！");
                }
            }
            showData2=()=>{
                if(this.refs.ipt2.value.trim()!=''){
                    alert(this.refs.ipt2.value)
                }
                else{
                    alert("检索为空！！！");
                }
            }
            render(){
                return(
                   <div>
                        <input ref="ipt1" type="text" placeholder="点击按钮"/>&nbsp;&nbsp;
                        <button ref="btn1" onClick={this.showData}>点击弹出提示信息</button>
                        &nbsp;&nbsp;
                        <input ref="ipt2" onBlur={this.showData2} type="text" placeholder="失去焦点"/>
                    </div>
                )
            }
        }

        ReactDOM.render(<TextIpt/>,document.getElementById("test"))
    </script>
```





#### 5.3.2 回调形式的refs

```react
 <input ref={(c)=>{this.ipt1=c}} type="text" placeholder="点击按钮"/>&nbsp;&nbsp;
 
 
    showData=()=>{
                if(this.ipt1.value.trim()!=''){
                    alert(this.ipt1.value)
                }
                else{
                    alert("检索为空！！！");
                }
            }
```

调用的次数问题与解决：

```react
 			saveInput=(c)=>{
                this.ipt1=c;
            }
            

            <input type="text" ref={this.saveInput}/><br/><br/>
            <button onClick={this.showData}>点击弹出提示信息</button>
            
            
             showData=()=>{
                alert(this.ipt1.value);
            }
            
```

#### 5.3.3 API形式创建Ref



```react
 
//原理是创建了一个Ref容器
			myRef1=React.createRef();
            
            
            
            <input type="text"ref={this.myRef1} onBlur={this.showData2} placeholder="失去焦点提示数据"/><br/><br/>
            
            
            
            showData2=()=>{
                alert(this.myRef1.current.value);
            }
```





5.3.4 事件处理：

```react

<script type="text/babel">
		//创建组件
		class Demo extends React.Component{
			/* 
				(1).通过onXxx属性指定事件处理函数(注意大小写)
						a.React使用的是自定义(合成)事件, 而不是使用的原生DOM事件 —————— 为了更好的兼容性
						b.React中的事件是通过事件委托方式处理的(委托给组件最外层的元素) ————————为了的高效
				(2).通过event.target得到发生事件的DOM元素对象 ——————————不要过度使用ref
			 */
			//创建ref容器
			myRef = React.createRef()
			myRef2 = React.createRef()

			//展示左侧输入框的数据
			showData = (event)=>{
				console.log(event.target);
				alert(this.myRef.current.value);
			}
            

			//展示右侧输入框的数据
			showData2 = (event)=>{
				alert(event.target.value);
			}

			render(){
				return(
					<div>
						<input ref={this.myRef} type="text" placeholder="点击按钮提示数据"/>&nbsp;
						<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
						<input onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>&nbsp;
					</div>
				)
			}
		}
		//渲染组件到页面
		ReactDOM.render(<Demo a="1" b="2"/>,document.getElementById('test'))
	</script>
```





### 6.1  收集表单数据（受控与非受控组件）

理论理解：

```

```





受控式组件（相对还是推荐用这个组件）

```react
<script type="text/babel">
		//创建组件
		class Login extends React.Component{

            state={username:"",password:""};//初始化state

			handleSubmit=(eve)=>{
                eve.preventDefault();//阻止表单提交
                const {username,password}=this.state;
                alert(`你提交的数据姓名为：${username}，密码为：${password}`);
            }//数据提交（表单类的DOM并不是现用现取，所以这是受控式组件

            saveUserName=(event)=>{
                this.setState({username:event.target.value});
            }//用户信息的维护
            savePassword=(event)=>{
                this.setState({password:event.target.value});
            }//密码信息的维护
            
			render(){
				return(
					<form  onSubmit={this.handleSubmit}>
						用户名：<input   onChange={this.saveUserName} type="text" name="username"/>
						密码：<input  onChange={this.savePassword} type="password" name="password"/>
						<button>登录</button>
					</form>
				)
			}
		}
		//渲染组件
		ReactDOM.render(<Login/>,document.getElementById('test'))
	</script>
```

### 7.  生命周期

#### 7.1生命周期引入：

```react
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>1_引出生命周期</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		//<生命周期回调函数 == 生命周期钩子函数 == 生命周期函数 == 生命周期钩子
		class Life extends React.Component{

			state = {opacity:1}

			death = ()=>{
				//卸载组件
				ReactDOM.unmountComponentAtNode(document.getElementById('test'))
			}

			//组件挂完毕
			componentDidMount(){
				console.log('componentDidMount');
				this.timer = setInterval(() => {
					//获取原状态
					let {opacity} = this.state
					//减小0.1
					opacity -= 0.1
					if(opacity <= 0) opacity = 1
					//设置新的透明度
					this.setState({opacity})
				}, 200);
			}

			//组件将要卸载
			componentWillUnmount(){
				//清除定时器
				clearInterval(this.timer)
			}

			//初始化渲染、状态更新之后
			render(){
				console.log('render');
				return(
					<div>
						<h2 style={{opacity:this.state.opacity}}>React学不会怎么办？</h2>
						<button onClick={this.death}>不活了</button>
					</div>
				)
			}
		}
		//渲染组件
		ReactDOM.render(<Life/>,document.getElementById('test'))
	</script>
</body>
</html>
```

<img src="C:/Users/M/Desktop/SharedScreenshot.jpg" alt="SharedScreenshot" style="zoom: 67%;" />

#### 7.2 旧生命周期全部与总结

```react
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>2_react生命周期(旧)</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
	
		//创建组件
		class Count extends React.Component{

			//构造器
			constructor(props){
				console.log('Count---constructor');
				super(props)
				//初始化状态
				this.state = {count:0}
			}

			//加1按钮的回调
			add = ()=>{
				//获取原状态
				const {count} = this.state
				//更新状态
				this.setState({count:count+1})
			}

			//卸载组件按钮的回调
			death = ()=>{
				ReactDOM.unmountComponentAtNode(document.getElementById('test'))
			}

			//强制更新按钮的回调
			force = ()=>{
				this.forceUpdate()
			}

			//组件将要挂载的钩子
			componentWillMount(){
				console.log('Count---componentWillMount');
			}

			//组件挂载完毕的钩子
			componentDidMount(){
				console.log('Count---componentDidMount');
			}

			//组件将要卸载的钩子
			componentWillUnmount(){
				console.log('Count---componentWillUnmount');
			}

			//控制组件更新的“阀门”
			shouldComponentUpdate(){
				console.log('Count---shouldComponentUpdate');
				return true
			}

			//组件将要更新的钩子
			componentWillUpdate(){
				console.log('Count---componentWillUpdate');
			}

			//组件更新完毕的钩子
			componentDidUpdate(){
				console.log('Count---componentDidUpdate');
			}

			render(){
				console.log('Count---render');
				const {count} = this.state
				return(
					<div>
						<h2>当前求和为：{count}</h2>
						<button onClick={this.add}>点我+1</button>
						<button onClick={this.death}>卸载组件</button>
						<button onClick={this.force}>不更改任何状态中的数据，强制更新一下</button>
					</div>
				)
			}
		}
		
		//父组件A
		class A extends React.Component{
			//初始化状态
			state = {carName:'奔驰'}

			changeCar = ()=>{
				this.setState({carName:'奥拓'})
			}

			render(){
				return(
					<div>
						<div>我是A组件</div>
						<button onClick={this.changeCar}>换车</button>
						<B carName={this.state.carName}/>
					</div>
				)
			}
		}
		
		//子组件B
		class B extends React.Component{
			//组件将要接收新的props的钩子
			componentWillReceiveProps(props){
				console.log('B---componentWillReceiveProps',props);
			}

			//控制组件更新的“阀门”
			shouldComponentUpdate(){
				console.log('B---shouldComponentUpdate');
				return true
			}
			//组件将要更新的钩子
			componentWillUpdate(){
				console.log('B---componentWillUpdate');
			}

			//组件更新完毕的钩子
			componentDidUpdate(){
				console.log('B---componentDidUpdate');
			}

			render(){
				console.log('B---render');
				return(
					<div>我是B组件，接收到的车是:{this.props.carName}</div>
				)
			}
		}
		
		//渲染组件
		ReactDOM.render(<Count/>,document.getElementById('test'))
	</script>
</body>
</html>
```





**总结：**

```javascript
	/* 
				1. 初始化阶段: 由ReactDOM.render()触发---初次渲染
									1.	constructor()
									2.	componentWillMount()
									3.	render()
									4.	componentDidMount() =====> 常用
										一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息
				2. 更新阶段: 由组件内部this.setSate()或父组件render触发
									1.	shouldComponentUpdate()
									2.	componentWillUpdate()
									3.	render() =====> 必须使用的一个
									4.	componentDidUpdate()
				3. 卸载组件: 由ReactDOM.unmountComponentAtNode()触发
									1.	componentWillUnmount()  =====> 常用
											一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息
		*/
```

#### 7.3新版生命周期的全部与总结：

流程图：

<img src="C:/Users/M/Desktop/SharedScreenshot1.jpg" alt="SharedScreenshot1" style="zoom: 50%;" />



与旧版生命周期相比，React 生命周期**即将废弃**componentWillMount()、componentWillReceiveProps()、componentWillUpdate()三个钩子，现在使用会出现警告，下一个大版本需要加上**UNSAFE_**前缀才能使用，以后可能会被彻底废弃，不建议使用。

新版增加了两个钩子：**getDerivedStateFromProps()**和**getSnapshotBeforeUpdate()**。



1.**`getDerivedStateFromProps()`**

在调用`render()`方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新`state`，如果返回 `null` 则不更新任何内容

使用场景（罕见）：`state`的值在任何时候都取决于`props`



2.**`getSnapshotBeforeUpdate()`**

最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的**任何返回值**将**作为参数**传递给 `**componentDidUpdate()**`

**3.小案例（通过获取快照的方式获取上一步的滑动数据给更新状态来实现）：**

```javascript
class ScrollingList extends React.Component {
  constructor(props) {
    super(props);
    this.listRef = React.createRef();
  }
 
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // 我们是否在 list 中添加新的 items ？
    // 捕获滚动​​位置以便我们稍后调整滚动位置。
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }
 
  componentDidUpdate(prevProps, prevState, snapshot) {
    // 如果我们 snapshot 有值，说明我们刚刚添加了新的 items，
    // 调整滚动位置使得这些新 items 不会将旧的 items 推出视图。
    //（这里的 snapshot 是 getSnapshotBeforeUpdate 的返回值）
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }
 
  render() {
    return (
      <div ref={this.listRef}>{/* ...contents... */}</div>
    );
  }
}
```



### 8.differ算法：

#### 8.1----differ验证

**只对变化的DOM节点进行变更渲染，不变的不会**

**最小力度是一个标签，但标签内部的标签同样会对比**



```jsx
	<script type="text/babel">
		class Time extends React.Component {
			state = {date: new Date()}

			componentDidMount () {
				setInterval(() => {
					this.setState({
						date: new Date()
					})
				}, 1000)
			}

			render () {
				return (
					<div>
						<h1>hello</h1>
						<input type="text"/>
						<span>
							现在是：{this.state.date.toTimeString()}
							<input type="text"/>
						</span>
					</div>
				)
			}
		}

		ReactDOM.render(<Time/>,document.getElementById('test'))
</script>
```



#### 8.2---differ深入：



```jsx
<script type="text/babel">

	class Person extends React.Component{

		state = {
			persons:[
				{id:1,name:'小张',age:18},
				{id:2,name:'小李',age:19},
			]
		}

		add = ()=>{
			const {persons} = this.state
			const p = {id:persons.length+1,name:'小王',age:20}
			this.setState({persons:[p,...persons]})
		}

		render(){
			return (
				<div>
					<h2>展示人员信息</h2>
					<button onClick={this.add}>添加一个小王</button>
					<h3>使用index（索引值）作为key</h3>
					<ul>
						{
							this.state.persons.map((personObj,index)=>{
								return <li key={index}>{personObj.name}---{personObj.age}<input type="text"/></li>
							})
						}
					</ul>
					<hr/>
					<hr/>
					<h3>使用id（数据的唯一标识）作为key</h3>
					<ul>
						{
							this.state.persons.map((personObj)=>{
								return <li key={personObj.id}>{personObj.name}---{personObj.age}<input type="text"/></li>
							})
						}
					</ul>
				</div>
			)
		}
	}

	ReactDOM.render(<Person/>,document.getElementById('test'))
</script>
```

**相关总结：**

```javascript
	/*
   经典面试题:
      1). react/vue中的key有什么作用？（key的内部原理是什么？）
      2). 为什么遍历列表时，key最好不要用index?
      
			1. 虚拟DOM中key的作用：
					1). 简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用。

					2). 详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, 
												随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：

									a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：
												(1).若虚拟DOM中内容没变, 直接使用之前的真实DOM
												(2).若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM

									b. 旧虚拟DOM中未找到与新虚拟DOM相同的key
												根据数据创建新的真实DOM，随后渲染到到页面
									
			2. 用index作为key可能会引发的问题：
								1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:
												会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。

								2. 如果结构中还包含输入类的DOM：
												会产生错误DOM更新 ==> 界面有问题。
												
								3. 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，
									仅用于渲染列表用于展示，使用index作为key是没有问题的。
					
			3. 开发中如何选择key?:
								1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
								2.如果确定只是简单的展示数据，用index也是可以的。
   */
	
	/* 
		慢动作回放----使用index索引值作为key

			初始数据：
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			初始的虚拟DOM：
					<li key=0>小张---18<input type="text"/></li>
					<li key=1>小李---19<input type="text"/></li>

			更新后的数据：
					{id:3,name:'小王',age:20},
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			更新数据后的虚拟DOM：
					<li key=0>小王---20<input type="text"/></li>
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>

	-----------------------------------------------------------------

	慢动作回放----使用id唯一标识作为key

			初始数据：
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			初始的虚拟DOM：
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>

			更新后的数据：
					{id:3,name:'小王',age:20},
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			更新数据后的虚拟DOM：
					<li key=3>小王---20<input type="text"/></li>
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>


	 */
```

