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
              </div>
            );
          }
        }
```

    2) 给class添加构造函数（constructor），给this.state进行初始化
```javascript
    class Clock extends React.Component {
      constructor(props) {
        super(props);
        this.state = {date: new Date()};
      }

      render() {
        return (
          <div>
            <h1>Hello, world!</h1>
            <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
          </div>
        );
      }
    }
```

    注意constructor中是如何传递props的。
```javascript
    constructor(props) {
     super(props);
     this.state = {date: new Date()};
    }
```

    Class组件应该总是要调用基类的构造函数并传递props作为参数。
    3) 删除<Clock />元素中的date属性
```javascript
        ReactDOM.render(
          <Clock />,
          document.getElementById('root')
        );
```

    稍后我们将把定时器添加到组件当中去。
    结果如下：
```javascript
        class Clock extends React.Component {
          constructor(props) {
            super(props);
            this.state = {date: new Date()};
          }

          render() {
            return (
              <div>
                <h1>Hello, world!</h1>
                <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
              </div>
            );
          }
        }

        ReactDOM.render(
          <Clock />,
          document.getElementById('root')
        );
```

    接下来，我们将让Clock来设置自身的定时器， 并进行每秒更新。
    
## 给class添加生命周期方法
    在多组件的应用当中，当组件销毁的时候，释放组件占用的资源至关重要。
    当Clock被首次渲染到DOM的时候，我们想要去设置定时器。在React中称之为“mounting”。
    当通过Clock渲染的DOM被移除的时候，我们也想要去清楚定时器。在React中称之为“unmounting”。
    当一个组件装载和卸载的时候，我们可以在class组件中声明特殊的方法来执行相应的代码：
```javascript
    class Clock extends React.Component {
      constructor(props) {
        super(props);
        this.state = {date: new Date()};
      }

      componentDidMount() {

      }

      componentWillUnmount() {

      }

      render() {
        return (
          <div>
            <h1>Hello, world!</h1>
            <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
          </div>
        );
      }
    }
```

    这些方法我们称之为“生命周期钩子”函数。
    componentDidMount()是在组件被渲染到DOM之后调用的。在这儿设置定时器是比较理想的。
```javascript
    componentDidMount() {
        this.timerID = setInterval(
          () => this.tick(),
          1000
        );
      }
```

    注意我们是如何把定时器ID通过this进行保存的。
    当React初始化好this.props，以及this.state之后，如果你需要存储一些额外的信息，你可以自主的添加一些属性。
    如果在render中不使用的东西，那它不应该出现在state中。
    我们将在componentWillUnmount() 中销毁定时器：
```javascript
      componentWillUnmount() {
        clearInterval(this.timerID);
      }
```

    最后我们来实现每秒钟执行一次的tick()方法。
    它将使用this.setState()去更新组价的state:
```javascript
        class Clock extends React.Component {
          constructor(props) {
            super(props);
            this.state = {date: new Date()};
          }

          componentDidMount() {
            this.timerID = setInterval(
              () => this.tick(),
              1000
            );
          }

          componentWillUnmount() {
            clearInterval(this.timerID);
          }

          tick() {
            this.setState({
              date: new Date()
            });
          }

          render() {
            return (
              <div>
                <h1>Hello, world!</h1>
                <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
              </div>
            );
          }
        }

        ReactDOM.render(
          <Clock />,
          document.getElementById('root')
        );
```

    现在这个时钟将会每秒钟执行一次。
    现在让我们快速的回顾下发生了什么以及方法的执行顺序是什么样的：
    1）当<Clock />被传递给ReactDOM.render()的时候。React 调用Clock组建的构造函数，由于Clock需要展示当前时间，
    它使用一个包含当前时间的对象初始化this.state。稍后将会更新这个state。
    2）紧接着React 调用Clock组件的render()方法。React通过render()了解到如何在页面进行展示。然后React根据render()的输出进行页面的渲染。
    3）当Clock组件被渲染到DOM上之后，React调用componentDidMount()钩子函数。在函数里，Clock组件请求浏览器去调用每秒执行一次的tick()方法。
    4）浏览器每一秒调用一次tick()方法。在方法内部，Clock组件调用setState()方法，通过传递一个包含当前时间的参数对象来更新UI。通过setState()，React知道state的变化，然后再一次调用render()方法，去对比看看应该展示什么。此时，在render()方法里的this.state.date将会不一样，所以渲染输出的内容将包含更新的时间。React 将会相应的更新DOM。
    5）一旦Clock组件从DOM中移除之后，React将会调用componentWillUnmount()钩子
    函数，定时任务将会被阻止。

## 正确的使用State
    关于setState()，有以下3点需要注意。
### 不要直接修改State
    例如，以下的代码并不会重新渲染组件：
```javascript
    // Wrong
    this.state.comment = 'Hello';
```

    应该使用setState()代替：
```javascript
    // Correct
    this.setState({comment: 'Hello'});
```

    只能在constructor中指定this.state。

### State的更新可能是异步的
    为了提高性能，React会一次性批量的处理多个setState()的调用。
    由于this.props和 this.state 可能会异步的更新，所以不应该依赖于他们的值来计算下一个状态值。
    例如，下面的代码可能会失败：
```javascript
    // Wrong
    this.setState({
      counter: this.state.counter + this.props.increment,
    });
```
    
    利用传递一个函数而不是对象的形式来修正一下。该函数接受前一个状态值作为第一个参数，当前需要更新的props作为第二个参数：
```javascript
    // Correct
    this.setState((prevState, props) => ({
      counter: prevState.counter + props.increment
    }));
```

    上面我们使用了箭头函数，通过普通函数的方式也可以：
```javascript
    // Correct
    this.setState(function(prevState, props) {
      return {
        counter: prevState.counter + props.increment
      };
    });
```

### 状态的更新会进行合并
    当调用setState()的时候，React 会把你提供的对象与当前的state进行合并。
    例如，你的state可能包含多个独立的变量：
```javascript
    constructor(props) {
      super(props);
      this.state = {
        posts: [],
        comments: []
      };
    }
```

    你可以通过分别调用setState()来进行更新。
```javascript
    componentDidMount() {
      fetchPosts().then(response => {
        this.setState({
          posts: response.posts
        });
      });

    fetchComments().then(response => {
        this.setState({
          comments: response.comments
        });
      });
    }
```

    合并是互不影响的，所以this.setState({comments})并不会影响 this.state.posts，只会完全的替换掉this.state.comments。

### 数据的向下流转
