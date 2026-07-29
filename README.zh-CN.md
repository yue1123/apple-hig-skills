# Apple HIG for React Native

[English](README.md) · **简体中文**

一个 agent skill，把 Apple 的 Human Interface Guidelines 完整 172 页蒸馏成 33 个参考文件交给 Claude，每一条 Apple 规则旁边都配着实现它的 React Native 代码。

模型对 React Native 很熟，对 Apple 则含糊。让它写一个设置页，你会拿到一个能跑、能渲染、但读起来悄悄像 Android 的东西：固定的行高、硬编码的 `#F2F2F7`、手搓的 header、一个 Material 风格的 chevron。这里每一处都是 HIG 明确规定过的数值，而模型只是近似了它。

这个 skill 就是把「近似」去掉。**永远不要猜任何数值** —— 命中区、类型标尺、安全区内边距、对比度、弹簧动画行为，全都在里面，旁边挨着 RN 的实现映射，每个文件末尾还有一份审查清单。

## 安装

克隆或下载本仓库，然后：

```bash
cp -r apple-hig ~/.claude/skills/
```

如果只想给单个项目用而非全局安装，把它复制到项目内的 `.claude/skills/` 即可。

在 Claude.ai 或 Claude Desktop 上，到 Settings → Capabilities → Skills 上传 [`apple-hig.skill`](apple-hig.skill)。

装好之后 Claude 会自己加载它 —— skill 的 description 覆盖了页面、导航、sheet、排版、颜色、深色模式、Liquid Glass、触感反馈、安全区、Dynamic Type、VoiceOver、widget 以及 App Store 要求，所以你不需要点名调用。问一句「这个够不够原生」就足够触发。

## 为什么要用

### 问题出在精度，不在品味

一个没读过 HIG 的模型不会产出难看的 UI —— 它产出的是在十五个地方各差几个点的 UI。iOS 正文是 17 pt，不是 16。行要用 `minHeight` 而不是 `height`，否则在大号 Dynamic Type 下文字会被截断。tvOS 正文是 29 pt **Medium**、下限 23 pt，所以把 iOS 的类型标尺搬到 Apple TV 上，坐沙发上根本看不清。Liquid Glass 没法用 `rgba()` 伪造，因为它会跟随亮度自适应并绘制镜面高光；而给原生 tab bar 设 `backgroundColor` 会直接覆盖掉整个材质。

这些都不是主观判断，全都有文档，也全都在这里面。

### 它确实改变了答案，而且可测量

取 [`evals.json`](apple-hig/evals/evals.json) 中的 5 个任务，各由两个完全相同的 agent 回答一遍 —— 一个带 skill，一个既不能访问 skill 也不能联网搜索。同一模型、同一 prompt、同一篇幅预算。评分依据每个 eval 的 `expected_output`，拆成可核验的评分点：

| 任务 | 带 skill | baseline |
|---|---|---|
| 把 Android 设计的设置页移植到 iOS | **9.0** / 9 | 5.0 / 9 |
| 把 iOS 媒体应用发布到 Apple TV | **9.0** / 9 | 5.0 / 9 |
| 呈现带未保存修改的编辑资料 sheet | **6.5** / 7 | 4.0 / 7 |
| 让 tab bar 用上 iOS 26 玻璃材质 | **6.0** / 8 | 5.0 / 8 |
| 审查组件的 App Store 与无障碍风险 | **9.0** / 9 | 8.5 / 9 |
| **总计** | **39.5 / 42 · 94%** | 27.5 / 42 · 65% |

差距并不均匀，而这恰恰是有价值的部分。它在训练数据稀薄的平台专属知识上最大，在通用无障碍审查上几乎归零 —— 对比度和缺失 label 这类问题本来就众所周知。baseline 答错的都是具体细节：把 `minHeight` 写成 `height: 44`、把系统色板当字面量硬编码、该用 action sheet 的地方用了 alert、声称 Expo 支持 tvOS、漏掉 Reduce Transparency 的回退。

每个任务只跑了一次，且评分非盲 —— 请把任务间的排序当作结论，而不是把具体数字当作结论。

## 参考文件

从 [`SKILL.md`](apple-hig/SKILL.md) 开始 —— 路由表、关键数值和通用审查清单都在那里。下面这些就是它路由到的文件。

**Foundations** —— 跨领域的通用规则

- [color-and-materials.md](apple-hig/references/foundations/color-and-materials.md) —— 语义色、深色模式、模糊、Liquid Glass
- [typography.md](apple-hig/references/foundations/typography.md) —— 类型标尺、Dynamic Type、UI 文案、空状态
- [layout.md](apple-hig/references/foundations/layout.md) —— 页面结构、安全区、旋转、断点、RTL
- [accessibility.md](apple-hig/references/foundations/accessibility.md) —— VoiceOver、命中区、对比度、Reduce Motion
- [motion.md](apple-hig/references/foundations/motion.md) —— 动画、转场、手势驱动的交互
- [icons-and-images.md](apple-hig/references/foundations/icons-and-images.md) —— SF Symbols、资源导出、应用图标
- [privacy.md](apple-hig/references/foundations/privacy.md) —— 权限、凭据、登录

**Patterns** —— 流程应有的行为

- [modality-and-multitasking.md](apple-hig/references/patterns/modality-and-multitasking.md) —— sheet、模态、进入后台、音频中断
- [feedback-and-loading.md](apple-hig/references/patterns/feedback-and-loading.md) —— 异步状态、错误、撤销、破坏性操作
- [launching-and-onboarding.md](apple-hig/references/patterns/launching-and-onboarding.md) —— 启动屏、首次运行、内置提示、状态恢复
- [data-entry-search-settings.md](apple-hig/references/patterns/data-entry-search-settings.md) —— 表单、搜索、设置、账号注销
- [media-and-haptics.md](apple-hig/references/patterns/media-and-haptics.md) —— 音频、视频、触感反馈
- [notifications-sharing-files.md](apple-hig/references/patterns/notifications-sharing-files.md) —— 推送、分享、拖放、文档

**Components** —— 具体控件

- [menus-and-actions.md](apple-hig/references/components/menus-and-actions.md) —— 按钮、工具栏、菜单、分享面板
- [navigation-and-search.md](apple-hig/references/components/navigation-and-search.md) —— tab bar、侧边栏、搜索框
- [presentation.md](apple-hig/references/components/presentation.md) —— sheet、alert、action sheet、popover
- [selection-and-input.md](apple-hig/references/components/selection-and-input.md) —— 文本框、开关、选择器、滑块、键盘
- [layout-and-organization.md](apple-hig/references/components/layout-and-organization.md) —— 列表、表格、网格、分栏视图、展开折叠
- [content-and-charts.md](apple-hig/references/components/content-and-charts.md) —— 图表、图片视图、文本视图、网页视图
- [status.md](apple-hig/references/components/status.md) —— 进度、仪表、活动圆环、评分
- [system-experiences.md](apple-hig/references/components/system-experiences.md) —— widget、实时活动、控件、App Shortcuts

**Platforms** —— 只读你要发布的那几个

- [ios.md](apple-hig/references/platforms/ios.md) —— RN 一等支持
- [ipados.md](apple-hig/references/platforms/ipados.md) —— 同一构建目标；布局、输入、尺寸变化
- [macos.md](apple-hig/references/platforms/macos.md) —— `react-native-macos` 或 Mac Catalyst
- [tvos.md](apple-hig/references/platforms/tvos.md) —— `react-native-tvos`
- [visionos.md](apple-hig/references/platforms/visionos.md) —— `react-native-visionos`，仅窗口化 UI
- [watchos.md](apple-hig/references/platforms/watchos.md) —— RN 无路可走；读它是为了那些会波及 iOS 应用的规则

**React Native 实现**

- [design-tokens.md](apple-hig/references/rn/design-tokens.md) —— 可直接复制的 token 文件
- [liquid-glass.md](apple-hig/references/rn/liquid-glass.md) —— 玻璃表面、滚动边缘效果
- [platform-strategy.md](apple-hig/references/rn/platform-strategy.md) —— 项目结构、库选型、该支持哪些平台

**输入与技术**

- [inputs.md](apple-hig/references/inputs.md) —— 手势、键盘快捷键、悬停、焦点、Pencil、传感器
- [identity-and-commerce.md](apple-hig/references/technologies/identity-and-commerce.md) —— Sign in with Apple、Apple Pay、内购、Wallet
- [system-integration.md](apple-hig/references/technologies/system-integration.md) —— Siri 与 App Intents、地图、SharePlay、iCloud、App Clips

## 你会查得最多的几个数

| 平台 | 命中区 | 正文 | 最小字号 | Dynamic Type |
|---|---|---|---|---|
| iOS、iPadOS | **44 × 44 pt** | 17 pt | 11 pt | 最大约 3.1× |
| macOS | 28 × 28 pt | 13 pt | 10 pt | 无 |
| tvOS | 66 × 66 pt | 29 pt **Medium** | 23 pt | 有 |
| visionOS | **60 × 60 pt** | 17 pt | 12 pt | 有 |
| watchOS | 44 × 44 pt | 16 pt | 12 pt | 有，另加 AX1–AX3 |

对比度：17 pt 及以下文字 **4.5:1**，18 pt 以上或加粗 **3:1**，自定义小字目标 **7:1** —— 浅色和深色两种外观都要验证。

tvOS 安全区：上下 **60 pt**，左右 **80 pt**。React Native 不会替你加上。

## 来源与时效

蒸馏自 [Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines) 全部 172 页，对应站点 2025 年 12 月的修订版 —— 已包含 Liquid Glass 与 iOS 26 时代的指引。

skill 对自身内容声明了两点：具体色值和弹簧参数会漂移，所以文中的 hex 值只是设计期参考，运行时应通过 `PlatformColor` 解析；RN 库的推荐比 HIG 本身过时得更快，但「优先选择包装真实 UIKit 组件的包，而不是 JS 重新实现」这条原则会比任何具体包名活得更久。

## 参与改进

[evals](apple-hig/evals/evals.json) 是验证一处改动是否有效的最快途径。加一个带 `expected_output` 的任务，分别在带 skill 和不带 skill 的情况下跑一遍，看你改的那个参考文件是否真的被读到并被应用 —— 上面那次 A/B 就是这样暴露出两处内容缺口的，其中一个是参考文件从未写明的 tvOS tab bar 细节。
