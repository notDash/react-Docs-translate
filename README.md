# [react 官方文档](https://facebook.github.io/react/docs/installation.html)翻译

此文档只是针对自己在学习[react](https://facebook.github.io/react/)的过程中进行的翻译。可能不是太专业，但是基于自己的理解，用于往后的查阅来说应该是足够了。

* [JSX的介绍](https://github.com/notDash/react-Docs-translate/blob/master/JSX%E4%BB%8B%E7%BB%8D.md)
* [渲染元素](https://github.com/notDash/react-Docs-translate/blob/master/渲染元素.md)
* [组件和Props](https://github.com/notDash/react-Docs-translate/blob/master/%E7%BB%84%E4%BB%B6%E5%92%8CProps.md)
* [State和生命周期](https://github.com/notDash/react-Docs-translate/blob/master/State%E5%92%8C%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.md)
* [事件处理](https://github.com/notDash/react-Docs-translate/blob/master/%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86.md)

## JSX 的介绍
    思考以下变量的声明：
    const element = <h1>Hello, world!<h1>
    以上有意思的语法，既不是字符串，也不是HTML。
    我们称之为JSX，它是基于JavaScript的扩展语法。我们推荐在React中使用JSX语法来描述UI应该是什么样子的。JSX也许会让你想到模板
    语言，但是它能够很好的与JavaScript结合，具有JavaScript同样强大的功能。
    
    JSX用于生成React的元素（elements），我们将在下一章节探讨如何把它们渲染到DOM上。接下来， 你将会了解到开始使用JSX需要掌握的
    一些必要知识。
    
### 1. 在JSX中嵌入表达式
    在JSX中可以嵌入任何JavaScript 表达式，通过使用一对花括号“{}”进行包裹。
    例如2 + 2, user.firstName, 和 formatName(user)都是有效的表达式：
```javascript
      import React from 'react';
      import ReactDom from 'react-dom';
      function formatName(user) {
          return user.firstName + ' ' + user.lastName;
      }

      const user = {
          firstName: 'Harper',
          lastName: 'Perez'
      };

      const element = (
          <h1>
              Hello, {formatName(user)}!
          </h1>
      );

      ReactDom.render(
          element,
          document.getElementById('root')
      );
```

    输出结果为：  Hello, Harper Perez!
    为了方便阅读，我们把JSX分割成多行来书写，当然并不是必须的，但是如果要这么做的话，我们还是建议用圆括号括起来，从而避免分号自动
    插入的陷阱。
    
### 2. JSX也是一种表达式
    编译之后，JSX表达式就会变成普通的JavaScript对象。
    这意味着你可以在if以及for语句块里使用JSX，赋值给一个变量，当做参数传递以及作为函数的返回值：
```javascript
      function getGreeting(user) {
        if (user) {
          return <h1>Hello, {formatName(user)}!</h1>;
        }
        return <h1>Hello, Stranger.</h1>;
      }
```

### 3. 使用JSX来指定属性值
    你可以使用引号来为属性指定字符串常量值：
```javascript
      const element = <div tabIndex="0"></div>;
```

    你也可以在属性值中使用花括号来嵌套JavaScript表达式：

```javascript
    const element = <img src={user.avatarUrl}></img>;
```

    当使用花括号来嵌套JavaScript表达式的时候，不要在花括号外面使用引号来嵌套。否则JSX将会把该值当做一个字符串字面量来处理，而不是
    表达式。在同一个属性值当中，你可以使用花括号和引号中的任意一个，但是不能两个都同时使用。
    
### 4. 使用JSX来指定子元素
    如果一个标签是空的，你可以使用‘/>’进行标签的闭合。看如下的XML:

```javascript
    const element = <img src={user.avatarUrl} />;
```

    JSX标签可能会包含子元素：

```javascript
    const element = (
        <div>
          <h1>Hello!</h1>
          <h2>Good to see you here.</h2>
        </div>
      );
```

      警告：比起HTML，JSX更接近于JavaScript，所以JSX采用驼峰式的属性命名约定来代替HTML中的属性名。
      例如在JSX中，class变为className, tabindx变为tabIndex。
      
### 5. JSX阻止了注入攻击
    在JSX中嵌入用户的输入是安全的：

```javascript
      const title = response.potentiallyMaliciousInput;
      // This is safe:
      const element = <h1>{title}</h1>;
```

    默认情况下， 在渲染之前，React Dom会把嵌套在JSX里的任何值进行转义，也就是意味着，你无法注入任何非明确在应用中指定的东西。
    在渲染之前，所有的值都会被转换成字符串，这帮助解决了跨站点攻击。
    
### 6. JSX 是对象
     Babel 把 JSX 语法编译之后，其实就是调用了 React.createElement().
     以下的两个实例是等效的：
```javascript     
     const element = (
       <h1 className="greeting">
          Hello, world!
       </h1>
     );
     const element = React.createElement(
       'h1',
       {className: 'greeting'},
       'Hello, world!'
     );
    
     React.createElement() 在执行的时候进行了一些校验以帮助我们写出更加健壮的代码，本质上它创建了如下的一个对象：
        // 注意: 当前这个结构是简化了的
        const element = {
          type: 'h1',
          props: {
            className: 'greeting',
            children: 'Hello, world'
          }
        };
```

    这样的对象我们称之为React元素。你可以把它想象为我们在屏幕上所看到元素的一个描述。React读取这些对象，使用它们
    来构造DOM，并且保持实时更新。
    
    我们将在下一章节讨论如何把React的元素渲染到DOM上。
    
    提示： 我们推荐使用支持Babel的编辑器，从而es6和JSX语法均能够正确的高亮显示。

## 渲染元素
    元素是React应用中最小的构建模块。
    一个元素描述了你想要在屏幕上看到的东西：
```javascript    
      const element = <h1>Hello, world<h1/>;
```

    不像浏览器DOM元素那样，React元素是简单的对象，并且易于创建。（浏览器的元素创建代价比较昂贵）React DOM
    兼顾更新DOM去匹配React元素。
    
    注意： 另外一个被大家熟知的概念“组件”可能会使元素理解起来比较困惑。我们将在下一章节讲解组件。元素是组件
    的组成部分， 我们建议在阅读下一章节前仔细阅读该部分的内容。
    
### 1. 把元素渲染到DOM
    假设在你的HTML文件中有一个<div>元素：
        <div id="root"></div>
    我们称之为根元素节点，因为在该元素当中的所有内容将会由React DOM 所管理。
    使用React构建的应用通常来说只有单一的一个根节点。如果你想在现有的应用中集成React的话，那你可以定义多个
    孤立的根节点。
    
    渲染React元素到根DOM节点上，把二者都传递给ReactDom.render():
```javascript    
        const element = <h1>Hello, world</h1>;
        ReactDOM.render(
            element,
            document.getElementById('root')
        );
```

    在页面中将会显示： Hello, world
    
### 2. 更新已经渲染的元素
    React元素是不可变的。只要创建一个元素之后，就不能改变它的子元素和属性。一个元素就像是电影中的某一帧：它
    代表了在某一个确定的时间点上的UI展现。
    
    根据我们目前的知识，更新UI的唯一途径是创建一个新的元素，传递给ReactDOM.render()进行渲染。
    
    思考以下时钟的示例：
```javascript    
        function tick() {
            var element = (
                <div>
                  <h1>Hello, world!</h1>
                  <h2>It is {new Date().toLocaleTimeString()}.</h2>
                </div>
            );
            ReactDom.render(
                element,
                document.getElementById('root')
            );
        }
        
        setInterval(tick, 1000);
```

    每隔一秒钟setInteval的回调就会去调用一次ReactDom.render（）。
        
    注意： 在实践中，大多数的React应用只调用一次ReactDom.render（）。下一个章节中，我们将会学习如何在
    有状态的组件（stateful components）中去封装这样的代码。
    
### 3. React只会更新需要更新的元素
    React DOM会比较前一个元素以及它的子元素，并且只会去更新需要更新的DOM，从而达到预期的效果。
    你可以运行上一个例子，通过浏览器工具来进行验证。（以下为实例的截图）
![image](https://github.com/notDash/react-Docs-translate/blob/master/ticking-clock.png)
   
    尽管我们在每一秒钟的时候创建了一个描述整个UI树的元素，但是只有文本节点当中的内容被React DOM更新了。



## 组件和Props
    组件可以让你把UI分成独立的，可服用的一些代码块。每一个代码块都可以单独的考虑。
    
    在概念上，组件类似于JavaScript的函数（function）。他们接收任意的输入（称之为“props”），返回一个React 元素，
    描述了屏幕上应该显示的信息。
    
### 1. 函数（functional）和类（class）申明的组件
    定义组件的最简单的方式是写一个JavaScript函数：
```javascript    
      function Welcome(props) {
        return <h1>Hello, {props.name}</h1>;
      }
```

    这个函数是一个有效的React组件，因为它接收一个单一的props对象作为参数，并且返回了一个React元素。我们称之为
    函数化的组件，因为从字面上来看它就是一个JavaScript函数。
    
    你同样可以使用es6的class来定义一个组件：

```javascript
    class Welcome extends React.Component {
          render() {
            return <h1>Hello, {this.props.name}</h1>;
          }
      }
```

     从React的角度来看，以上两个组件是等价的。
     
     Classes 有一些其他的特性，我们将在下一章节探讨。鉴于函数化的组件简洁，我们将使用它来进行组件的定义。
     
### 2. 渲染组件
    在上面的示例中，我们仅仅使用到了表示DOM标签的React元素：

```javascript    
      const element = <div />;
```

    另外，元素也可以是用户自定义的组件：

```javascript
    const element = <Welcome name="Sara" />;
```

    当React遇到用户自定义的组件的时候，它把JSX属性当做一个对象传递给组件，我们把改对象称之为“props”。
    例如，下面的代码将会把“Hello, Sara”渲染到上：

```javascript    
      function Welcome(props) {
          return <h1>Hello, {props.name}</h1>;
      }
      const element = <Welcome name="Sara" />;
      ReactDOM.render(
        element,
        document.getElementById('root')
      );
```

    让我们回顾下这个示例都发生了什么：
      1. 我们调用ReactDom.render()来渲染<Welcome name="Sara" />
      2. React调用Welcome组件，把{name: 'Sara'}作为props
      3. Welcome组件返回一个<h1>Hello, Sara</h1>元素作为结果。
      4. React DOM会及时的更新DOM去匹配 <h1>Hello, Sara</h1>
      
    警告： 
    组件名称要以大写字母开头。
    例如：<div />表示DOM标签，但是<Welcome />代表一个组件。
    
### 3. 组件的组合
    组件可以被另外一个组件所引用。这使得我们可以在不同层级的详细信息中使用同一个组件。一个button，form，
    dialog，screen：在React中，这些通常都表现为一个组件。
    
    例如，我们可以创建一个多次渲染 Welcome 的组件App:
    
```javascript
      function Welcome(props) {
        return <h1>Hello, {props.name}</h1>;
      }
      function App() {
          return (
            <div>
              <Welcome name="Sara" />
              <Welcome name="Cahal" />
              <Welcome name="Edite" />
            </div>
          );
      }
      ReactDOM.render(
          <App />,
          document.getElementById('root')
      );
```

    典型的，新的React应用会在顶部有一个单一的App组件。然而，如果是在已有的项目中集成React，你可以通过自下而上的方式，从一个小的组件例如Botton开始，按照你自己的方式逐渐的由下而上
    到达视图的顶部。

    警告： 
    组件必须返回一个单一根元素的元素。这就是为什么我们添加一个<div>来包含所有的<Welcome />
    元素了。

### 4. 组件的提取
    不要担心把组件切分为更小的组件。
    例如，思考如下的 Comment 组件：

```javascript    
      function Comment(props) {
          return (
            <div className="Comment">
              <div className="UserInfo">
                <img className="Avatar"
                  src={props.author.avatarUrl}
                  alt={props.author.name}
                />
                <div className="UserInfo-name">
                  {props.author.name}
                </div>
              </div>
              <div className="Comment-text">
                {props.text}
              </div>
              <div className="Comment-date">
                {formatDate(props.date)}
              </div>
            </div>
          );
      }
```

    它接收author(一个对象)，text(字符串)，以及date(日期)作为props，描述了一个社交媒体
    网站上的评论。

    因为嵌套的关系，这个评论组件不好去做改动，也难于重用其中的个人信息部分。让我们从中提取
    一些组件。
    首先，我们将提取Avatar：

```javascript    
      function Avatar(props) {
          return (
              <img className="Avatar"
              src={props.user.avatarUrl}
              alt={props.user.name}
             />
          );
      }
```

    Avatar组件并不需要知道在Comment组件当中被渲染了。这也是为什么我们把prop的name定义为
    更加通用的user，而不是author。

    我们更加推荐从组件自身的视图角度来命名props，而不是通过他被使用的场景来定义。

    现在我们可以稍微的简化下Comment:
```javascript    
        function Comment(props) {
          return (
            <div className="Comment">
              <div className="UserInfo">
                <Avatar user={props.author} />
                <div className="UserInfo-name">
                  {props.author.name}
                </div>
              </div>
              <div className="Comment-text">
                {props.text}
              </div>
              <div className="Comment-date">
                {formatDate(props.date)}
              </div>
            </div>
          );
        }
```

    下一步我们将要提取UserInfo组件，该组件紧挨着用户的姓名，渲染一个Avatar组件：
```javascript    
        function UserInfo(props) {
          return (
            <div className="UserInfo">
              <Avatar user={props.user} />
              <div className="UserInfo-name">
                {props.user.name}
              </div>
            </div>
          );
        }
```

    这让我们进一步的简化Comment组件：
```javascript    
        function Comment(props) {
          return (
            <div className="Comment">
              <UserInfo user={props.author} />
              <div className="Comment-text">
                {props.text}
              </div>
              <div className="Comment-date">
                {formatDate(props.date)}
              </div>
            </div>
          );
        }
```

    提取组件似乎是一件枯燥乏味的工作，但是在大型的应用中提供了可重用组件的模板。首要的一个原则
    就是如果UI中的一部分被使用了多次(Button, Panel, Avatar)，或者足够的复杂（pp, 
    FeedStory,Comment），它将会是可重用组件的最佳候选。

### 5. Props是只读的
    不管你是通过function或者是class的方式申明的组件，它从不会修改它自身的props。思考如下的
    Sum函数：
```javascript    
        function sum(a,b) {
            renturn a + b;
        }  
```

    类似这样的函数我们称之为纯粹(pure)的函数，因为这样的函数不会试图去改变他们的输入参数，
    并且相同的输入总是返回相同的结果。

    相反，如下的函数并不是纯粹的，因为它改变了它的入参。
```javascript    
        function withdraw(account, amount) {
          account.total -= amount;
        }
```

    React非常的灵活，但是它有一条非常严格的规则：
        所有的React组件必须像纯粹的函数一样，不去影响到他们的props。
    当然，应用程序的UI是随时在变化的。在下一个章节，我们将介绍一个新的概念：state。State允许
    React组件根据用户的动作、网络的响应以及其他的事件来改变他们的输出，但是并不违背这一原则。
