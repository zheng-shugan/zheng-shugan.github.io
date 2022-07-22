# React组件间通信


Hi，作为一个初次学习 React 的小白在了解了`state`和`props`之后就可以快乐的在父组件中传递数据给子组件，然后在子组件里使用父组件的回调函数去修改父组件的`state`了。但是在兄弟组件再像这样传递数据就会变得很麻烦了，父组件需要分别传递回调函数和数据给两个子组件，然后子组件再调用相应的属性和回调才能传递数据。

比如说要实现一个在`Search`组件和`List `组件之间传递数据：

```javascript
// App 组件
class App extends Component {
  state = {
    users: [], 			// 初始化状态，users 是数组
    isFirst: true, 		// 是否为第一次打开
    isLoading: false, 	// 标识是否处于加载中
    err: '', 			// 存储请求相关错误信息
  }

  // 更新 App 的 state；传递给 Search 组件
  updateAppState = (stateObj) => {
    this.setState(
      stateObj
    )
  }

  render() {
    return (
      <div className="container">
        
        <Search updateAppState={this.updateAppState}/>
        <List {...this.state}/>
      </div>
    );
  }
}

// List 组件
class List extends Component {
  render() {
    const {users, isFirst, isLoading, err} = this.props
    return (
      <div className="row">
        {
          // 根据父组件中的 state 来选择展示页面
          isFirst ? <h2>欢迎输入，输入关键词搜索</h2> :
            isLoading ? <h2>Loading</h2> :
              err ? <h2 style={{color: 'red'}}>{err}</h2>:
          users.map((userObj) => {
            return(
               	// 页面内容
            )
          })
        }
      </div>
    );
  }
}

// Search 组件
class Search extends Component {
 
  search = () => {
    // 获取输入（连续解构赋值+重命名
    const {keywordElement: {value: keyword}} = this
    // 发送请求前通知 App 更新 state；通过调用父组件传递的函数更新父组件中的数据
    this.props.updateAppState({isFirst: false, isLoading: false})
    // 发送网络请求
    axios.get(`http://localhost:3000/api1/search/users?q=${keyword}`).then(
      response => {
        // 发送请求成功通知 App 更新 state；通过调用父组件传递的函数更新父组件中的数据
        this.props.updateAppState({isLoading: false, users: response.data.items})
      },
      error => {
        // 发送请求失败通知 App 更新 state；通过调用父组件传递的函数更新父组件中的数据
        this.props.updateAppState({isLoading: false, err: error.message})
      }
    );
  }
}为了在两个组件之间传递数据我们不得不通过父组件`App`给两个组件传递函数和属性，明明是两个兄弟组件传递起数据来别别扭扭的感觉是有仇一样的组件之间的交流全靠父组件。这样在组件之间传递数据的方式决不是一个好的选择，于是乎就有了**消息订阅与发布机制**，[PubSub.js](https://github.com/mroderick/PubSubJS) 就是这样的一个工具库。
```

README 就有 Example 来知指导我们怎么下载和导入，在这就不多赘述。安装、导入之后就可以开始重构我们的代码了。

先考虑 App 组件。App 组件中的`state`部分是为了在两个子组件传递数据才设置的，现在可以直接在两个组件之间传递数据，而在`Search`组件中需要通过数据来更新展示页面所以可以把 App 组件中的`state`部分剪切到`Search`组件中；数据都到了`Search`组件中那么原来为了修改`state`的函数`updateAppState`也就不需要了；对应的也不需要再往子组件里传递`props`了。

```javascript
class App extends Component {

  render() {
    return (
      <div className="container">
        <Search />
        <List />
      </div>
    );
  }
}
```

接下来我们需要将`Search`组件中得到的数据传递到`List`组件。在这个关系中`Search`的发送方对应 PubSub 关系中的 puhlisher，`List`对应的是 subscriber。我们在这个项目中只需要知道怎么订阅和取消订阅即可，文档中对于的部分为 Basic example 和 Cancel specific subscription。

```javascript
// 其中的 msg 是订阅消息的名称，data 是传递的数据
var mySubscriber = function (msg, data) {
    console.log(msg, data);
};

// 先推送后订阅
// 推送一个订阅名称为 MY TOPIC 的消息，内容为 hello world!
PubSub.publish('MY TOPIC', 'hello world!');

// 创建一个 token 接收来自 MY TOPIC 推送
var token = PubSub.subscribe('MY TOPIC', mySubscriber);
// 根据 token 取消订阅消息
PubSub.unsubscribe(token);
```

在知道了怎么推送/订阅消息之后我们还要考虑什么时候订阅和取消订阅推送，正常的逻辑是在这个组件加载染到页面时才订阅消息，组件被卸载后就取消订阅。所以我们就可以在`componentDidMount`中订阅推送，在`componentWillUnmount`中取消订阅。到了这我们就可以放心的去重构`List`和`Search`中的代码了

`List`组件重构后

```javascript
class List extends Component {
  state = {
    users: [], 			// 初始化状态，users 是数组
    isFirst: true, 		// 是否为第一次打开
    isLoading: false, 	// 标识是否处于加载中
    err: '', 			// 存储请求相关错误信息
  }

  // 组件被挂载时订阅 
  componentDidMount() {
    this.token = PubSub.subscribe('MY TOPIC', (msg, data) => {
      this.setState(data)
    })
  }

  // 组件被卸载时取消订阅
  componentWillUnmount() {
    PubSub.unsubscribe(this.token)
  }

  render() {
    const {users, isFirst, isLoading, err} = this.state
    return (
      <div className="row">
        {
          isFirst ? <h2>欢迎输入，输入关键词搜索</h2> :
            isLoading ? <h2>Loading</h2> :
              err ? <h2 style={{color: 'red'}}>{err}</h2>:
          users.map((userObj) => {
            return(
             // 页面内容
            )
          })
        }
      </div>
    );
  }
}
```

`Search`组件重构后

```javascript
class Search extends Component {

  search = () => {
    
    // 获取输入（连续解构赋值+重命名
    const {keywordElement: {value: keyword}} = this
    // 推送一个名为 MY TOPIC，内容是一个对象
    PubSub.publish('MY TOPIC', {isFirst: false, isLoading: false})
    // 发送网络请求
    axios.get(`http://localhost:3000/api1/search/users?q=${keyword}`).then(
      response => {
        // 发送请求成功通知 List 更新 state
        // 推送一个名为 MY TOPIC，内容是一个对象
        PubSub.publish('MY TOPIC', {isLoading: false, users: response.data.items})
      },
      error => {
        // 发送请求失败通知 List 更新 state
        // 推送一个名为 MY TOPIC，内容是一个对象
        PubSub.publish('MY TOPIC', {isLoading: false, err: error.message})
      }
    )
  }

  render() {
    return (
        // 页面内容
    );
  }
}
```

经过测试重构后的代码的功能和原来的代码保持一致但是代码和逻辑都更清楚了。

