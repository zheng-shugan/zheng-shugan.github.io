# 我听说新 Mac 是造梦引擎？


# MacBook Pro

我自己拥有的第一台电脑苹果第一台搭载自研桌面级处理的 MacBook，2020 年它带着光环而来。搭载全新的 Apple Silicon、10 小时续航、统一内存、超低功耗、Rosetta2、桌面级处理器最先进的制程。其他一切都是原来的样子。加上了 T2 芯片的主板，意味着这台电脑从第一次开机的时候所有的数据都是加密过的，数据丢失就不可恢复。统一内存架构将内存集成到 SoC 上的做法彻底关闭了 Mac mini 升级硬件的大门至于 MacBook 已经习惯它不能自行升级任何配置了。所以只能买购买的时候花天价升级配置。我的MacBook 只把内存升级到了 16G 就多花了 ¥1500。

自 20 年 12 月以来我已经用了这台 MacBook 一年半的时间了。当我的室友在打王者荣耀的时候我用它学习 C++；当我的室友在考前突击的时候我用它搭建了个人网站；我在这台电脑上写下了第一篇博客，创建了第一个 steam 账号。

### Enter the macOS

不管是 macOS 还是 Windows 官网都提供相应的使用手册，虽然里面的完成一些功能的步骤是有些复杂的但是对于初次上手的新手真的是非常友好了，有了这个就可以不用到什么论坛、微博、知乎去求助。可以说看完这个再结合自己的操作绝对比大部分的网友更懂 macOS / Windows。在初次上手时很多操作我就参考了《macOS 使用手册》。

[《macOS 使用手册》](https://support.apple.com/zh-cn/guide/mac-help/welcome/mac)

[《Windows 帮助和学习》](https://support.microsoft.com/zh-cn/windows?ui=zh-CN&rs=zh-CN&ad=CN)

### 硬件完成度

从 MacBook Pro 2016 到 MacBook Pro 2020 的外观几乎是没有变化，没有变化说明现在的模具稳定性没有什么大问题了但这个模具真的是用了有点久了。几乎一致的模具的内部藏着的就是本次最大的升级：

- Apple Silicon on Mac
- 统一内存架构
- 第一颗 5nm 桌面级处理器
- MacBook 最强的集成显卡
- 雷电4 / USB 4
- 一天办公需求的续航
- 十分冷静的核心

大部分的场景下可以把搭载 M 系列的 Mac 当作之前搭载 Intel 的 Mac 一般使用只是发热低了许多。M1 没有因为功耗低在性能上做出太多的妥协，在你需要性能的时候它也不必之前的 MacBook 弱；更强的能耗比表现使其续航可以满足一天的办公需求。M1 芯片使得 MacBook 在市场成为一个独特的存在也走出了自己的一条路。需要注意的是 M1 独特的硬件架构**不是适合所有人**，在买所有 Mac 之前一定要清楚自己拿电脑是来做些什么的，一些人用上 M1 或许能让生产力提高；一些人或许面对的就是需要的软件在这个平台上根本没有。也不是所有的人都用的惯 macOS。

用了这么久的 M1，发现它的性能对于我来说现阶段是发挥不出它全部的实力除了觉得内存有点不太够用（选 Mac 时有预算尽量还是加在内存上；如果是定点办公闪存用靠雷电接口外接一点问题都没有）其他方面真的是非常够了。

今天看到了 M1 MAX 胶水拼接成了一块 M1 Ultra，在 2020 年我绝对想不到 [M1 系列](https://www.apple.com.cn/mac-studio/)能如此的残暴：初次登场轻薄本中无敌的能耗比；Pro 、 Max 大幅度升级 GPU；最后的 Ultra 直接 Fusion。M1 都到 Ultra 了都没登陆到 Mac Pro 上或许下一代还有定位更高的？可能吧，让我们一起期待下一世代的 Apple Silicon。

### 软件完成度

有自己的操作系统就是舒服，早在 2005 苹果就推出了 Universal Binary 和 Rosetta：为了从 PowerPC 过渡到 Intel 做准备。当时从 ARM -> X86 的经验也让这次 X86 -> ARM 变的更加的顺利，最明显的一点就是在搭载 M1 的 Mac 一发布就有不上第三方的软件完成了原生的适配。不仅如此，在 App Store 里还可以下载 iOS/iPadOS 的软件了虽然不是所有软件都支持但至少有了可以做到的能力，让 macOS 的生态和移动端更加紧密。~~（说句题外话：也希望能在未来的 WWDC 上看到 iPadOS 中能有更多关于内容生产的部分吧，去年的 iPad 的 playgrounds 已经可以写 swiftUI 的软件就是一个很好的开始。）~~

说完了为了 Apple Silicon 做出的适配再说说 macOS 本身。macOS 本就是一个 Unix 系统，自带的终端就默认的`shell`就是`zsh`。 在 Mac 上的 [homebrew](https://brew.sh)，可以直接通过终端下载软件和环境如果缺少对应的依赖也会自动下载好。比如这个博客的框架就是用 homebrew 下载的：`brew install hugo`，一行命令即可。

再来就是 macOS 中安装 Window。曾经我也对这个充满疑问：为什么在 macOS 里还要安装 Windows？？？后来才知道是我年轻了 ------ 一些软件在 macOS 里根本没有。

如果你是大学生在不知道自己未来的学习需要用到什么软件的时候最好还是选择购买一台 Windows 的电脑，因为无法保证每个人都买得起 Parallels Desktop 虚拟机安装 Windows，买得起 Parallels Desktop 也不能保证需要的软件能运行在 arm 架构上或者说是流畅度可用。虽然的 M1 的 Mac 能耗比的优势巨大但是之前的 Intel Mac 还是有优势在的如果你需要那样的生产环境 Intel 的 Mac 还是很值得购买。

除却 Rosetta2 和 Universal Binary 之外 macOS 也从原先的拟物化设置变为了和 iOS 一样的扁平中带些拟物的化设计，比起之前更像移动端的设计风格了也让我这种 macOS 小白在 2020 年第一次使用  macOS 都没有感到一丝的不适应，至少不会出现不知道在哪显示桌面图标......

不过当然了该有的 bug 一个都不会少：中文输入法卡顿、Safari 闪退等等问题（具体的问题根据使用场景的不同也都不一样）。如果安装了 beta 版的操作系统会更加的糟糕，不过没事**别升级 beta 版系统**。

### 体验和感受

用了这么久的电脑早就习惯了它的操作逻辑也已经没有了刚拿到上手的新奇劲了，现在就把它当作一个工具使用，工具嘛自己用的顺手和习惯就好。电脑买来总是拿去干活的，如果它能让你工作流的效率提升那就买吧，多赚的钱都购买好几台了电脑了。我现在希望的是它不要出问题：苹果的维修价格实在是太感人了。

我听说新 Mac 是造梦引擎，可我手机的这台已经是了呀！
