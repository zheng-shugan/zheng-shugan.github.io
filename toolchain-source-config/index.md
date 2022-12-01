# Web 工具链换源


### 配置的烦恼

每次用个新的工具链都要先看一下怎么换到国内源，要不然那个下载速度实在是太感人了，所以就打算收集一下 Web 前后端开发需要换源的工具链。
在这也感谢各机构备份镜像源，如果没有你们也就不会有这篇记录。

### 系统设置

#### Homebrew
Homebrew 是 macOS 下的一个包管理工具，可以很方便的去安装、管理各种开发环境和软件，如果你用的也是 macOS 作为开发平台 homebrew一定不可错过的一个好工具。

查看当前源：

```zsh
cd "$(brew --repo)" && git remote -v
```
替换为清华源：
```zsh
git -C "$(brew --repo)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git 
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git

echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.zshrc
source .zshrc
```
### 前端
#### Node
正式因为 Node 的出现前端才越来越组件化、工程化，Node.js 的发展也证明它不仅可以在前端发光发热，在后端领域也是有着不少合适的领域。
npm、yarn、pnpm 都是前端的包管理工具，个人更偏爱 pnpm 一点但是 yarn 在键盘上的位置更好按哈哈哈哈
npm
```zsh 
# 查看当前源
npm get registry
# 设置 npm 镜像源为淘宝镜像
npm config set registry https://registry.npm.taobao.org/  
```
yarn
```zsh
# 查看当前源
yarn get registry
# 设置 yarn 镜像源为淘宝镜像
yarn config set registry https://registry.npm.taobao.org/
```
pnpm
```zsh
# 查看当前源
pnpm get registry
# 设置 pnpm 镜像源为淘宝镜像
pnpm config set registry https://registry.npm.taobao.org/
```

