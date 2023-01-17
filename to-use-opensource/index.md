# 如何入手一个前端开源项目


- 如何入手一个开源项目
  - 克隆到本地
  - 安装依赖
    - pnpm
    - yarn
    - npm
  - 看 package.json
    - 主要看 script 和 dependencies 部分
  - 从 App 组件入手，逐级往下看

## 为什么要找开源项目

很多人在学完基础想要找项目的时候就不知道怎么找，有些是因为之前的视频看的都是培训机构的视频，如果当前这个培训机构的项目实战视频又有点老了或者是一些后端接口不能用的情况，当然这不是他们的问题，机构也需要为自己的发展做打算，一般能发出来作为视频教程的都是已经被机构内部淘汰出来的项目其他的基础教程也是同理。所以想把新技术都用上就只有去实现自己的一个 idea 或者是在 Github 上找到适合自己的项目。

我那时候就是这样的，看到机构的项目实战视频都有点老了，就自己去 Github 上找开源项目，找啊找终于是找到是纯前端的项目了。可就是咋样的都看不懂代码，于是我就从 `components` 文件夹开始抄，`package.json` 文件我就直接复制然后下载依赖，做完发现不对啊，我还是看不懂啊，怎么会这样，明明是把项目都抄了一遍，效果是出来了，但我咋还是不明白呢？

能写下这篇文章就说明我已经是踩了很多坑之后，才明白的要怎么样从 0 开始入手一个前端的开源项目。这就来分享一下我在自学前端的时候遇到的坑～

## 分析过程

一般在 Github 上 star 数比较多的项目都会有一个比较详细的 README 文档，一般包括以下几点：

1. 项目介绍
2. 项目特点
3. 在本地如何开发
4. 项目完成/没完成哪些功能
5. 开源协议

这篇文章着重介绍如何在本地开发。

### 在本地启动项目

首先入手项目的第一步肯定是想把项目下载到本地，一般都是

先把项目克隆到本地

    git clone xxx example-project

然后紧接着就是安装依赖

    cd example-project
    npm/yarn/pnpm install

最后再根据 `package.json` 里的命令把项目在本地给跑起来

    pnpm serve/start/dev

这样就跑通了一个项目，安装依赖方面多说一句有时候用 `pnpm` 安装好的依赖可能跑不通，就换 `yarn/npm` 试一下。

### 目录分析

在本地开发最重要的就是要看懂那些代码都做了什么，一般的前端项目都会有一个 `App` 组件在 Vue 项目里是 `App.vue`，React 则是 `App.txs`，入口文件则都是 `main.ts`。

在这我的建议是从上到下去分析目录，先看 `package.json` 了解一下项目大概都用了哪些依赖以及启动/打包项目的命令是什么，然后再去 `main.ts` 里看都引入了哪些组件和工具函数，最后就一步步的从 `App` 开始往下看。

有和后端联调的项目就去看看它都发了哪些请求、是怎么封装 axios 请求的，也可以用 postman/apifox 这样的工具去测试一下一些接口是返回什么数据的。

**这个时候千万不要着急去动手写**，看别人怎么写的代码是非常重要的，花个三五天甚至一周时间看代码看一遍都无妨。优质一点的项目代码量基本都不小，直接上手开始模仿人家的代码可能会直接晕掉。

### 开始动手

在做了以上两步再结合项目介绍，对项目也已经有了一个更加清楚的认知了，这个时候就可以根据自己的需求是在原项目上添加一些功能还是说自己在学习的过程中发现了一些 bug 也可以试着去 fork 这个仓库去提 issues、去解决它。

在学习的过程中，最忌讳直接去抄代码的，这样做一遍下来可以说是毫无效果的，先思考这个页面做到这样的效果的，自己试试看能不能做一个最小实现的 demo。这样渐进的去学习做一个项目才是能收获到最多的。

很多东西是在看项目的时候是不知道的，这是很正常的。遇到没有用过的第三方包就去搜：npm + 包名，一些组件不知道怎么用就去查文档，英文文档看不懂？那就一边开翻译一边原文对着看啦。等自己的项目做完了好好的写一个 README 就算是完成了这个项目。

## 总结

软件开发是个正向激励很快的工作，我的水平很次，但我很享受看到自己写的项目成果呈现在眼中去向朋友介绍我做了一个什么什么的网站，实现了什么什么样的功能，正是这些激励着我继续去做这件事。

