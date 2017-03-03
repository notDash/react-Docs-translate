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
    
    
    
