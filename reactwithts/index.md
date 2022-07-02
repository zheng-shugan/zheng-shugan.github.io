# React With TS




## 为什么要使用 TypeScript？

JS（JavaScript）是一门功能强大的语言，但是作为一门动态类型语言有时候简直是在浪费 IDE 强大的补全功能，就因为它是弱类型的语言，IDE 不能准确的推断出对象的类型，从而进行准确的提示。这一点对 VSCode 这种文本编辑器影响愈大。

TS（TypeScript）作为 JS 的一个超集，就和它的名字一样，为 JS 添加了 **Type**。作为一门静态语言，毫无疑问对 IDE 的语法补全有出色的帮助。别的优点不提，在 IDE 中的补全更强就是使用 TS 的一大理由，而且类型本身就是一种注释，哪怕需要添加更多的代码。

## 在 React 中使用 TS

新建一个 React app 最简单的方式当然是使用 CRA 的 TS 模版了。

```bash
npx create-react-app my-ts-app --template typescript
```

打开项目，目录结构为：

```bash
├── README.md
├── node_modules
├── package-lock.json
├── package.json
├── public
├── src
└── tsconfig.json
```

打开 `/src` 会发现 原来的 `.jsx` 文件的都变成了 `.tsx`。

### 创建一个 TS 组件

```tsx
import { useState, useEffect } from "react";

// Type with props
export interface HelloProps {
	name: string;
	enthusiasmLevel?: number;
}

const Hello = ({ name, enthusiasmLevel = 1 }: HelloProps) => {
	const getExclamationMarks = (numChars: number): string => {
		return Array(numChars + 1).join('!')
	}

	// Hooks with TS
  	// 将 message 的类型设置为 string
	const [message, setMessage] = useState<string>('')

	useEffect(() => {
		setMessage(`Hello ${name}` + getExclamationMarks(enthusiasmLevel))
	}, [enthusiasmLevel, name])

	return (
		<div>
			<h1>{ message }</h1>
		</div>
	);
};

export default Hello;
```

注意到我们定义了一个 `HelloProps` 类型，它指定了这个组件需要用到的属性。在接受参数的时候我们为参数加上类型 `HelloProps`，一旦定义了类型，那么父组件就一定需要传递类型中的属性给子组件，否则将会报错。这样我们在 `Hello` 组件里接受到的参数就都是有类型的了。

同样的我们也可以选用类式组件

```tsx
// src/components/Hi.tsx
// 只是组件类型和 Hello.tsx 不同，其他都是一样的
import React from 'react'

export interface HiProps {
	name: string
	enthusiasmLevel?: number
}

// React.Component<HiProps, object> 相当于 React 的泛型
// 这里的 HiProps 是 this.props 类型
class Hi extends React.Component<HiProps, object> {
	render() {
		const { name, enthusiasmLevel = 1 } = this.props

		if (enthusiasmLevel <= 0) {
			throw new Error('You could be a little more enthusiastic. :D')
		}

		const getExclamationMarks = (numChars: number): string => {
			return Array(numChars + 1).join('!')
		}

		return <h1>Hello {name + getExclamationMarks(enthusiasmLevel)}</h1>
	}
}

export default Hi
```

然后在 `./src/app.tsx` 里把 Hello 组件挂载上就可以了。

```tsx
import './App.css'
import Hello from './components/Hello'
import Hi from './components/Hi'

const App = () => {
	const name: string = 'React.js'

	return (
		<div className='App'>
			<header className='App-header'>
				<Hello name={name} />
				<Hi name='TypeScript' />
			</header>
		</div>
	)
}

export default App
```

这样就算是完成了使用 TS 来建立 React 组件啦。

