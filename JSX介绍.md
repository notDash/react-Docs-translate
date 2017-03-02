
## 1. JSX 的介绍
    思考以下变量的声明：
    const element = <h1>Hello, world!<h1>
    以上有意思的语法，既不是字符串，也不是HTML。
    我们称之为JSX，它是基于JavaScript的扩展语法。我们推荐在React中使用JSX语法来描述UI应该是什么样子的。JSX也许会让你想到模板
    语言，但是它能够很好的与JavaScript结合，具有JavaScript同样强大的功能。
    
    JSX用于生成React的元素（elements），我们将在下一章节探讨如何把它们渲染到DOM上。接下来， 你将会了解到开始使用JSX需要掌握的
    一些必要知识。
    
## 2. 在JSX中嵌入表达式
    在JSX中可以嵌入任何JavaScript 表达式，通过使用一对花括号“{}”进行包裹。
    例如2 + 2, user.firstName, 和 formatName(user)都是有效的表达式：

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
  
    输出结果为：  Hello, Harper Perez!
    为了方便阅读，我们把JSX分割成多行来书写，当然并不是必须的，但是如果要这么做的话，我们还是建议用圆括号括起来，从而避免分号自动
    插入的陷阱。
    
## 3. JSX也是一种表达式
    编译之后，JSX表达式就会变成普通的JavaScript对象。
    这意味着你可以在if以及for语句块里使用JSX，赋值给一个变量，当做参数传递以及作为函数的返回值：
      function getGreeting(user) {
        if (user) {
          return <h1>Hello, {formatName(user)}!</h1>;
        }
        return <h1>Hello, Stranger.</h1>;
      }
  
## 4. 使用JSX来指定属性值
    你可以使用引号来为属性指定字符串常量值：
      const element = <div tabIndex="0"></div>;
    你也可以在属性值中使用花括号来嵌套JavaScript表达式：
      const element = <img src={user.avatarUrl}></img>;
      
    当使用花括号来嵌套JavaScript表达式的时候，不要在花括号外面使用引号来嵌套。否则JSX将会把该值当做一个字符串字面量来处理，而不是
    表达式。在同一个属性值当中，你可以使用花括号和引号中的任意一个，但是不能两个都同时使用。
    
## 5. 使用JSX来指定子元素
    如果一个标签是空的，你可以使用‘/>’进行标签的闭合。看如下的XML:
      const element = <img src={user.avatarUrl} />;
    
    JSX标签可能会包含子元素：
      const element = (
        <div>
          <h1>Hello!</h1>
          <h2>Good to see you here.</h2>
        </div>
      );
    
      警告：比起HTML，JSX更接近于JavaScript，所以JSX采用驼峰式的属性命名约定来代替HTML中的属性名。
      例如在JSX中，class变为className, tabindx变为tabIndex。
      
## 6. JSX阻止了注入攻击
    在JSX中嵌入用户的输入是安全的：
      const title = response.potentiallyMaliciousInput;
      // This is safe:
      const element = <h1>{title}</h1>;
      
    默认情况下， 在渲染之前，React Dom会把嵌套在JSX里的任何值进行转义，也就是意味着，你无法注入任何非明确在应用中指定的东西。
    在渲染之前，所有的值都会被转换成字符串，这帮助解决了跨站点攻击。
    
## 7. JSX 是对象
    Babel 把 JSX 语法编译之后，其实就是调用了 React.createElement().
    以下的两个实例是等效的：
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
    这样的对象我们称之为React元素。你可以把它想象为我们在屏幕上所看到元素的一个描述。React读取这些对象，使用它们
    来构造DOM，并且保持实时更新。
    
    我们将在下一章节讨论如何把React的元素渲染到DOM上。
    
    提示： 我们推荐使用支持Babel的编辑器，从而es6和JSX语法均能够正确的高亮显示。
