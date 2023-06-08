# React Hooks学习笔记

## 1.useState相关：

### 一系列的基本使用：

```javascript
import React ,{useState}from 'react'

export default function Demo11() {
    const [count,setCount]=useState(0);//启用状态与状态改变函数
    const [wid,setWid]=useState(1920);
  return (
    <div>
        <p>当前的数为：{count}</p>
        {/* 直接使用count和setCount即可 */}
        <button onClick={()=>{setCount(count+1)}}>点我加一</button>
    </div>
  )
}

```

### 相关问题的解决：

**1.调用位置和数组结构：**

1、仅顶层调用：不能在循环，条件，嵌套函数等中调用useState().在多个useState()调用中，渲染之间的调用顺序必须相同。
 2、仅从React 函数调用 :必须仅在函数组件或自定义钩子内部调用useState()。

3.记住一定以数组的形式启动useState();

**2.惰性初始化：**

**3.复杂状态管理可启动useReducer进行**



## 2.useEffect：

### 简介：

`useEffect()`的作用就是指定一个副效应函数，组件每渲染一次，该函数就自动执行一次。组件首次在网页 DOM 加载后，副效应函数也会执行。

当不希望`useEffect()`每次渲染都执行，可以使用它的第二个参数，使用一个数组指定副效应函数的**依赖项**，只有依赖项发生变化，**才会重新渲染**。

### 几种情况解析：

1. **仅需要在程序开始执行一次的方法（其不需要关闭），直接在依赖项使用空数组并不需要return任何**
2. **需要每次渲染都执行，什么都不用管**
3. **例如需要name状态改变时渲染，依赖项写上name就行；**
4. **仅需要在程序开始执行，声明周期结束关闭的方法，直接在依赖项使用空数组，关闭动作写在return内**



### **注意点：**

使用`useEffect()`时，有一点需要注意。如果有多个副效应，应该调用多个`useEffect()`，而不应该合并写在一起，

功能逻辑相同的才写在一起；





## 3.useContext共享状态钩子：



如果需要在组件之间共享状态，可以使用`useContext()`。

```jsx
//目的是实现下面几个组件之间的状态共享
<div className="father">
  <Son1/>
  <Son2/>
</div>


//实现共享数据，第一步是定义启动useContext()

const ctxs = React.createContext({});

//随后使用如下的方式进行

<ctxs.Provider value={{
  name: 'xiaoming'
}}>
  <div className="father">
    <Son1/>
    <Son2/>
  </div>
</ctxs.Provider>



//子组件使用状态

const Son1 = () => {
  const { name } = useContext(ctxs);//将对象进行结构出来
  return (
      <p>你好，我是：{username}</p>
  );
}
```





## 4.useReducer 处理复杂状态的钩子：



基本的使用思路与redux相近

它接受自**定义操作函数**和**状态的初始值**作为参数，返回一个数组。

数组的**第一个成员是状态的当前值**，（一般就是状态变量或者使用state作为多个状态的集合对象写进来）

**第二个成员是发送 action 的`dispatch`函数。**

**小例子：**

```jsx
function App() {
  const [state, dispatch] = useReducer(myReducer, { count:   0 });//启用useReducer()(state即为多个状态的集合对象)（也可直接用count作为状态变量）
  return  (
    <div className="App">
      <button onClick={() => dispatch({ type: 'countUp' })}>
        点我加一
      </button>
      <p>Count: {state.count}</p>
    </div>
  );
}


const myReducer = (state, action) => {//第二步创建自定义操作函数（与redux中的reducer一样
  switch(action.type)  {
    case('countUp'):
      return  {
        ...state,//state作为集合对象，其他的状态不需要变
        count: state.count + 1//变动state中count的值
      }
    default:
      return  state;//初始化状态就是将初始值返回
  }
}


//第三步，对应的回调函数中调用dispatch即可


```





## 5.useReducer与useContext共同使用：

结合状态共享和复杂状态处理，可以在一定程度上替代redux的作用，

（项目中可以将两者领出来放到一个文件中，把外界需要的东西分别暴露出来**（如：dispatch 、创建的ctxs等）** 

但没法提供中间件（middleware）和时间旅行（time travel），





## 6.自定义Hooks：



