# State和生命周期([State and Lifecycle](https://facebook.github.io/react/docs/state-and-lifecycle.html))
    思考一下[上一章节]()中的时钟示例。到目前为止，我们只学习了一种更新UI的方法。
    我们调用ReactDOM.render()来改变渲染的输出：
```javascript
  function tick() {
    const element = (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {new Date().toLocaleTimeString()}.</h2>
      </div>
    );
    ReactDOM.render(
      element,
      document.getElementById('root')
    );
  }

  setInterval(tick, 1000);
```

    在这一部分，我们将学习如何使 Clock 组件具有封装性和可重用性。它将会设置自身的一个定时器，并每秒钟自己更新一次。
    我们可以从封装Clock 开始：
```javascript
    function Clock(props) {
      return (
        <div>
          <h1>Hello, world!</h1>
          <h2>It is {props.date.toLocaleTimeString()}.</h2>
        </div>
      );
    }
    
    function tick() {
      ReactDOM.render(
        <Clock date={new Date()} />,
        document.getElementById('root')
      );
    }
    
    setInterval(tick, 1000);
```

    但是，以上的写法忽略了关键的一点：设置每秒更新UI的定时器，应该属于Clock内部里的一个具体实现。
    理想的方式是我们只写一次，然后让Clock组件自己进行更新：
```javascript
    ReactDOM.render(
      <Clock />,
      document.getElementById('root')
    );
```

    要实现这样的方式，我们需要向Clock组件中添加“State”。
    State很像props，区别在于State是属于Component私有的，完全有Component进行控制。
    我们之前说过通过classes的方式定义组件有一些特性。
    State就是其中的一个特性。
    
## 转换function为class

    转换一个函数化的组件为class的形式，只需要一下5步:
        1. 创建一个具有相同名字、继承自React.Component的ES6语法的class。
        2. 添加一个空的方法render()。
        3. 把函数体里的内容拷贝到render()方法里。
        4. 在render()里，把props替换为this.props。
        5. 删除之前的函数声明
```javascript
        class Clock extends React.Component {
            render() {
                return (
                    <div>
                        <h1>Hello, world!</h1>
                        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
                    </div>
                );
            }
        }
```

    现在Clock 组件就是以class来定义的了。
    我们现在就可以使用一些特性了。比如state以及生命周期的钩子函数了。
    
## 给class添加state
    通过以下3个步骤，我们把date从props移动到state:
    1) 在render()方法中，把this.props.date替换为this.state.date
```javascript
        class Clock extends React.Component {
          render() {
            return (
              <div>
                <h1>Hello, world!</h1>
                <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
                -----------------------------------------------------
              </div>
            );
          }
        }
```
