

![](https://cdn.filestackcontent.com/uNjPSWPjTLCqnu21yYEP)



# Redux中使用多层Reducer处理器

## 写在前面

当我们的系统变得越来越复杂的时候，使用一个Reducer函数来管理全局数据，会让一个Reducer函数变得臃肿，不利于后期的开发和管理。

比如一个图书馆应用系统，我们需要管理图书数据，图书馆管理员数据，读者数据，借阅记录数据采购数据等等。如果把这些数据放在一个Reducer中，那么会造成数据管理的混乱，不利于后期的维护和开发。

一个比较好的解决方案是：建立多个Reducer，分别用于管理图书，管理员，读者等数据。当我们需要更新图书数据的时候，只需要查看管理图书的Reducer代码即可，无需关心其他Reducer。这样既省时省力，有可以减少开发中的bug。

下面我们就来介绍一个如果建立多个Reducer，即如何拆分大的Reducer。

## 假设业务场景

比如我们现在要开发一个图书馆应用，需要对图书馆中的图书和管理员进行管理。

图书数据管理要求如下：查看图书列表，向图书列表中增加新的图书，从图书列表中删除图书。

管理员数据管理要求如下：设置管理员的姓名，角色等信息

有了这个简单的业务场景，我们开始设置对应的Redux设置吧。

## 开始进行解决：

### 创建第一个reducer，管理图书馆管理员的数据

首先我们需要创建几个action tye常量，新建UserType.js文件，代码如下：

```jsx
export const SET_FIRST_NAME = "SET_FIRST_NAME";
export const SET_LAST_NAME = "SET_LAST_NAME";
export const ADD_NEW_USER_ROLE = "ADD_NEW_USER_ROLE";
export const REMOVE_USER_ROLE = "REMOVE_USER_ROLE";
复制代码
```

创建按完成之后，使用这些常量定义action，用来调用reducer中的逻辑。新建UserAction.js文件，代码如下：

```jsx
import { SET_FIRST_NAME, SET_LAST_NAME, ADD_NEW_USER_ROLE, REMOVE_USER_ROLE } from "./UserType"

export const setFirstName = firstName => {
  return {
    type: SET_FIRST_NAME,
    payload: firstName
  }
}

export const setLastName = lastName => {
  return {
    type: SET_LAST_NAME,
    payload: lastName
  }
}

export const addNewUserRole = newUserRole => {
  return {
    type: ADD_NEW_USER_ROLE,
    payload: newUserRole
  }
}

export const removeUserRole = roleName => {
  return {
    type: REMOVE_USER_ROLE,
    payload: roleName
  }
}
复制代码
```

最后根据定义reducer函数，用来相应action中的动作，新建UserReducer.js文件，代码如下：

```jsx
// 定义原始user数据，reducer的作用就是修改这些数据，然后返回这些数据
const initialUserState = {
    firstName: "",
    lastName: "",
    userRoleList: []
}

// 引入type常量，reducer中收到action发送过来的数据，在reducer中判断type类型，进行处理
import { SET_FIRST_NAME, SET_LAST_NAME, ADD_NEW_USER_ROLE, REMOVE_USER_ROLE } from "./UserType"

const UserReducer = (state = initialUserState, action) => {
    switch(action.type) {
        case SET_FIRST_NAME:
            return {
                ...state,
                firstName: action.payload
            }
        case SET_LAST_NAME:
            return {
                ...state,
                lastName: action.payload
            }
        case ADD_NEW_USER_ROLE:
            let newUserRoleList = [...state.userRoleList]
            if (!newUserRoleList.includes(action.payload)) {
                newUserRoleList.push(action.payload)
            }
            return {
                ...state,
                userRoleList: newUserRoleList
            }
        case REMOVE_USER_ROLE:
            return {
                ...state,
                userRoleList: state.userRoleList.filter(roleName => roleName !== action.payload)
            }
        default:
            return state;
    }
}

export default UserReducer;
复制代码
```

### 创建第二个reducer，管理图书的数据

同理，首先新建BookType.js文件，定义type常量。

```jsx
export const ADD_NEW_BOOK = "ADD_NEW_BOOK";
export const REMOVE_NEW_BOOK = "REMOVE_NEW_BOOK";
复制代码
```

然后使用这些常量定义action，新建BookAction.js文件，代码如下：

```jsx
import { ADD_NEW_BOOK, REMOVE_NEW_BOOK } from "./BookType";

export const addNewBooks = newBookName => {
  return {
    type: ADD_NEW_BOOK,
    payload: newBookName
  }
}

export const removeNewBooks = bookName => {
  return {
    type: REMOVE_NEW_BOOK,
    payload: newBookName
  }
}
复制代码
```

最后定义reducer，管理图书信息，新建BookReducer.js文件，代码如下：

```jsx
const initialBookState = {
  bookList: ["明朝那些事", "水浒传"]
}

import { ADD_NEW_BOOK, REMOVE_NEW_BOOK } from "./BookType";

const BookReducer = (state = initialBookState, action) => {
  if (action.type === ADD_NEW_BOOK) {
    return {
      ...initialBookState,
      bookList: [...initialBookState.bookList, action.payload]
    }
  }

  if (action.type === REMOVE_NEW_BOOK) {
    return {
      ...initialBookState,
      bookList: initialBookState.bookList.filter(_book => _book !== action.payload)
    }
  }

  return state;
}

export default BookReducer;
复制代码
```

### 合并两个reducer，创建reducer store实例

现在我们已经创建了两个reducer，下面需要做的就是把这两个reducer合并起来，创建一个redux store实例。这里需要用到 redux 包提供的combineReducers方法，新建Store.js文件，具体代码如下：

```jsx
// 引入需要的redux方法
import { createStore, combineReducers } from 'redux'

// 引入定义好的reducer
import BookReducer from './BookReducer'
import UserReducer from './UserReducer';

// 使用该方法合并两个reducer
const rootReducer = combineReducers({
  BookReducer,
  UserReducer
});

// 使用合并后的reducer创建store实例
const Store = createStore(rootReducer)

export default Store;
复制代码
```

### 使用多个reducer创建的store实例，数据结构是怎样的

现在我们已经创建了reduxe实例，可以在一个组件中获取redux中的数据，看下此时的数据结构是怎样的，组件代码如下：

```jsx
import React from 'react'
import Store from 'myRedux/Store'

class Login extends React.Component {

  constructor(props) {
    super(props);
    this.state = {
    }
  }

  componentDidMount() {
    this.setState((preState, props) => ({
      ...preState,
      ...Store.getState()
    }));

    Store.subscribe(() => {
			// 打印出redux中的数据
      console.log("Redux data: ", Store.getState());
      this.setState((preState, props) => ({
        ...preState,
        ...Store.getState()
      }));
    });
  }

  render() {
    return (
      <div>
        测试页面
      </div>
    );
  }

}

export default Login;
复制代码
```

在控制台中，我们可以看到redux中的数据结构如下：

```jsx
{
    "UserReducer": {
        "firstName": "",
        "lastName": "",
        "userRoleList": []
    },
    "BookReducer": {
        "bookList": [
            "明朝那些事",
            "水浒传"
        ]
    }
}
复制代码
```

了解redux的数据结构，在后期获取这些数据的时候，我们就不会出错了。

### 如何修改其中一个reducer管理的数据

现在数据有了，我们还要能够修改多个reducer的数据，其实非常简单。虽然我们定义了两个reducer，但是完全看作是一个reducer，在修改数据的时候遵循这样的流程：引入action，然后触发action。比如下面的代码实例：

```jsx
import React from 'react';
import Store from "myRedux/Store";

// 引入action
import { setFirstName, setLastName } from "myRedux/UserAction";

const LeftSideBar = () => {

  const modifyUserName = () => {
    // 触发action
    Store.dispatch(setFirstName("Allen"));
    Store.dispatch(setLastName("Feng"));
  }
  
  return (
    <div>
      <button className="icon" onClick={modifyUserName} ></button>
    </div>
  );
}

export default LeftSideBar;
复制代码
```

## 写在最后

如何创建多个reducer来管理redux数据，我的心得就是上述这些。在一些很大的项目中，创建多个reduxer可以帮助我们清晰明了的管理redux中的数据



作者：程序铺子
链接：https://juejin.cn/post/6957696212761837605
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。