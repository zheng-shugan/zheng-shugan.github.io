# Vue 集成 Vitest


之前看 CS61B 的时候看到里面的第一个例子就是用 JUnit 来作为检查数据结构和算法有没有写错的验证程序，后来学了 Java 之后才知道 JUnit 是一个单元测试包，专门用来测试一些函数是否能得到预期的结果。但在学习 Web 前端的时候就一直没有看到过有教程或者是学习路线中有提到前端开发的单元测试要怎么写。
### 为什么不能没有单元测试
单元测试的目的很简单，就是为了验证代码能不能按照编写的预期工作的，如果在写完一个函数就进行单元测试并通过就能更快的将问题排查出来，而不是等到项目运行的时候才去排错。
插一句不太相关的话，在平时的项目用 TypeScript 来替换 JavaScript 的一个很大原因就是希望将一些错误留在编译期间而不是项目运行时。
### 为什么会是 Vitest
很简单，因为 vitest 的核心成员有 vue 和 vite 的核心成员，所以 vite 项目里集成 vitest 只要把 `vite.config.ts` 改成 `vitest.config.ts` 就可以将 vitest 引入已有的 vite 工程当中。
### 准备工作
#### 安装依赖
要在 vue 项目中加入 vitest 先要下载依赖：
```
pnpm add @vue/test-utils jsdom vitest -D
```
`@vue/test-utils` 是官方的测试工具库，`jsdom` 是拿去模拟 DOM 的，vitest 作为单元测试。
#### 修改配置文件
vitest 是原生的 vite 框架集成起来是非常容易的，只需要替换一个配置就可以完成在 vitest 的集成。
```ts
/* 将 vite 的 defineConfig 换成 vitest/config 的 defineConfig */
import { defineConfig } from 'vitest/config'

// https://vitejs.dev/config/  
export default defineConfig({  
  // 其他配置
  test: {  
    environment: 'jsdom', // 指定虚拟 DOM 环境
  },  
})
```
#### 要测试什么？
在测试之前我们需要知道希望某个组件预期会有什么效果。我准备的测试需要检查以下内容
- Hello 组件根据 DOM 渲染出一对 `<h1></h1>` 标签
- 标签内部是否有文本 `Hello Vitest`

Hello 组件
```vue
<script lang='ts' setup>  <script lang='ts' setup>  
const props = withDefaults(defineProps<{ msg: string }>(), {  
  msg: 'Hello Vue',  
})  
</script>  
  
<template>  
  <h1>{{ msg }}</h1>  
</template>  
  ```
### 开始测试！
#### 创建测试文件
现创建一个 `__test__` 的文件夹专门用于存放测试文件，然后在此文件夹里创建一个 `Hello.test.ts` 的测试文件。
#### 创建一个测试
可以通过引入 vitest 里的 `descript` 函数来创建一个测试组，它接收一个字符串作为测试组的名字和一个函数进行测试。
```typescript
describe('Hello.vue', () => {
  // 需要测试的代码
})

```
在 `descript` 的回调函数中我们可以使用 vitest 的 `test` 来创建一个测试函数，同样的它也接受一个字符串作为测试的名字和一个回调函数。
```ts
describe('Hello.vue', () => {
  test('mount component', () => {
    // 需要测试的代码
  })
})

```
但在开始测试之前我们必须拿到需要测试的组件，我们可以通过 @vue/test-utils 里的 `mount` 函数做到这一点，它接收一个 Vue 组件和一些可选配置
```ts
/**  
 * 创建一个 wrapper 包含需要渲染的组件
 * @param Component 需要测试的组件  
 * @param options   可选项配置  
 */
function mount(Component, options?: MountingOptions): VueWrapper

interface MountingOptions<Props, Data = {}> {
  attachTo?: HTMLElement | string
  attrs?: Record<string, unknown>
  data?: () => {} extends Data ? any : Data extends object ? Partial<Data> : any
  props?: (RawProps & Props) | ({} extends Props ? null : never)
  slots?: { [key: string]: Slot } & { default?: Slot }
  global?: GlobalMountOptions
  shallow?: boolean
}
```
我们可以在 `test` 中拿到这个需要的组件
```ts
describe('Hello.vue', () => {describe('Hello.vue', () => {
  test('mount component', async () => {
	// 通过 mount 拿到一个包含 Hello 组件的 wrapper
    const wrapper = mount(Hello, {
      // 传入 Hello 组件的 props
      props: { 
        msg: 'Hello Vitest',
      },
    })

  })
})

```
#### 验证程序
到目前为止我们已经创建好测试文件和拿到需要渲染的组件，接下来就是验证组件有没有按我们预期进行渲染。
现在就可以使用 vitest 中的 `except` 来验证组件的某些部分是否和我们预期的相同。现在 Hello 组件应该是只会显示标题 1 且文本为 `Hello Vitest`，所以我们可以这样做：
```ts
const expectMsg = 'Hello Vitest'

// wrapper.find() 来查找组件中的一个 HTML 标签
// wrapper.text() 返回一个元素的文本
expect(wrapper.find('h1').text()).toContain(msg)
```
到这一步我们的测试文件就写完啦，可以在终端里运行
```bash
pnpm run test
```
这样就完成了在 vue 中集成 vitest。
#### 完整测试程序
```ts
import { describe, expect, test } from 'vitest'  
import { mount } from '@vue/test-utils'  
import Hello from '@/components/Hello.vue'  
  
describe('Hello.vue', () => {  
  test('mount component', async () => {  
    const msg = 'Hello Vue & Vitest'  
  
    const wrapper = mount(Hello, {  
      props: {  
        msg: 'Hello Vue & Vitest',  
      },  
    })  
  
    expect(wrapper.find('h1').text()).toContain(msg)  
  })  
})
```

### Useful Link
- [Vitest | 由 Vite 提供支持的极速单元测试框架](https://cn.vitest.dev/)
- [Home | Vue Test Utils (vuejs.org)](https://test-utils.vuejs.org/)
- [测试代码]( https://github.com/zheng-shugan/vue-starter )
