# 组件和Props([Components and Props](https://facebook.github.io/react/docs/components-and-props.html))
    组件可以让你把UI分成独立的，可服用的一些代码块。每一个代码块都可以单独的考虑。
    
    在概念上，组件类似于JavaScript的函数（function）。他们接收任意的输入（称之为“props”），返回一个React 元素，
    描述了屏幕上应该显示的信息。
    
## 1. 函数（functional）和类（class）申明的组件
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
     
## 2. 渲染组件
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
    
## 3. 组件的组合
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

    典型的，新的React应用会在顶部有一个单一的App组件。然而， 如果是在已有的项目中集成React，你可以通过自下而上的
    方式，从一个小的组件
    
    警告： 
    组件必须返回一个单一根元素的元素。这就是为什么我们添加一个<div>来包含所有的<Welcome />
    元素了。

## 4. 组件的提取
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

## 5. Props是只读的
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
    
    
    
    
