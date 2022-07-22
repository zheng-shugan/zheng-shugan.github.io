# Mock 数据


### 从前后端分离开发说起

都说前后端开发要分离，而前后端分离的重点就是**前后端独立可运行**，那在后端接口没有返回数据的情况下来完成前端数据的获取呢？答案就是使用 Mock 数据来模拟预先与后端约定好的接口数据，来作为开发时的测试数据。

### Mock 数据

#### 什么是 Mock 数据

Mock.js 是一款模拟数据生成器，可以生成模拟数据来让前端开发独立于后端进行开发，帮助编写单元测试。提供了以下模拟功能：

- 根据模版生产模拟数据
- 模拟 Ajax 请求，生成并返回模拟数据
- 基于 HTML 模版生成模拟数据

#### 使用 Mock 数据

安装

```bash
yarn add mockjs
```

使用

```javascript
import Mock from 'mockjs'

const Dashboard = Mock.mock({
  'sales|8': [
    {
      'name|+1': 2008,
      'Clothes|200-500': 1,
      'Food|180-400': 1,
      'Electronics|300-550': 1,
    },
  ],
  cpu: {
    'usage|50-600': 1,
    space: 825,
    'cpu|40-90': 1,
    'data|20': [
      {
        'cpu|20-80': 1,
      },
    ],
  },
  ...
})
```



[这个项目](https://github.com/zheng-shugan/antd-admin)所有的数据都是由 [Mock.js ](http://mockjs.com/) 生成的，在 Umi 项目中，`/mock` 文件夹里的所有文件都被视为 Mock 数据文件。

在 `/mock` 里的文件，最后都有一行 `module.exports = { }`

`/mock/dashboard.js`

```js
module.exports = {
  [`GET ${ApiPrefix}/dashboard`](req, res) {
    res.json(Dashboard)
  },
}
```

这段代码的意思是生成了一个 GET 方法，地址为 `localhost:port/APIPrefix/dashboard` 的后端接口，使用 Postman 等工具也可以测试的出来返回结果。

同样的也可以使用 *ESM* 来导出：

```js
export default {
  [`GET ${ApiPrefix}/dashboard`](req, res) {
    res.json(Dashboard)
  }
}
```

Mock 数据生成规则详情见 [Mock.js wiki](https://github.com/nuysoft/Mock/wiki)

