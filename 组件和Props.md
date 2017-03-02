# 组件和Props
    组件可以让你把UI分成独立的，可服用的一些代码块。每一个代码块都可以单独的考虑。
    
    在概念上，组件类似于JavaScript的函数（function）。他们接收任意的输入（称之为“props”），返回一个React 元素，
    描述了屏幕上应该显示的信息。
    
## 1. 函数（functional）和类（class）申明的组件
    定义组件的最简单的方式是写一个JavaScript函数：
      function Welcome(props) {
        return <h1>Hello, {props.name}</h1>;
      }
    这个函数是一个有效的React组件，因为它接收一个单一的props对象作为参数，并且返回了一个React元素。我们称之为
    函数化的组件，因为从字面上来看它就是一个JavaScript函数。
    
    你同样可以使用es6的class来定义一个组件：
      class Welcome extends React.Component {
          render() {
            return <h1>Hello, {this.props.name}</h1>;
          }
      }
     从React的角度来看，以上两个组件是等价的。
     
     Classes 有一些其他的特性，我们将在下一章节探讨。鉴于函数化的组件简洁，我们将使用它来进行组件的定义。
     
## 2. 渲染组件
    在上面的示例中，我们仅仅使用到了表示DOM标签的React元素：
      const element = <div />;
    另外，元素也可以是用户自定义的组件：
      const element = <Welcome name="Sara" />;
    
    当React遇到用户自定义的组件的时候，它把JSX属性当做一个对象传递给组件，我们把改对象称之为“props”。
    例如，下面的代码将会把“Hello, Sara”渲染到上：
      function Welcome(props) {
          return <h1>Hello, {props.name}</h1>;
      }
      const element = <Welcome name="Sara" />;
      ReactDOM.render(
        element,
        document.getElementById('root')
      );
    
    让我们回顾下这个示例都发生了什么：
    
    
