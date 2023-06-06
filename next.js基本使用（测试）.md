![](https://fleek-team-bucket.storage.fleek.co/thumbnails-blog/Next.png)

# Next.js基本使用

## next 简介

next.js作为一款轻量级的应用框架，主要用于构建静态网站和后端渲染网站。

## next 特点

- 默认情况下由服务器呈现
- 自动代码拆分可加快页面加载速度
- 简单的客户端路由（基于页面）
- 基于 Webpack 的开发环境,支持热模块替换（HMR）
- 能够与 Express 或任何其他 Node.js HTTP 服务器一起实现
- 可使用您自己的 Babel 和 Webpack 配置进行自定义

## 系统需求

Next.js 可与 Windows,Mac 和 Linux 一起使用.您只需要在系统上安装 Node.js 即可开始构建 Next.js 应用程序.如果有个编辑器就更好了

## 初始化项目

```csharp
mkdir next-demo //创建项目
cd next-demo //进入项目
npm init -y // 快速创建package.json而不用进行一些选择
npm install --save react react-dom next // 安装依赖
mkdir pages //创建pages
```

mkdir pages 这一步是必须创建一个叫 pages 的文件夹,因为 next 是根据 pages 下面的 js jsx tsx 文件来进行路由生成,且文件夹名字必须是pages

然后打开 package.json 目录中的 next-demo 文件并替换 scripts 为以下内容：

```json
"scripts": {
  "dev": "next",
  "build": "next build",
  "start": "next start"
}
```

运行以下命令以启动开发服务器：

```dockerfile
npm run dev
```

现在可以打开 localhost:3000 来查看页面效果,如果不喜欢 3000 或者端口冲突,执行下面命令

```css
npm run dev -p 6688(你喜欢的端口)
```

这时候就可以在 localhost:6688 上看到页面效果了

### hello world

此时我们在 pages 文件夹下创建一个 index.js 作为首页

```javascript
const Index = () => (
  <div>
    <p>Hello Next.js</p>
  </div>
);

export default Index;
```

再次查看 localhost:6688 就可以看到当前页面显示出 hello world

## 页面间导航

next 中实现路由非常的简便,新建 pages/about.js

```javascript
export default function About() {
  return (
    <div>
      <p>This is the about page</p>
    </div>
  );
}
```

此时访问 localhost:6688/about,就可以看到页面相应的效果(路由与 pages 下的文件名称完全匹配)

页面间的导航,我们可以 a 标签来进行导航.但是,它不会执行客户端导航.并且,每次点击浏览器将向服务器请求下一页,同时刷新页面.因此,为了支持客户端导航,,我们需要使用 Next.js 的 Link API,该 API 通过导出 next/link. Link 将预取页面,并且导航将在不刷新页面的情况下进行.

### 使用 Link API

修改 pages/index.js

```javascript
import Link from 'next/link';

const Index = () => (
  <div>
    <Link href="/about">
      <a>About Page</a>
    </Link>
    <p>Hello Next.js</p>
  </div>
);

export default Index;
```

再次访问 localhost:6688,然后点击 About Page 跳转到 about 页面.之后点击浏览器的后退按钮,页面能够回到 index.

> Link组件默认是将路由push进入浏览器记录,所以点击后退按钮是返回上一页.这一默认形式可以替换为replace,更改为`<Link href="/about" replace>`

因为 next/link 只是一个更高阶的组件(高阶组件) , next/link 组件上的设置 props 无效.只接受 href 和类似的 props.如果需要向其添加 props,则需要对下级组件进行添加. next/link 组件不会将那些 props 传递给子组件,并且还会给你一个错误警告.在这种情况下,next/link 组件的子组件/元素是接受样式和其他 props 最好对象.它可以是任何组件或标签,唯一要求是能够接受 onClick 事件.

> 如果将功能组件作为子组件进行传递,则需要将功能组件包装`React.forwardRef`才能在`<Link>`使用

```xml
  <Link href="/about">
    <a className="redLink">About Page</a>
  </Link>

  <Link href="/show">
    <div>Show Page</div>
  </Link>
```

> 这是客户端导航；该操作在浏览器中进行,而无需向服务器发出请求.打开开发者工具 networks 进行查看

另外的客户端导航是Router

```javascript
import Router from 'next/router'

function Index() {
  return (
   <div>
      Click <span onClick={() => Router.push('/about')}>here</span> to read more
    </div>
  )
}

export default Index
```

[Next.js更多关于路由route的内容](https://nextjs.org/docs/#routing)

> 尽管实现代码的过程中以及官方案例中在Link组件里将a标签作为子元素传进去,但是实际使用中,a标签会造成路由切换失效的情况,酌情使用其他标签代替.

## 组件[#](https://www.cnblogs.com/mybilibili/p/11615055.html#组件)

目前 Next.js 代码都是关于页面的.我们可以通过导出 React 组件并将该组件放入 pages 目录来创建页面.然后,它将具有基于文件名的固定 URL. 但同时一些共享组件也是项目中必须的,我们将创建一个公共的 Header 组件并将其用于多个页面.

### 创建公用组件[#](https://www.cnblogs.com/mybilibili/p/11615055.html#创建公用组件)

新建 components/Header.js

```javascript
import Link from "next/link";

const linkStyle = {
  marginRight: 15
};

const Header = () => (
  <div>
    <Link href="/">
      <a style={linkStyle}>Home</a>
    </Link>
    <Link href="/about">
      <a style={linkStyle}>About</a>
    </Link>
    <Link href="/show">
      <a style={linkStyle}>Show</a>
    </Link>
  </div>
);

export default Header;
```

然后修改 pages 目录下的 index.js / about.js / show.js

```javascript
import Header from '../components/Header';

export default function Show() {
  return (
    <div>
      <Header />
      <p>Hello Next.js</p>
    </div>
  );
}
```

打开 localhost:6688 点击 3 个 link 按钮就可以进行页面间的来回跳转了

> 当前所使用的components这个名字并不是必须的,你可以将这个文件夹命名为任何名称.next中固定且不能改变的文件夹只有两个'pages'和'static'.next也并不限制将公共组件存放在pages里面,但最好不要在 pages 里面创建共享组件,这样会生成许多无效的路由.

### layout 组件[#](https://www.cnblogs.com/mybilibili/p/11615055.html#layout-组件)

在我们的应用中,我们将在各个页面上使用通用样式.为此,我们可以创建一个通用的 Layout 组件并将其用于我们的每个页面.

components/MyLayout.js

```javascript
import Header from './Header';

const layoutStyle = {
  margin: 20,
  padding: 20,
  border: '1px solid #DDD'
};

const Layout = props => (
  <div style={layoutStyle}>
    <Header />
    {props.children}
  </div>
);

export default Layout;
```

然后修改 pages 目录下的 index.js / about.js / show.js

```javascript
import Layout from '../components/MyLayout';

export default function Show() {
  return (
    <Layout>
      <p>Hello Next.js</p>
    </Layout>
  );
}
```

此外还可以使用 hoc 组件进行内容传递获取使用 props 属性进行传递.最终实现的是布局组件实现了多页面共用.

## 动态页面[#](https://www.cnblogs.com/mybilibili/p/11615055.html#动态页面)

在实际应用中,我们需要创建动态页面来显示动态内容.

首先修改 pages/about.js 文件

```javascript
import Layout from "../components/MyLayout";
import Link from "next/link";

const PostLink = props => (
  <li>
    <Link href={`/post?title=${props.title}`}>
      <a>{props.title}</a>
    </Link>
  </li>
);

export default function About() {
  return (
    <Layout>
      <h1>My Blog</h1>
      <ul>
        <PostLink title="Hello Next.js" />
        <PostLink title="Learn Next.js is awesome" />
        <PostLink title="Deploy apps with Zeit" />
      </ul>
    </Layout>
  );
}
```

同样的效果Link使用对象形式

```yaml
<Link href={{ pathname: '/post', query: { title: 'this is title' } }}>
```

依然同样的效果使用Router

```php
import Router from 'next/router'

const handler = () => {
  Router.push({
    pathname: '/about',
    query: { name: 'this is title },
  })
}
```

Router.push(url, as)与Link组件使用了相同的参数,第一个参数就是url,如果有第二参数,就是对应as

创建 pages/post.js

```javascript
import { useRouter } from 'next/router';
import Layout from '../components/MyLayout';

const Page = () => {
  const router = useRouter();

  return (
    <Layout>
      <h1>{router.query.title}</h1>
      <p>This is the blog post content.</p>
    </Layout>
  );
};

export default Page;
```

打开 localhost:6688 查看页面效果,点击 about 下面的 3 个帖子,会出现对应的 title 页面

- 我们通过查询字符串参数（查询参数）传递数据,通过查询字符串传递任何类型的数据.
- 我们导入并使用 useRouter 函数,next/router 函数将返回 Next.js router 对象.
- 我们使用 query 获取查询字符串参数
- 获得标题需要的参数 router.query.title.

post 页面也可以添加通用 header

```javascript
import { useRouter } from "next/router";
import Layout from "../components/MyLayout";

const Content = () => {
  const router = useRouter();

  return (
    <Layout>
      <h1>{router.query.title}</h1>
      <p>This is the blog post content.</p>
    </Layout>
  );
};

const Page = () => (
  <Layout>
    <Content />
  </Layout>
);

export default Page;
```

再次查看 localhost:6688 看看不同,现在的页面也具有一套完整的布局.

### 路由事件[#](https://www.cnblogs.com/mybilibili/p/11615055.html#路由事件)

可以通过Router监听路由器内部发生的不同事件

```csharp
// 监听
Router.events.on('routeChangeStart', handleRouteChange)
// 关闭
Router.events.off('routeChangeStart', handleRouteChange)
```

- routeChangeStart(url) 当路由开始改变时触发
- routeChangeComplete(url) 路由完全改变时触发
- routeChangeError(err, url) 更改路由时发生错误或取消路由负载时触发
- beforeHistoryChange(url) 在更改浏览器的历史记录之前触发
- hashChangeStart(url) 当哈希值改变但页面不改变时触发
- hashChangeComplete(url) 哈希值更改但页面未更改时触发

> 官方表示getInitialProps情况下不建议使用路由器事件.如果需要最好是在组件加载后或者某些事件后进行监听.

## 动态路由[#](https://www.cnblogs.com/mybilibili/p/11615055.html#动态路由)

当前我们的路由是这样的 http://localhost:6688/post?title=Hello Next.js , 现在需要更干净的路由 http://localhost:6688/p/10. 添加新页面来创建我们的第一个动态路由 p/[id].js

新建 pages/p/[id].js

```javascript
import { useRouter } from 'next/router';
import Layout from '../../components/MyLayout';

export default function Post() {
  const router = useRouter();

  return (
    <Layout>
      <h1>{router.query.id}</h1>
      <p>This is the blog post content.</p>
    </Layout>
  );
}
```

- next 会处理后面的路由/p/.例如,/p/hello-nextjs 将由此页面处理.而/p/post-1/another 不会.
- 方括号使其成为动态路由.而且在匹配动态路由的时候必须使用全名,无法添加前缀或者后缀.例如,/pages/p/[id].js 受支持,但/pages/p/post-[id].js 不受支持.
- 创建动态路由时,我们 id 放在方括号之间.这是页面接收到的查询参数的名称,因此/p/hello-nextjs 在 query 对象就是{ id: 'hello-nextjs'},我们可以使用 useRouter（）进行访问.

> `useRouter`是一个React钩子函数,它不能与类一起使用.类组件可以使用`withRouter`高阶组件或将类包装在功能组件中.同时withRouter也可以直接用于功能组件中.

在链接多个页面,新建 pages/page.js

```javascript
import Layout from '../components/MyLayout';
import Link from 'next/link';

const PostLink = props => (
  <li>
    <Link href="/p/[id]" as={`/p/${props.id}`}>
      <a>{props.id}</a>
    </Link>
  </li>
);

export default function Blog() {
  return (
    <Layout>
      <h1>My Blog</h1>
      <ul>
        <PostLink id="hello-nextjs" />
        <PostLink id="learn-nextjs" />
        <PostLink id="deploy-nextjs" />
      </ul>
    </Layout>
  );
}
```

`<Link href={`/p?id=${item.id}`} as={`/p/${item.id}`}>`,访问路径就是http://localhost:6688/p/975

`<Link href={`/p?id=${item.id}`}>`,访问路径就是http://localhost:6688/p?id=975

`<Link href={`/p?id=${item.id}`} as={`/post/${item.id}`}>`,起别名http://localhost:6688/post/975

在该页面中我们看一下元素,其中 href 属性 p 文件夹中页面的路径, as 是要在浏览器的 URL 栏中显示的 URL,这是next官方提供的一个路由遮挡功能,用来隐藏原本复杂的路由,显示出来简洁的路由.使网站路径更简洁.as 通常是用来与浏览器历史记录配合使用.

## 获取远程数据[#](https://www.cnblogs.com/mybilibili/p/11615055.html#获取远程数据)

实际上,我们通常需要从远程数据源获取数据.Next.js 自己有标准 API 来获取页面数据.我们通常使用异步函数 getInitialProps 来完成此操作 .这样,我们可以通过远程数据源获取数据到页面上,并将其作为 props 传递给我们的页面.getInitialProps 在服务器和客户端上均可使用.

首先需要一个获取数据的库

```css
npm install --save isomorphic-unfetch
```

然后修改 pages/index.js

```javascript
import Layout from '../components/MyLayout';
import Link from 'next/link';
import fetch from 'isomorphic-unfetch';

const Index = props => (
  <Layout>
    <h1>Batman TV Shows</h1>
    <ul>
      {props.shows.map(show => (
        <li key={show.id}>
          <Link href="/detail/[id]" as={`/detail/${show.id}`}>
            <a>{show.name}</a>
          </Link>
        </li>
      ))}
    </ul>
  </Layout>
);

Index.getInitialProps = async function() {
  const res = await fetch('https://api.tvmaze.com/search/shows?q=batman');
  const data = await res.json();

  return {
    shows: data.map(entry => entry.show)
  };
};

export default Index;
```

现在这种情况下,我们只会在服务器上获取数据,因为我们是在服务端进行渲染.

再创建一个详情页,这里用到了动态路由

新建 pages/detail/[id].js

```javascript
import Layout from "../../components/MyLayout";
import fetch from "isomorphic-unfetch";
import Markdown from "react-markdown";

const Post = props => (
  <Layout>
    <h1>{props.show.name}</h1>
    <div className="markdown">
      <Markdown source={props.show.summary.replace(/<[/]?p>/g, "")} />
    </div>
    <img src={props.show.image.medium} />
    <style jsx global>{`
     .markdown {
        font-family: "Arial";
      }

     .markdown a {
        text-decoration: none;
        color: blue;
      }

     .markdown a:hover {
        opacity: 0.6;
      }

     .markdown h3 {
        margin: 0;
        padding: 0;
        text-transform: uppercase;
      }
    `}</style>
  </Layout>
);

Post.getInitialProps = async function(context) {
  const { id } = context.query;
  const res = await fetch(`https://api.tvmaze.com/shows/${id}`);
  const show = await res.json();

  return { show };
};

export default Post;
```

点击 list 中的随便一个,然后打开控制台和浏览器的 networks,会发现这次是在浏览器端进行接口请求.

getInitialProps 上下文对象context具有以下属性

- pathname -URL的路径部分
- query -URL的查询字符串部分被解析为对象
- asPath- String实际路径（包括查询）的-在浏览器中显示
- req -HTTP请求对象（仅服务器）
- res -HTTP响应对象（仅服务器）
- err -渲染期间遇到任何错误的错误对象

## 给组件添加样式[#](https://www.cnblogs.com/mybilibili/p/11615055.html#给组件添加样式)

Next.js 在 JS 框架中预加载了一个称为 styled-jsx 的 CSS,该 CSS 使你的代码编写更轻松.它允许您为组件编写熟悉的 CSS 规则.规则对组件（甚至子组件）以外的任何东西都没有影响.简单来说就是带有作用域的 css.

修改 pages/page.js

```javascript
import Layout from "../components/MyLayout";
import Link from "next/link";

function getPosts() {
  return [
    { id: "hello-nextjs", title: "Hello Next.js" },
    { id: "learn-nextjs", title: "Learn Next.js is awesome" },
    { id: "deploy-nextjs", title: "Deploy apps with ZEIT" }
  ];
}

export default function Blog() {
  return (
    <Layout>
      <h1>My Blog</h1>
      <ul>
        {getPosts().map(post => (
          <li key={post.id}>
            <Link href="/p/[id]" as={`/p/${post.id}`}>
              <a>{post.title}</a>
            </Link>
          </li>
        ))}
      </ul>
      <style jsx>{`
        h1,
        a {
          font-family: "Arial";
        }

        ul {
          padding: 0;
        }

        li {
          list-style: none;
          margin: 5px 0;
        }

        a {
          text-decoration: none;
          color: red;
        }

        a:hover {
          opacity: 0.6;
        }
      `}</style>
    </Layout>
  );
}
```

在上面的代码中,我们直接写在模板字符串中,而且必须使用模板字符串（{``}）编写 CSS .

此时修改一下代码

```javascript
import Layout from "../components/MyLayout";
import Link from "next/link";

function getPosts() {
  return [
    { id: "hello-nextjs", title: "Hello Next.js" },
    { id: "learn-nextjs", title: "Learn Next.js is awesome" },
    { id: "deploy-nextjs", title: "Deploy apps with ZEIT" }
  ];
}

const PostLink = ({ post }) => (
  <li>
    <Link href="/p/[id]" as={`/p/${post.id}`}>
      <a>{post.title}</a>
    </Link>
  </li>
);

export default function Blog() {
  return (
    <Layout>
      <h1>My Blog</h1>
      <ul>
        {getPosts().map(post => (
          <PostLink key={post.id} post={post} />
        ))}
      </ul>
      <style jsx>{`
        h1,
        a {
          font-family: "Arial";
        }

        ul {
          padding: 0;
        }

        li {
          list-style: none;
          margin: 5px 0;
        }

        a {
          text-decoration: none;
          color: blue;
        }

        a:hover {
          opacity: 0.6;
        }
      `}</style>
    </Layout>
  );
}
```

这时候打开浏览器观察就会发现也是不生效,这是因为 style jsx 这种写法样式是有作用域,css 只能在当前作用域下生效.

解决 1 , 给子组件添加上子组件的样式

```javascript
const PostLink = ({ post }) => (
  <li>
    <Link href="/p/[id]" as={`/p/${post.id}`}>
      <a>{post.title}</a>
    </Link>
    <style jsx>{`
      li {
        list-style: none;
        margin: 5px 0;
      }

      a {
        text-decoration: none;
        color: blue;
        font-family: 'Arial';
      }

      a:hover {
        opacity: 0.6;
      }
    `}</style>
  </li>
);
```

解决 2 , [全局样式](https://github.com/zeit/styled-jsx#one-off-global-selectors)

```javascript
 <style jsx global>{`
......css
 `}
```

一般不使用全局样式来解决

[styled-jsx 文档](https://www.npmjs.com/package/styled-jsx)

### 使用全局样式[#](https://www.cnblogs.com/mybilibili/p/11615055.html#使用全局样式)

有时,我们确实需要更改子组件内部的样式.尤其是使用一些第三方库样式又有些不满意的时候.

安装 react-markdown

```css
npm install --save react-markdown
```

修改 pages/post.js

```javascript
import { useRouter } from "next/router";
import Layout from "../components/MyLayout";
import Markdown from "react-markdown";

const Content = () => {
  const router = useRouter();

  return (
    <Layout>
      <h1>{router.query.title}</h1>
      <div className="markdown">
        <Markdown
          source={`  # Live demo

                  Changes are automatically rendered as you type.

                  ## Table of Contents

                  * Implements [GitHub Flavored Markdown](https://github.github.com/gfm/)
                  * Renders actual, "native" React DOM elements
                  * Allows you to escape or skip HTML (try toggling the checkboxes above)
                  * If you escape or skip the HTML, no dangerouslySetInnerHTML is used! Yay!

                  ## HTML block below

                <blockquote>
                  This blockquote will change based on the HTML settings above.
                </blockquote>`
            }
        />
      </div>
      <style jsx global>{`
       .markdown {
          font-family: "Arial";
        }

       .markdown a {
          text-decoration: none;
          color: blue;
        }

       .markdown a:hover {
          opacity: 0.6;
        }

       .markdown h3 {
          margin: 0;
          padding: 0;
          text-transform: uppercase;
        }
      `}</style>
    </Layout>
  );
};

const Page = () => (
  <Layout>
    <Content />
  </Layout>
);

export default Page;
```

打开 localhost:6688 的 about 页面点击查看样式效果

[其他解决方案](https://github.com/zeit/next.js#css-in-js)

## 引入 ui 库[#](https://www.cnblogs.com/mybilibili/p/11615055.html#引入-ui-库)

目前代码在页面中呈现的样式是比较随意的,秉承着能打开就行的原则开发到这一步,是否应该稍微美化一下下.

### 引入 less[#](https://www.cnblogs.com/mybilibili/p/11615055.html#引入-less)

首先安装需要的库

```cmake
npm install --save @zeit/next-less less
```

然后把 mylayout 和 header 里面的行内样式去掉

新建 assets/css/styles.less

```css
.header {
  display: block;
  z-index: 500;
  width: 100%;
  height: 60px;
  font-size: 14px;
  background: #fff;
  color: rgba(0, 0, 0, 0.44);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  letter-spacing: 0;
  font-weight: 400;
  font-style: normal;
  box-sizing: border-box;
  top: 0;

  &:after {
    box-shadow: 0 2px 3px 0 rgba(0, 0, 0, 0.07);
    display: block;
    position: absolute;
    top: 60px;
    color: rgba(0, 0, 0, 0.07);
    content: "";
    width: 100%;
    height: 2px;
  }

 .header-inner {
    width: 1000px;
    margin: 0 auto;

    a {
      height: 60px;
      line-height: 60px;
      font-size: 18px;
      color: #c7c7c7;
      cursor: pointer;
      margin-right: 25px;
      &:hover {
        font-size: 18px;
        color: #2d2d2f;
      }
    }
  }
}

.content {
  width: 1000px;
  margin: 0 auto;
  padding-top: 30px;
}
```

修改 next.config.js

```java
// next.config.js
const withLess = require('@zeit/next-less')
module.exports = withLess({
  /* config options here */
})
```

在 MyLayout 里面引入 less

```cpp
import "../assets/css/styles.less";
```

在 localhost:6688 查看页面出现相应的样式

[next-less 文档](https://github.com/zeit/next-plugins/tree/master/packages/next-less)

### 引入 antd[#](https://www.cnblogs.com/mybilibili/p/11615055.html#引入-antd)

```mipsasm
npm install antd --save
npm install babel-plugin-import --save-dev

touch.babelrc
```

.babelrc

```json
{
  "presets": ["next/babel"],
  "plugins": [
    [
      "import",
      {
        "libraryName": "antd",
        "style": "less"
      }
    ]
  ]
}
```

之后引入 antd 的样式

assets/css/styles.less

```scss
@import "~antd/dist/antd.less";
```

这时候就是正常引入 antd 的组件进行使用就可以了

```javascript
import { Typography, Card, Avatar } from "antd";
const { Title, Paragraph, Text } = Typography;
```

### 错误解决(新版问题)[#](https://www.cnblogs.com/mybilibili/p/11615055.html#错误解决新版问题)

ValidationError: Invalid options object. CSS Loader has been initialised using an options object that does not match the API schema. - options has an unknown property 'minimize'. These properties are valid: #541

新版中 css-loader 和 webpack 会出现这样一个错误,这是升级过程中代码变更导致了,css-loader 已经没有 minimize 这一选项.

解决方法,在 next.config.js 添加去除代码

```javascript
const withLess = require("@zeit/next-less");

if (typeof require !== "undefined") {
  require.extensions[".less"] = file => {};
}

function HACK_removeMinimizeOptionFromCssLoaders(config) {
  console.warn(
    "HACK: Removing `minimize` option from `css-loader` entries in Webpack config"
  );
  config.module.rules.forEach(rule => {
    if (Array.isArray(rule.use)) {
      rule.use.forEach(u => {
        if (u.loader === "css-loader" && u.options) {
          delete u.options.minimize;
        }
      });
    }
  });
}

module.exports = withLess({
  lessLoaderOptions: {
    javascriptEnabled: true
  },
  webpack(config) {
    HACK_removeMinimizeOptionFromCssLoaders(config);
    return config;
  }
});
```

## 部署 Next.js 应用[#](https://www.cnblogs.com/mybilibili/p/11615055.html#部署-nextjs-应用)

先安装 now,一个静态资源托管服务器

```css
npm i -g now

now
```

等待一段时间之后会生成一个静态链接,点击打开就可以看到自己网页的样子了https://react-next-demo.fuhuodemao.now.sh

[zeit now 文档](https://zeit.co/docs#related)

### 打包生产环境代码[#](https://www.cnblogs.com/mybilibili/p/11615055.html#打包生产环境代码)

查看 package.json 的 script

```json
"dev": "next -p 6688",
"build": "next build",
"start": "next start -p 6688",
```

现在执行命令来生成代码并预览

```cpp
npm run build // 构建用于生产的Next.js应用程序
npm start // 在6688端口上启动Next.js应用程序.该服务器将进行服务器端渲染并提供静态页面
```

在 localhost:6688 上我们可以看到同样的效果

### 开启多个端口[#](https://www.cnblogs.com/mybilibili/p/11615055.html#开启多个端口)

修改 script 命令

```json
 "start": "next start -p 6688",
```

然后执行`npm start`,我们可以在 localhost:8866 上再次打开一个应用

在 window 下需要额外的工具 cross-env

```sql
npm install cross-env --save-dev
```





## 参考[#](https://www.cnblogs.com/mybilibili/p/11615055.html#参考)

- [官方文档](https://nextjs.org/docs)
- [learn next](https://nextjs.org/learn/basics/getting-started/setup)
- [中文文档](https://nextjs.frontendx.cn/docs)

