# Makepad-Rust - Getting Started

**Pages:** 53

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/start/introduction

**Contents:**
- #介绍
- #Makepad Book 介绍
- #Makepad 介绍
- #Makepad 本身的优势
  - #性能至关重要
  - #更好的工作流程带来更好的产品
- #Makepad 采用的技术栈的优劣
  - #Rust
  - #使用着色器来渲染 UI 样式
  - #混合即时模式

官网地址：https://makepad.nl/

仓库地址：https://github.com/makepad/makepad

Makepad 之书，面向 Makepad 应用开发者和贡献者，系统地介绍了 Rust 跨平台 Makepad UI 框架的方方面面。

Makepad 是一个用 Rust 编写的开源跨平台 UI 框架，并配有功能强大的创作应用程序 Makepad Studio。这种组合为用户界面开发提供了一种简化的方法，使开发人员和设计人员能够轻松创建现代、高性能的应用程序。

Makepad 是使用 Rust 构建的，Rust 是一种现代、安全的编程语言。它可在本机和网络上运行，支持包括 Windows、Linux、macOS、iOS 和 Android 在内的所有主流平台。

Makepad 以其超快的编译速度著称，可确保高效、无中断的开发体验。

Makepad 用户界面基于着色器。这确保了它的高性能，使其适用于构建复杂的应用程序，如类似于 Photoshop 的应用程序，也适用于 2.5D UI，如 VR/AR 应用程序中的 2.5D UI。

与 HTML 相比，该框架通过使用即时模式和简单的 DSL 简化了样式，无需复杂的 DOM 或 CSS。它还提供对 GPU 的直接访问，提供更多控制和灵活性，同时避免了与垃圾回收相关的问题，确保更流畅的性能。

Makepad 最突出的功能之一是实时样式化，它允许在代码中立即反映用户界面的变化，反之亦然，无需重新编译或重启。这一功能大大缩小了开发人员与设计人员之间的差距，提高了整体工作效率。 Makepad Studio 将这一切与自己的集成开发环境和类似 Figma 的可视化工具结合在一起，让设计师可以直观地直接在实际产品上工作。

另一个关键组件是 Stitch，它是一个基于 Rust 的实验性 WebAssembly（WASM）解释器。Stitch 以其卓越的速度和轻量级性能而闻名，目前被认为是最快的 WASM 解释器。[待定：在 Makepad 中使用。Superapps，扩展系统，允许在不牺牲性能的情况下插入构件......］

Makepad 诞生于六年前（2019），可以查看 makepad history 仓库了解 makepad 的最初设想。

Makepad 的作者是 Rik Arends。这个名字也许很多人不熟悉，但是你一定听过 Cloud9 IDE 和 ACE 代码编辑器。

Cloud9 IDE： 成立于 2010 年，在 2016 年卖给了亚马逊。它是一种云 IDE，一个基于云的开源集成开发环境。它支持超过40种语言，包括：PHP和Ruby，Python和JavaScript/Node.js。它几乎完全用JavaScript编写，并使用Node.js上后端。

Ace：一个用JavaScript编写的独立的代码编辑器。其目标是创建一个基于Web的代码编辑器，与现有的本地编辑器（如TextMate、Vim或Eclipse）的功能、可用性和性能相匹配并加以扩展。它可以很容易地嵌入到任何网页和JavaScript应用程序中。Ace是作为Cloud9 IDE的主要编辑器和Mozilla Skywriter项目的继承者而开发的

Rik Arends 在创造这两个产品的时候，使用基于 HTML/CSS/Javascript 的 常见 Web 前端技术，他认为使用 HTML 作为 IDE 的 UI 简直是疯了，他深受折磨。比如，他最想做的代码折叠动画，用 HTML 等前端技术做起来非常困难。

基于 Web 前端技术性能也非常慢，所以他就想用 Rust 重新实现一个 Native 的 UI 框架。

下面视频中展示的 demo 是这个愿景的演示。

可以访问 https://github.com/acyanbird/makepad-book-simple 来本地运行此 demo

在线体验（旧版），可以访问 https://makepad.dev/ 此链接展现的并非最新的 makepad studio 版本，您可以从这里下载最新版本。

可以看到视频里：代码、3D树、设计图标都可以做到实时联动效果，视频最后还展示了惊艳的代码折叠动画效果。

为了达到这样的愿景，Makepad提供了两大组成部分：

当开发人员专注于他们的专长时，质量会得到提升。Makepad 的设计编辑器将让设计师们自己将用户界面提升到一个新的水平。

对于也在网络上运行的产品来说，文件大小至关重要。Makepad 项目体积小巧。

编写特定于跨平台的代码既耗时又费力。Makepad 项目在 Web 和原生平台上共享相同的代码。

当开发人员专注于他们的专长时，质量会得到提升。Makepad 的设计编辑器将让设计师们自己将用户界面提升到一个新的水平。

将一个绝妙的想法付诸实践的兴奋感值得被保护。Makepad 无阻碍的开发体验保护了开发者的动力。

快速的编译时间可以保持思维的流畅。Makepad 证明了 Rust 可以令人惊叹地快速编译 —— 而且它的设计系统甚至是即时的。

世界上不存在完美的技术栈，一切都只是权衡，Makepad 也是在这种不断的权衡之下发展的。它也有自己的优势和劣势。

Makepad 还采用了混合即时模式（Hybrid immediate mode，交替使用即时模式和保留模式）。

Makepad 也重新实现了渲染栈（render stack）

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/widget/index

**Contents:**
- #Widget 基础
- #声明式 UI 构建自定义组件
  - #基本组件和主题
  - #定义App组件
  - #Root 组件
  - #渲染机制
  - #Window 组件
  - #增加业务逻辑
- #Action
  - #Button Widget 实现机制

通常，一个应用是由多个组件构成的，这些组件包括了多个窗口、按钮、输入框、标签等控件，或者是由这些控件组成的子组件。

组件（Component），广义来说，是一种封装了特定功能的模块化单元，通常包含逻辑和视图的定义。

组件可以由多个控件（Widget）组成，也可以是其他组件的子组件。它们通常具有独立的生命周期。

Makepad Widget 作为 Makepad 框架的一部分，由 makepad-widgets 库预置了 Makepad 框架的一些基本 UI 控件，如按钮、标签、输入框等。

这些控件旨在简化 UI 的构建和管理，使开发者能够快速创建复杂的 UI 组件。

本章不会对 makepad-widgets 库中的所有控件进行一一讲解（这部分内容将在后续应用实践中逐步展开），而是对其共性和公共特点进行描述，让你对 makepad UI 控件的基本工作机制建立一个整体的心智模型，以便更好地应用它们。

我们先以一个自定义的组件示例开始逐步了解这些内置控件。这个示例来自于 makepad 中的 simple 示例，但是本章会对其略微修改。该示例的最终效果如视频所示：

首先，使用 cargo new simple 来创建一个 crate。

在 Cargo.toml 中引入 makepad-widgets 库的依赖。

然后请在 src 下创建 lib.rs 和 app.rs 文件。此时你的目录结构如下：

然后我们在 app.rs 模块中完善我们的代码。

一个基本的 App 就是一个组件，我们该如何定义它呢？先来定义整个组件结构。

让我们先继续完成这个 App 组件，稍后再深入其背后机制。

Makepad 是声明式 UI 框架，这意味着，只需要声明用户想要的 UI 样式，框架会自动更新 UI 的变化。

makepad 通过在 live_design! 宏内部使用 Live DSL 语言来声明 UI 样式。

这两组 use 导入语句，用于导入基础组件和默认主题。注意，这里 use 关键字是 Live DSL 专用，它一般用于导入其他模块内由 live_design! 定义的Live 脚本。

App 组件在 live_design! 中由 App = {{App}} 这样的语法来定义。这里是创建了一个 App 组件实例，而 {{App}} 里面的 App 则对应 Rust 定义的 App 结构体。

App 结构体有两个字段 ui 和 counter ，其中 ui 是表示 UI 组件，所以用 #[live] 属性标注，而 counter 是非 UI 字段，所以用 #[rust] 属性标注。

在 live_design! DSL 宏中， ui: <Root> { ... }表示 UI 组件使用 Root 根节点。

Root 组件在 Makepad 中扮演着重要的角色，主要负责管理和协调多个子组件。

该组件通过 LiveId 和 WidgetRef 来管理多个 windows ，并且能跟踪当前绘制状态。

Makepad Widget 采用即时模式（immediate mode）和保留模式（retained mode）相结合的混合模式（Hybird）方式，提供了高度灵活性和性能优化。

Makepad 采用混合模式，保留了渲染 UI 树的结构状态，平衡性能和灵活性。遍历所有需要更新的窗口和组件，对于即时模式部分，重新计算和绘制，对于保留模式部分，检查是否需要更新，仅更新变化的部分。

Root 组件就是保留模式和即时模式混合的一个组件。

在 Root 组件内部，我们定义了 main_window 主窗口组件 <Window> 实例。

同样，该 <Window> 组件对应于定义在 widgets/window.rs 中的 Rust 结构体 Window 。

在 main_window 实例中，声明了一些基本属性： show_bg / width / height ，还有一个特别的属性 draw_bg ，该属性特别之处在于以 draw_ 为前缀，可以跟随一个 代码块（block） 来指定着色脚本，这里是指定了 fn pixel(self) -> vec4 函数来渲染背景色。

另外 main_window 实例中还定义了 body 组件实例，它对应的是一个滚动视图组件 <ScrollXYView> ，同样，该 body 实例中定义了该组件的一些基本属性和所包含的组件实例：

至此，我们的 Simple UI 声明完毕。接下来，我们要为该 UI 增加指定的业务逻辑。

我们的 Simple UI 的业务逻辑是：点击按钮时，文本输入框内值会变化，展示增加后的计数，每次点击按钮加一。

id!() 宏是获取传入组件实例的 LiveId 。在 makepad 中 Live System 、draw 库 和 Widget 库深度耦合。组件的渲染顺序依赖于 LiveId 。后续章节会深入讲解 Live System 相关内容。

为 App 实现 MatchEvent trait 来实现我们的业务逻辑。该 trait 是在 makepad-draw/src/match_event.rs 中被定义的，它是 Makepad 框架中处理各种事件的核心接口。

该 trait 定义了多个事件处理方法，如 handle_startup, handle_shutdown, handle_draw 等。这是将底层平台的各种事件抽象成统一的接口，简化了跨平台开发。

其中 handle_actions 方法用于处理多个 action，默认实现是遍历所有 action 并调用 handle_action。

在 MatchEvent trait 中还定义了 match_event 默认实现，它里面会调用 handle_actions 方法。

在我们前面定义的 App 入口事件处理方法 handle_event 中调用了该 match_event 方法 :

这里反映了 Makepad 将应用逻辑和 UI 组件关注点分离的架构决策。应用级别的事件处理（match event）通常更高层次和抽象。UI 事件处理更关注具体的组件交互。

Action 对于 Makepad 来说是一个重要的概念，用于表示用户界面的交互或状态变化。

在 platform/src/action.rs 中定义了 Action 的本质。

这段代码定义了 Makepad 平台中 Action 的核心结构和行为:

在 widgets/src/widget.rs 中定义了 Widget trait 核心接口，来管理实现 Widget 必须要实现的方法。WidgetRef 则是对 dyn Widget trait 对象的一个封装。

除此之外，还有 WidgetActionTrait 相关的 trait ，它是专门为 Widget 系统设计的 Action 接口，可以看作这是 ActionTrait 的一种特化。这样就允许 Widget 在运行时动态选择和执行不同的 WidgetAction 。

因为 WidgetAction 也默认实现了 ActionTrait ，所以这里可以将 action 转为 WidgetAction

我们再来看一下 makepad 内置 Button Widget 的实现。

这里只罗列出 Button Widget 的代码结构，而非全部代码。

要实现一个 Widget ，必须要为其实现 Widget trait ，包括其中两个必须的方法 handle_event 和 draw_walk 。

handle_event 中匹配特定的事件，比如 Hit::FingerDown 。匹配成功则通过 cx.widget_action 生成相应的 WidgetAction 。其中包含了 ButtonAction::Pressed 这个 Action。

draw_walk 方法是 Makepad 框架中用于绘制 Widget 的核心方法。它结合了布局计算和实际绘制操作。

这个流程展示了 Makepad 如何将底层事件转换为高抽象层面的 Actions，并在整个应用中传播。这种设计允许:

这个系统的优雅之处在于它结合了即时的事件处理和延迟的 action 处理，提供了灵活性和性能的平衡。

**Examples:**

Example 1 (bash):
```bash
1cargo new simple
```

Example 2 (unknown):
```unknown
1[dependencies]
2# 使用了 dev 分支，因为这个分支比较活跃。
3makepad-widgets = { git = "https://github.com/makepad/makepad", branch = "dev" }
```

Example 3 (unknown):
```unknown
1simple/
2├── Cargo.lock
3├── Cargo.toml
4├── src
5│   ├── app.rs
6│   ├── lib.rs
7│   └── main.rs
```

Example 4 (rust):
```rust
1pub mod app;
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/start/tutorial-ui-zoo

**Contents:**
- #Tutorial: UI Zoo

中文版本尚未发布，您可以先查看英文版。若您有兴趣，欢迎贡献中文版本翻译！

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/widget/widget-core-concepts

**Contents:**
- #Widget 核心概念
  - #与 Widget 的相关的trait
    - #WidgetNode
    - #Widget
  - #Widget 引用与管理
    - #WidgetRef
    - #WidgetCache
    - #WidgetSet

这是所有 Widget 的基础 trait :

该 trait 扩展了 WidgetNode，增加了:

WidgetCache 是 Makepad UI 框架中重要的性能优化机制，WidgetCache 贯穿整个组件树遍历过程的参数，用于控制各层组件是否启用、清除或绕过缓存。

这对于提升大型 UI 的性能很重要，因为它可以:

WidgetSet 是 WidgetRef 的集合。

**Examples:**

Example 1 (rust):
```rust
1pub trait WidgetNode: LiveApply {
2    // 一些主要的方法
3    fn uid_to_widget(&self, uid: WidgetUid) -> WidgetRef;
4    fn find_widgets(&self, path: &[LiveId], cached: WidgetCache, results: &mut WidgetSet);
5    fn walk(&mut self, cx: &mut Cx) -> Walk;
6    fn area(&self) -> Area;
7    fn redraw(&mut self, cx: &mut Cx);
8    // 其他默认实现
9}
```

Example 2 (rust):
```rust
1pub type DrawStep = Result<(), WidgetRef>;
2
3pub trait Widget: WidgetNode {
4    fn handle_event(&mut self, cx: &mut Cx, event: &Event, scope: &mut Scope);
5    fn draw_walk(&mut self, cx: &mut Cx2d, scope: &mut Scope, walk: Walk) -> DrawStep;
6
7    // 其他默认实现
8}
```

Example 3 (rust):
```rust
1pub struct WidgetRefInner {
2    pub widget: Box<dyn Widget>,
3}
4
5pub struct WidgetRef(Rc<RefCell<Option<WidgetRefInner>>>);
6
7// 关键方法
8impl WidgetRef {
9	// 其他方法
10    // 事件处理
11    pub fn handle_event(&self, cx: &mut Cx, event: &Event, scope: &mut Scope) {
12        // ...
13
14    }
15
16    // 绘制
17    pub fn draw(&mut self, cx: &mut Cx2d, scope: &mut Scope) -> DrawStep {
18        // ...
19    }
20
21    // 获取底层Widget的可变引用
22    pub fn borrow_mut<T: 'static + Widget>(&self) -> Option<RefMut<'_, T>> {
23        // ...
24    }
25
26	// 基础查找方法
27    pub fn find_widgets(&self, path: &[LiveId], cached: WidgetCache, results: &mut WidgetSet) {
28        if let Some(inner) = self.0.borrow().as_ref() {
29            inner.widget.find_widgets(path, cached, results)
30        }
31    }
32}
```

Example 4 (rust):
```rust
1// 典型调用链:
2widget.widget(&path)
3  -> widget.find_widgets(path, WidgetCache::Yes)  // 高层API默认使用缓存
4    -> child_widget.find_widgets(path, cached)    // 继续向下传递缓存参数
5      -> grandchild.find_widgets(path, cached)    // 缓存策略需要一致
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/live-DSL/props-inherit

**Contents:**
- #属性继承
- #继承方式
  - #1. 属性继承
  - #2. 布局继承
  - #3. 动画继承
  - #4. 事件处理继承
- #继承式创建新组件
- #继承的建议
- #Rust 结构体的继承

在 Live DSL 里 ProductCard = <Card> 这种方法一般是代表「引用」一个组件。

注意这种写法： MyButton = {{MyButton}} <Button> ，是基于已有 Button Widget 进行继承性创建自己的 MyButton 组件。

Rust 结构体中的 #[deref] 属性宏：

这是一个编译期的 Rust trait 实现机制：

#[deref] 与 Live DSL 中引用组件实例的关键区别：

**Examples:**

Example 1 (rust):
```rust
1// 基础卡片组件
2Card = {{MyView}} {
3    // 基础属性
4    background_color: #fff,
5    corner_radius: 4.0,
6    shadow_color: #0007,
7
8    // 基础布局
9    flow: Down,
10    spacing: 10
11}
12
13// 产品卡片扩展
14ProductCard = <Card> {
15    // 1. 覆盖父组件属性
16    background_color: #f5f5f5,  // 覆盖背景色
17    corner_radius: 8.0,         // 覆盖圆角
18
19    // 2. 添加新的属性
20    width: 200,
21    height: 300,
22    border_width: 1.0,         // 新增边框
23
24    // 3. 添加子组件
25    image_container = <View> {
26        height: 160
27    }
28
29    title = <Label> {
30        text_wrap: Word,
31        color: #333
32    }
33
34    price = <Label> {
35        color: #f00,
36        font_size: 18
37    }
38
39    buy_button = <Button> {
40        width: Fill,
41        label: "Buy Now"
42    }
43}
44
45// 可以进一步继承和扩展
46FeaturedProductCard = <ProductCard> {
47    // 覆盖属性
48    height: 350,
49    background_color: #fff0f0,
50
51    // 添加新组件
52    badge = <Label> {
53        text: "Featured",
54        background_color: #f00,
55        color: #fff
56    }
57}
```

Example 2 (rust):
```rust
1ParentView = {{View}} {
2    color: #f00,
3    spacing: 10
4}
5
6ChildView = <ParentView> {
7    // 继承 color 和 spacing
8    // 覆盖 color
9    color: #00f,
10    // 添加新属性
11    margin: 20
12}
```

Example 3 (rust):
```rust
1CardBase = {{View}} {
2    flow: Down,
3    spacing: 10,
4
5    header = <View> {
6        height: 50
7    }
8}
9
10CustomCard = <CardBase> {
11    // 继承布局属性
12    // 修改子组件
13    header = <View> {
14        height: 60,
15        background_color: #f00
16    }
17}
```

Example 4 (rust):
```rust
1ButtonBase = {{Button}} {
2    animator: {
3        hover = {
4            default: off
5            on = {
6                apply: {color: #f00}
7            }
8        }
9    }
10}
11
12CustomButton = <ButtonBase> {
13    // 继承动画
14    animator: {
15        // 添加新的动画状态
16        pressed = {
17            default: off
18            on = {
19                apply: {scale: 0.9}
20            }
21        }
22    }
23}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/adding-a-searchbox

**Contents:**
- #16 - 添加搜索框
- #你将学到的内容
- #添加放大镜图标
- #定义变量
- #定义 SearchBox
- #更新菜单栏（MenuBar）
- #检查到目前为止的进展
- #下一步

在前面的步骤中，我们实现了在图片网格和幻灯片之间切换的功能。接下来的几个步骤，我们将实现根据查询字符串过滤图片的功能。

本步骤中，我们将重点添加菜单栏上的搜索框。

注意：如果你不想手动输入代码，可以在这里找到本步骤的完整代码：https://github.com/makepad/image_viewer/tree/main/step_16

为了实现搜索框，我们需要一个放大镜图标，因此首先要把它作为资源添加到我们的应用中。

请进入 resources 目录，下载以下文件到该目录：looking_glass.svg

现在我们已经添加了放大镜图标，接下来在 DSL 代码中定义一个变量来引用这个放大镜图标。

在 app.rs 中，添加以下代码到 live design 代码块的顶部：

既然我们已经有了引用占位图像的方法，现在可以添加 SearchBox 的定义了。SearchBox 是一个简单的容器，组合了一个 Icon 和一个 TextInput。Icon 是用来显示图标的简单组件，而 TextInput 用于输入文本。

在 app.rs 中，添加以下代码到 live design 代码块，位于 MenuBar 定义的上方：

这段代码定义了一个搜索框（SearchBox），具有以下属性：

搜索框中的文本输入框（TextInput）具有以下属性：

既然我们已经有了一个搜索框（SearchBox），接下来让我们更新菜单栏的定义，将搜索框包含进去。

在 app.rs 文件中，找到 live design 块中的 MenuBar 定义，替换成下面的代码：

确保你当前在你的包目录下，然后运行以下命令：

如果一切正常，你现在应该能在菜单栏中看到一个搜索框，位于我们之前创建的图片网格上方：

在这一步，我们在图片网格上方的菜单栏中添加了一个搜索框。下一步，我们将更新应用的状态，根据搜索框中的查询内容过滤显示的图片。

**Examples:**

Example 1 (rust):
```rust
1LOOKING_GLASS = dep("crate://self/resources/looking_glass.svg");
```

Example 2 (rust):
```rust
1SearchBox = <View> {
2        width: 150,
3        height: Fit,
4        align: { y: 0.5 }
5        margin: { left: 75 }
6
7        <Icon> {
8            icon_walk: { width: 12.0 }
9            draw_icon: {
10                color: #8,
11                svg_file: (LOOKING_GLASS)
12            }
13        }
14
15        query = <TextInput> {
16            empty_text: "Search",
17            draw_text: {
18                text_style: { font_size: 10 },
19                color: #8
20            }
21        }
22    }
```

Example 3 (rust):
```rust
1MenuBar = <View> {
2        width: Fill,
3        height: Fit,
4
5        <SearchBox> {}
6        <Filler> {}
7        slideshow_button = <Button> {
8            text: "Slideshow"
9        }
10    }
```

Example 4 (bash):
```bash
1cargo run --release
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/click-to-slideshow

**Contents:**
- #20 - 创建点击放映
- #你将学到的内容
- #定义 Action
- #处理鼠标点击事件
  - #action 的传递方法
- #处理 action
- #检查到目前为止的进度

在上一步中，我们将不同的组件拆分成不同的模块，增加了代码的可维护性。在这一步中，我们将实现在网格模式下点击图片时候，自动切换到 slideshow 模式。

这一步，我们将重点放在如何使用 action 实现组件之间的通信上。

注意: 如果你不想自己敲代码，可以在这里找到本步骤的完整代码： https://github.com/Project-Robius-China/image-viewer-workshop

让我们先在 ImageItem 中，定义 action，关于 action 的讲解在处理事件中做过讲解。

在 image_item.rs 中，定义 ImageClickedAction :

这里定义了 ImageClickedAction 用于处理图片点击事件，它是一个枚举，有两个字段: Clicked(usize) 和 None。这里我们讲解一下关于在 Makepad 中 action 的定义和使用：

如前面章节中讲到，action 是 Makepad 中进行组件之间通信的一个重要方式。正如上面所见，Makepad 中 action 是以枚举的形式出现，你可以在这个枚举中定义你想要响应的用户动作。

Makepad 中，将 事件 event 和 动作 action 进行了区分：

这样可以更好的控制协调各种事件，并且在代码层级解耦，增加代码的可维护性。

一般的，我们要为创建的 action 实例实现三个 trait：

这三个trait 也是 Makepad 的默认 action 开发规范。

在 image_item.rs 文件中，用下面的代码替换 ImageItem 结构体的定义：

这为 ImageItem 结构体增加了以下字段：

在 handle_event 中，用下面的代码替换:

event.hits() 返回一个枚举 Hit,其中封装了常见的鼠标事件，所以这里我们使用 match 来进行匹配，Hit::FingerUp(_) 表示当我们点击鼠标左键手指抬起时的动作，也就是我们鼠标点击事件。这里我们只使用到了鼠标的点击事件，所以对于其他事件，我们使用 _ 进行统一处理。

cx.action 是传递 action 的方式，这里我们我们定义好的 action 传入这个函数。在 action 中带上我们设置的 image_index 字段，确保根组件能够正确获取到从那一张图片进行放映。

切换放映模式是在根组件下控制，所以我们要在 app.rs 下处理，在 handle_actions() 中增加一下代码：

for action in actions {...} 是将 action 从 参数 actions 中遍历出来。action.cast() 将遍历处的 action 进行投射，直到拿到我们设定的 ImageClickedAction::Clicked(image_idx), 进行接下来的处理。

之后调用 set_current_image() 来把点击的图片设置为当前需要播放的图片，之后开启放映模式，设置页面焦点，与我们直接点击放映按钮的功能一致。

注意: 如果你是使用 cx.widget_action() 来传递 action，那么相应的，你也应该使用 action.as_widget_action() 来接收 action。

如果一切正常，那么当鼠标点击图片，就可出现放映效果。

**Examples:**

Example 1 (rust):
```rust
1#[derive(Clone, Debug, Eq, Hash, Copy, PartialEq)]
2pub enum ImageClickedAction {
3    Clicked(usize),
4    None,
5}
```

Example 2 (rust):
```rust
1#[derive(Live, LiveHook, Widget)]
2pub struct ImageItem {
3    #[deref]
4    view: View,
5
6    #[live]
7    image_index: usize,
8}
```

Example 3 (rust):
```rust
1fn handle_event(&mut self, cx: &mut Cx, event: &Event, scope: &mut Scope) {
2        self.view.handle_event(cx, event, scope);
3        match event.hits(cx, self.view.area()) {
4            Hit::FingerUp(_) => {
5                cx.action(ImageClickedAction::Clicked(self.image_index));
6            }
7            _ => {}
8        }
9    }
```

Example 4 (rust):
```rust
1for action in actions {
2    if let ImageClickedAction::Clicked(image_idx) = action.cast() { 
3        self.set_current_image(cx, Some(image_idx));
4        self.ui.page_flip(id!(page_flip)).set_active_page(cx, live_id!(slideshow));
5        self.ui.view(id!(slideshow.overlay)).set_key_focus(cx);
6    }
7}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/animation-system/building-basic-animation-component

**Contents:**
- #构建一个基本的动画组件
- #基础结构设计
- #动画状态定义
- #实现动画逻辑
- #动画控制接口

这个结构为我们的动画组件提供了必要的基础设施。

在 Makepad 中，我们使用 live_design! 宏来声明动画状态：

在 Makepad 的 Live DSL 中，animator 是一个特殊的属性，用于定义组件的动画状态和行为。让我们深入了解它的语法和结构。

注意： Makepad 中实现动画要基于 NextFrame ，而非 Timer。因为 Timer 依赖于底层 OS API ，有跨平台的风险。

**Examples:**

Example 1 (rust):
```rust
1// 1. 首先声明组件结构
2#[derive(Live, Widget)]
3pub struct TypingAnimation {
4    #[deref] view: View,          // 基础视图组件
5    #[animator] animator: Animator,  // 动画控制器
6
7    // 动画参数
8    #[live(0.65)] animation_duration: f64,
9    #[live(10.0)] swing_amplitude: f64,
10    #[live(3.0)] swing_base: f64,
11
12    // 运行时状态
13    #[rust] next_frame: Option<NextFrame>,
14    #[rust] animation_start_time: f64,
15    #[rust] is_animating: bool,
16}
```

Example 2 (rust):
```rust
1live_design! {
2    TypingAnimation = {{TypingAnimation}} {
3        // 基础属性配置
4        width: Fit,
5        height: Fit,
6
7        // 动画状态定义
8        animator: {
9	        // 动画状态组
10            circle1 = {
11                default: down,  // 默认状态
12                down = {       // 向下移动状态
13                    redraw: true,
14                    from: {all: Forward {duration: 0.325}}
15                    ease: InOutQuad
16                    apply: {content = { circle1 = { margin: {top: 10.0} }}}
17                }
18                up = {        // 向上移动状态
19                    redraw: true,
20                    from: {all: Forward {duration: 0.325}}
21                    ease: InOutQuad
22                    apply: {content = { circle1 = { margin: {top: 3.0} }}}
23                }
24            }
25        }
26    }
27}
```

Example 3 (rust):
```rust
1animator: {
2    circle1_position = {  // 位置的 track
3        default: down,
4        down = { /* 控制位置的动画状态 */ }
5    }
6    circle1_opacity = {   // 透明度的 track
7        default: visible,
8        hidden = { /* 控制透明度的动画状态 */ }
9    }
10}
```

Example 4 (rust):
```rust
1// from
2from: {
3    all: Forward {duration: 0.2}      // 正向播放一次
4    all: Reverse {                    // 反向播放一次
5        duration: 0.2,
6        end: 1.0
7    }
8    all: Loop {                       // 循环播放
9        duration: 0.2,
10        end: 1.0
11    }
12    all: BounceLoop {                 // 来回循环播放
13        duration: 0.2,
14        end: 1.0
15    }
16    all: Snap                         // 瞬间切换，无动画
17}
18// ease
19// - `OutQuad`/`OutCubic`: 适用于自然运动
20// - `InOutQuad`: 适用于可逆动画
21// - `Linear`: 适用于旋转等持续动画
22ease: Linear          // 线性
23ease: InQuad          // 二次方加速
24ease: OutQuad         // 二次方减速
25ease: InOutQuad       // 二次方加速减速
26ease: Bezier {        // 贝塞尔曲线
27    cp0: 0.0,
28    cp1: 0.0,
29    cp2: 1.0,
30    cp3: 1.0
31}
32
33// apply
34apply: {
35    opacity: 1.0,
36    scale: 1.2,
37    color: #f00,
38    position: vec2(100.0, 0.0)
39}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/creating-a-slideshownavigatebutton

**Contents:**
- #8 - 创建幻灯片导航按钮
- #你将学到的内容
- #添加箭头图标
- #定义变量
  - #定义 SlideshowNavigateButton
  - #继承基础简介
- #更新 App
- #检查我们目前的进展
- #下一步

在之前的几个步骤中，我们为应用构建了一个图片网格。图片网格允许你同时查看多张图片。但是有时候，我们并不想一次看多张图片，而是想一张一张地浏览，这就是幻灯片的作用。接下来的几个步骤里，我们将为应用创建一个幻灯片功能。

为了保持最初实现的简单性，幻灯片目前只会显示一个占位符图片，暂时不会响应用户事件。我们将在后续步骤中去掉这些限制。

本步骤中，我们将从创建这两个 SlideshowNavigateButton 开始。

注意：如果你不想手动输入代码，可以在这里找到本步骤的完整代码：https://github.com/makepad/image_viewer/tree/main/step_8

对于两个 SlideshowNavigateButton，我们需要一些箭头图标，因此首先需要将它们作为资源添加到应用中。

进入 resources 目录，下载以下文件到该目录：left_arrow.svg, right_arrow.svg

既然我们已经添加了箭头图标，现在我们需要在 DSL 代码中定义一些变量来引用这些图标。

在 app.rs 中，将以下代码添加到 live design 块的顶部：

这段代码定义了两个变量：LEFT_ARROW 和 RIGHT_ARROW。我们将在 DSL 代码的其他地方使用这两个变量来引用箭头图标。

现在我们已经有了引用箭头图标的方法，接下来可以添加 SlideshowNavigateButton 的定义。SlideshowNavigateButton 是一个高而窄的条形按钮，占据其容器的整个高度，并在中间显示一个箭头图标。

在 app.rs 中，将以下代码添加到 live design 块中，位置在 App 定义之前：

该定义创建了一个具有以下属性的 SlideshowNavigateButton：

现在是一个很好的时机来简单了解一下 Makepad 中的继承机制。

Makepad 中的继承非常类似于 JavaScript 等语言中的原型继承（Prototypal Inheritance）：

我们刚刚定义的 SlideshowNavigateButton 就是一个例子。它的定义如下：

这意味着 SlideshowNavigateButton 派生自 Button。还记得我们通过 use link::widgets::*; 导入的内建组件吗？Button 就是其中之一。SlideshowNavigateButton 会复制 Button 的所有属性，并覆盖其中的一些属性以实现自定义行为。

你可能注意到，在我们定义 SlideshowNavigateButton 时，并没有为图标指定具体的图像。这是有意为之的，因为 SlideshowNavigateButton 本身是作为一个基类来使用的 —— 每当我们创建它的一个实例时，我们会为该实例单独指定一个图像作为图标。你马上就会在更新 App 的时候看到这个用法的例子。

现在我们已经有了 SlideshowNavigateButton，接下来我们要更新 App 的定义，让它显示两个 SlideshowNavigateButton，而不是之前的 ImageGrid（稍后我们会在讲解如何在不同视图之间切换时，把 ImageGrid 放回来）。

在 app.rs 文件的 live design 代码块中，用以下代码替换 App 的定义部分：

如你所见，我们创建了两个 SlideshowNavigateButton 的实例。对于每个 SlideshowNavigateButton，我们都重写了 draw_icon 的 svg_file 属性，使用了之前定义的箭头图标变量。

请确保你当前位于项目的包目录中，然后运行以下命令：

如果一切正常运行，你应该会看到两个按钮 —— 一个带有左箭头图标，另一个带有右箭头图标：

在这一步，我们创建了两个 SlideshowNavigateButton。下一步，我们将创建 SlideshowOverlay。

**Examples:**

Example 1 (rust):
```rust
1LEFT_ARROW = dep("crate://self/resources/left_arrow.svg");
2RIGHT_ARROW = dep("crate://self/resources/right_arrow.svg");
```

Example 2 (rust):
```rust
1SlideshowNavigateButton = <Button> {
2        width: 50,
3        height: Fill,
4        draw_bg: {
5            color: #fff0,
6            color_down: #fff2,
7        }
8        icon_walk: { width: 9 },
9        text: "",
10        grab_key_focus: false,
11    }
```

Example 3 (rust):
```rust
1SlideshowNavigationButton = <Button> {
2        ...
3    }
```

Example 4 (rust):
```rust
1App = {{App}} {
2        ui: <Root> {
3            <Window> {
4                body = <View> {
5                    <SlideshowNavigateButton> {
6                        draw_icon: { svg_file: (LEFT_ARROW) }
7                    }
8                    <SlideshowNavigateButton> {
9                        draw_icon: { svg_file: (RIGHT_ARROW) }
10                    }
11                }
12            }
13        }
14    }
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/widget/customized-button-widget

**Contents:**
- #自定义 Button Widget

让我们创建一个自定义 Button 组件。

总的来说，这段代码定义了一个高度可定制的按钮组件 MyButton，具有丰富的视觉效果如发光、阴影、悬停变色等。它继承了 Button 的功能,还添加了初始化检查和一些辅助方法。通过 live_design! 宏可以方便地定义按钮的样式。

特别说明：定义 Widget 主要的 Live 属性标记如下。

**Examples:**

Example 1 (rust):
```rust
1use makepad_widgets::*;
2
3live_design! {
4    use link::theme::*;
5    use link::shaders::*;
6    use link::widgets::*;
7
8    // 定义一个通用的按钮样式
9    // 继承自 Button
10    pub MyButton = {{MyButton}} <Button> {
11        width: 200, // 按钮宽度
12        height: 50, // 按钮高度
13        margin: {left: 20, right: 20}, // 按钮左右外边距
14
15        text: "My Button", // 按钮文字
16        draw_text: {
17            color: #ffffff // 文字颜色为白色
18        },
19
20        draw_bg: {
21            // 这里最多定义 6 个 instance，否则报错 subtract with overflow
22            instance background_color: #0000ff, // 背景色
23            instance hover_color: #0055ff, // 鼠标悬停时的颜色
24            instance pressed_color: #00008B, // 鼠标按下时的颜色
25
26            instance border_width: 1.0, // 边框宽度
27            instance border_color: #00f3ff, // 边框颜色
28
29            instance glow: 0.0, // 发光效果控制
30            instance hover: 0.0, // 控制鼠标悬停效果
31            instance pressed: 0.0, // 控制鼠标按下效果
32
33            fn pixel(self) -> vec4 {
34                let sdf = Sdf2d::viewport(self.pos * self.rect_size);
35                // sdf.box(0.0, 0.0, self.rect_size.x, self.rect_size.y, 8.0);
36
37                // 计算缩放和位移
38                let scale = 1.0 - self.pressed * 0.04; // 按下时稍微缩小
39                let size = self.rect_size * scale;
40                let offset = (self.rect_size - size) * 0.5; // 居中
41
42                // 绘制外层发光
43                sdf.box(
44                    offset.x ,  // 向外扩展4个像素
45                    offset.y ,
46                    size.x ,    // 两边各扩展4个像素
47                    size.y ,
48                    9.0            // 稍大的圆角
49                );
50
51                // 发光效果 - 使用半透明的边框颜色
52                let glow_alpha = self.glow * 0.5; // 控制发光强度
53                sdf.fill_keep(vec4(self.border_color.xyz, glow_alpha));
54
55
56                // 简化绘制，只保留主体
57                sdf.box(
58                    offset.x,
59                    offset.y,
60                    size.x,
61                    size.y,
62                    8.0
63                );
64
65                // 未按下时显示阴影,按下时减弱阴影
66                let shadow_alpha = (1.0 - self.pressed) * 0.2;
67                sdf.fill_keep(vec4(0.,0.,0.,shadow_alpha));
68
69                // 基础颜色
70                let base_color = self.background_color;
71
72                // hover效果通过降低透明度来实现,不直接修改颜色
73                let hover_alpha = self.hover * 0.2;
74                let color_with_hover = mix(
75                    base_color,
76                    vec4(1.0, 1.0, 1.0, 1.0),
77                    hover_alpha
78                );
79
80                // pressed效果
81                let final_color = mix(
82                    color_with_hover,
83                    self.pressed_color,
84                    self.pressed
85                );
86
87
88                // 先填充主体颜色
89                sdf.fill_keep(final_color);
90
91                // 边框发光效果
92                let border_glow = max(self.hover * 0.5, self.glow);
93                let border_color = mix(
94                    self.border_color,
95                    vec4(1.0, 1.0, 1.0, 0.8),
96                    border_glow
97                );
98                sdf.stroke(border_color, self.border_width);
99
100                return sdf.result
101            }
102        }
103
104    }
105}
106
107
108// 定义组件结构体
109#[derive(Live,Widget)]
110pub struct MyButton {
111    // 继承 Button 的所有功能
112    #[deref]
113    button: Button,
114    #[rust]
115    initialized: bool, // 标记是否已初始化
116}
117
118impl LiveHook for MyButton {
119    fn after_new_from_doc(&mut self, cx: &mut Cx) {
120        log!("MyButton: after_new_from_doc");
121        self.initialized = true; // 在创建后就将其标记为已初始化
122        self.button.after_new_from_doc(cx);
123        log!("button text is empty? {:?}", self.button.text.as_ref())
124    }
125
126    fn before_apply(&mut self, cx: &mut Cx, apply: &mut Apply, index: usize, nodes: &[LiveNode]) {
127        log!("MyButton: before_apply");
128        self.button.before_apply(cx, apply, index, nodes);
129    }
130
131    fn after_apply(&mut self, cx: &mut Cx, apply: &mut Apply, index: usize, nodes: &[LiveNode]) {
132        log!("MyButton: after_apply");
133        self.button.after_apply(cx, apply, index, nodes);
134    }
135}
136
137
138impl Widget for MyButton {
139    fn handle_event(&mut self, cx: &mut Cx, event: &Event, scope: &mut Scope) {
140        log!("MyButton handle_event");
141        log!("MyButton not initialized!");
142        self.button.handle_event(cx, event, scope);
143    }
144
145    fn draw_walk(&mut self, cx: &mut Cx2d, scope: &mut Scope, walk: Walk) -> DrawStep {
146        log!("MyButton draw_walk");
147        self.initialized = true;
148        log!("MyButton initialized!");
149        self.button.draw_walk(cx, scope, walk)
150    }
151}
152
153impl MyButtonRef {
154    pub fn clicked(&self, actions: &Actions) -> bool {
155        self.borrow().map(|button| button.button.clicked(actions)).unwrap_or(false)
156    }
157
158    pub fn apply_over(&self, cx: &mut Cx, nodes: LiveNodeSlice) {
159        if let Some(mut inner) = self.borrow_mut() {
160            log!("Applying style to MyButton");
161            // 应用样式到内部按钮
162            inner.button.apply_over(cx, nodes);
163            // 确保重绘
164            inner.button.redraw(cx);
165        } else {
166            log!("Failed to borrow MyButton - this may indicate an initialization problem");
167        }
168    }
169
170    pub fn set_text_and_redraw(&self, cx: &mut Cx, text: &str) {
171        if let Some(mut inner) = self.borrow_mut() {
172            inner.button.set_text_and_redraw(cx, text);
173            inner.button.redraw(cx);
174        }
175    }
176
177    // 添加检查方法
178    pub fn is_some(&self) -> bool {
179        self.borrow().is_some()
180    }
181}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/app-examples

**Contents:**
- #应用与示例

你可以从 Makepad仓库中的示例examples及本书的示例入手单元来学习 Makepad。

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/creating-an-imageitem

**Contents:**
- #3 - 创建 ImageItem
- #你将学到的内容
- #添加占位符图片
- #定义变量
- #定义 ImageItem
- #更新 App
- #检查到目前为止的进展
- #下一步

在上一步中，我们创建了一个包含空窗口的最简 Makepad 应用程序。接下来的几步中，我们将为应用构建一个图片网格。

本步骤中，我们将先从创建一个用于显示单张图片的 ImageItem 开始。

注意：如果你不想手动输入代码，可以在这里找到本步骤的完整代码：https://github.com/makepad/image_viewer/tree/main/step_3

如前所述，我们的初始实现将使用占位符图片来代替真实图片。要使用占位符图片，首先需要将其作为资源添加到我们的应用中。

为此，首先确保你处于你的包目录下，然后运行：

这将创建一个名为 resources 的新目录。

进入 resources 目录，并将以下文件下载到该目录：placeholder.jpg

现在我们已经添加了占位符图片，可以在 DSL 代码中通过以下表达式来引用它：

这个表达式表示一个依赖。Makepad 中的依赖是随应用打包的资源——在这里指的就是我们之前添加到 resources 目录中的占位符图片。

不过每次想引用占位符图片时都写这么长的表达式会非常冗长，所以我们定义一个变量来简化它。

在 app.rs 中，添加以下代码到 live design 块的顶部：

既然我们有了引用占位符图片的方法，就可以为 ImageItem 添加定义了。ImageItem 作为图片的容器，同时还可以存放关于该图片的其他元数据（比如文件名、时间戳等），这些信息我们以后可能会用到。

在 app.rs 中，将以下代码添加到 live design 块中，放在 App 定义之前：

这段代码定义了一个 ImageItem，包含以下属性：

在 ImageItem 内部，我们使用 Image 组件来显示实际的图片：

注意：在 DSL 代码中计算表达式时，需要用括号 (...) 包裹起来。由于变量本身也是表达式，所以我们写成 (PLACEHOLDER) 来计算 PLACEHOLDER 变量。

既然我们已经有了 ImageItem，接下来更新 App 的定义，让它显示一个 ImageItem，而不是一个空的 View。

在 app.rs 中，将 live design 块里 App 的定义替换为下面的内容：

注意：在当前版本的 DSL 中，定义的顺序很重要。特别是，不能引用尚未在代码中先前定义的内容，因此请确保在 App 中引用 ImageItem 之前，先定义好 ImageItem。

如果一切正常，屏幕上应该会出现一个包含单张图片的窗口：

在这一步，我们创建了一个用于显示单张图片的 ImageItem。下一步，我们将把多个 ImageItem 组合成一个 ImageRow，以显示一行图片。

**Examples:**

Example 1 (bash):
```bash
1mkdir resources
```

Example 2 (rust):
```rust
1dep("crate://self/resources/placeholder.jpg")
```

Example 3 (rust):
```rust
1PLACEHOLDER = dep("crate://self/resources/placeholder.jpg");
```

Example 4 (rust):
```rust
1ImageItem = <View> {
2        width: 256,
3        height: 256,
4        
5        image = <Image> {
6            width: Fill,
7            height: Fill,
8            fit: Biggest,
9            source: (PLACEHOLDER)
10        }
11    }
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/live-DSL/index

**Contents:**
- #基本概念
- #生命周期
  - #1. 输入阶段
  - #2. Live Compiler 阶段
  - #3. 展开阶段
  - #4. 实例化阶段
  - #5. 运行时交互
- #Live Node
  - #1. 节点的基本结构
  - #2. 节点的类型(LiveValue)

Live 语言是一个专门用于 UI 组件和样式的领域特定语言（DSL）。它的基本结构是以节点为单位，每个节点可以包含属性和子节点。

Live DSL是在运行时实时编译的，支持热重载和动态更新。

Live DSL 运行时编译大概过程如下：

第一步输入 Live Documents 。

Live Original 存储了从源代码解析得到的原始节点数据，包含：

LiveOriginal 相当于 Live DSL 的 AST(抽象语法树)。

展开阶段是对 DSL 中的组件声明进行扩展(expand)，比如处理属性展开、继承、建立组件关系，并设置运行时上下文。这为后续的渲染和交互打下基础。

所以它适合用于 Widget Registry 。

Widget Node 是 UI 组件树中的基本节点，它提供基础的布局、遍历和绘制能力，且负责组件的区域管理和重绘，支持组件查找和导航。 同时是 Widget trait 的基础要求。

一个 Widget 实例可以包含多个 Node，每个 Node 负责特定的底层功能。Widget 通过组合 Node 实现完整功能。

Widget 通过 Event、Action 和 Scope 这三个重要机制来实现运行时交互。其细节在事件章节详细介绍。

Live Node 可以理解为解析以后的每个 AST 节点。

在代码实现中，节点由 LiveNode 结构表示：

**Examples:**

Example 1 (rust):
```rust
1// Live DSL 代码
2Button = {
3    width: 100,
4    height: 50,
5    text: "Click me"
6}
```

Example 2 (rust):
```rust
1pub struct TokenWithSpan {
2    pub token: LiveToken,
3    pub span: TextSpan
4}
5
6// 将代码转换成 token 流
7[
8    Token::Ident("Button"),
9    Token::Punct("="),
10    Token::Open(Brace),
11    Token::Ident("width"),
12    Token::Punct(":"),
13    Token::Int(100),
14    ...
15]
```

Example 3 (rust):
```rust
1pub struct LiveParser<'a> {
2    pub token_index: usize,
3    pub file_id: LiveFileId,
4    pub tokens_with_span: Cloned<Iter<'a, TokenWithSpan>>,
5    // ...
6}
7
8// 将 token 流解析成初始的节点结构
```

Example 4 (rust):
```rust
1pub struct LiveOriginal {
2    pub nodes: Vec<LiveNode>,           // 解析出的节点列表
3    pub edit_info: Vec<LiveNode>,       // 编辑相关的元数据节点
4    pub design_info: Vec<LiveDesignInfo>, // 设计阶段的信息
5    pub tokens: Vec<TokenWithSpan>,     // 源代码的token流
6}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/handling-events

**Contents:**
- #12 - 处理事件
- #你将学到的内容
- #添加辅助方法
- #事件-动作流程
- #实现MatchEvent Trait
  - #处理按钮点击事件
  - #处理按键事件
- #检查目前的进度
- #下一步

在上一步中，我们为应用添加了更多状态，以使幻灯片更加动态。

我们已经添加了状态，现在将使用它来处理用户事件。

注意：如果你不想手动输入代码，可以在这里找到本步骤的所有代码：https://github.com/makepad/image_viewer/tree/main/step_12

为了让后续代码更易编写，我们将为 App 结构体添加更多辅助函数。

在 app.rs 文件中，添加以下代码到 App 结构体的 impl 块中：

我们刚刚添加了几个辅助方法，用来方便在运行时更新幻灯片。下一步是将这些方法连接起来，使得当用户与 UI 交互时（点击幻灯片中的按钮，或者按键盘上的左右方向键），这些方法能够被调用。为此，我们将使用一种叫做“动作”（actions）的机制。

动作是通知某个控件（widget）发生了有趣事件的通知。例如，Button 控件有一个 clicked 动作，当按钮被点击时，它会向应用的其他部分发送该通知。

相比之下，Makepad 中的事件（event）是用户进行了某种操作的通知。例如，用户点击鼠标按钮时，会触发一个 mouse-down 事件，并发送给应用。

下面是 Makepad 中事件-动作流程的工作原理：

这种模式有助于将底层的输入处理与高层的 UI 行为分离开来。

为了处理我们应用中的动作，我们可以使用 MatchEvent trait。

MatchEvent trait 提供了几个可以重写的方法，这些方法会在特定事件发生时被调用。当你在 MatchEvent trait 上调用 match_event 方法并传入一个事件时，它会自动将该事件转发给对应的方法。

我们将从在 App 结构体的 handle_event 方法中添加对 match_event 的调用开始：

在 app.rs 文件中，用下面的代码替换 App 结构体的 AppMain trait 实现：

接下来，我们将为 App 结构体实现 MatchEvent trait。对于我们的使用场景，重点关注的方法是 handle_actions。

这段代码会检查幻灯片中的某个按钮是否被点击。如果是，它会调用相应的辅助方法（navigate_left 或 navigate_right）来更新幻灯片。

注意：当一个或多个动作由某个控件触发时，会派发一个包含这些动作列表的动作事件。当对 MatchEvent trait 调用 match_event 方法并传入动作事件时，会导致 handle_actions 方法被调用，并传入触发的动作列表。为了处理动作，每个控件都会提供一个或多个辅助方法，比如上面 Button 上的 clicked 方法。当你使用动作列表调用该方法时，它会检查列表中是否有动作表示按钮被点击，如果有则返回 true。

这段代码会检查幻灯片覆盖层是否具有键盘焦点，并且是否按下了左箭头或右箭头键。如果是，它会调用相应的辅助方法（navigate_left 或 navigate_right）来更新幻灯片。

如果一切正常，你应该会看到和之前一样的幻灯片播放效果。

不过这一次，点击左右两边的按钮应该分别导航幻灯片到上一张或下一张图片。或者，按键盘上的左箭头或右箭头键也应产生相同的效果：

我们现在已经有了一个相当不错的幻灯片实现。它可以显示真实的图片，并且能响应用户事件，比如按钮点击和鼠标点击，从而导航到上一张或下一张图片。

我们暂时先保持幻灯片的现状。在接下来的几个步骤中，我们将实现能够在图片查看器和幻灯片之间切换的功能。

**Examples:**

Example 1 (rust):
```rust
1pub fn navigate_left(&mut self, cx: &mut Cx) {
2        if let Some(image_idx) = self.state.current_image_idx {
3            if image_idx > 0 {
4                self.set_current_image(cx, Some(image_idx - 1));
5            }
6        }
7    }
8
9    pub fn navigate_right(&mut self, cx: &mut Cx) {
10        if let Some(image_idx) = self.state.num_images() {
11            if image_idx + 1 < self.state.image_paths.len() {
12                self.set_current_image(cx, Some(image_idx + 1));
13            }
14        }
15    }
```

Example 2 (rust):
```rust
1impl AppMain for App {
2    fn handle_event(&mut self, cx: &mut Cx, event: &Event) {
3        self.match_event(cx, event);
4        self.ui
5            .handle_event(cx, event, &mut Scope::with_data(&mut self.state));
6    }
7}
```

Example 3 (rust):
```rust
1impl MatchEvent for App {
2    fn handle_actions(&mut self, cx: &mut Cx, actions: &Actions) {
3        if self.ui.button(id!(navigate_left)).clicked(&actions) {
4            self.navigate_left(cx);
5        }
6        if self.ui.button(id!(navigate_right)).clicked(&actions) {
7            self.navigate_right(cx);
8        }
9
10        if let Some(event) =
11            self.ui.view(id!(slideshow.overlay)).key_down(&actions)
12        {
13            match event.key_code {
14                KeyCode::ArrowLeft => self.navigate_left(cx),
15                KeyCode::ArrowRight => self.navigate_right(cx),
16                _ => {}
17            }
18        }
19    }
20}
```

Example 4 (rust):
```rust
1if self.ui.button(id!(navigate_left)).clicked(&actions) {
2        self.navigate_left(cx);
3    }
4    if self.ui.button(id!(navigate_right)).clicked(&actions) {
5        self.navigate_right(cx);
6    }
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/creating-a-slideshow

**Contents:**
- #10 - 创建幻灯片放映
- #你将学到的内容
- #定义 Slideshow
- #更新 App
- #检查到目前为止的进展
- #下一步

在上一步中，我们创建了 SlideshowOverlay，作为为我们的应用构建幻灯片放映的一部分。

在这一步，我们将创建 Slideshow。

注意：如果你不想跟着敲代码，可以在这里找到本步骤的全部代码：https://github.com/makepad/image_viewer/tree/main/step_10

让我们从添加 Slideshow 的定义开始。Slideshow 结合了一个 Image 和我们之前创建的 SlideshowOverlay。

在 app.rs 文件的 live design 块中，添加以下代码，放在 SlideshowOverlay 定义之后：

这定义了一个具有以下属性的 Slideshow：

既然我们已经有了 Slideshow，现在让我们更新 App 的定义，使其显示 Slideshow，而不是 SlideshowOverlay。

在 app.rs 文件中，用下面的代码替换 live design 块中 App 的定义：

如果一切正常，你应该会看到和之前一样的两个按钮，不过这次它们背后有一张占位图片：

在这一步中，我们创建了 Slideshow。

我们现在有了一个幻灯片的初步实现。它功能还很有限——目前只显示一张占位图片，还不能响应用户事件。

在接下来的几步中，我们将去除这些限制，让幻灯片变得动态起来。

**Examples:**

Example 1 (rust):
```rust
1Slideshow = <View> {
2        flow: Overlay,
3
4        image = <Image> {
5            width: Fill,
6            height: Fill,
7            fit: Biggest,
8            source: (PLACEHOLDER)
9        }
10        overlay = <SlideshowOverlay> {}
11    }
```

Example 2 (rust):
```rust
1App = {{App}} {
2        ui: <Root> {
3            <Window> {
4                body = <View> {
5                    slideshow = <Slideshow> {}
6                }
7            }
8        }
9    }
```

Example 3 (bash):
```bash
1cargo run --release
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/shading-language/index

**Contents:**
- #Shader 基础概念
- #什么是Shader
- #GPU vs CPU 渲染的区别
- #坐标系统详解
  - #归一化设备坐标(NDC)
  - #像素坐标
  - #UV 坐标：
  - #代码和图示来理解坐标转换
- #颜色与像素

Makepad Shader 是自定义的 MPSL 着色语言。 MPSL 可以生成为 glsl 等着色语言。

推荐前置知识学习：GLSL 着色器入门教程 https://thebookofshaders.com/?lan=ch

让我们先从一个简单的比喻开始理解什么是 shader。想象你是一个画家，面前有一块巨大的画布，这个画布被分成了数百万个小格子(像素)。

为什么要用GPU渲染？让我们通过一个实例来理解。

假设要渲染一个 1000x1000 像素的区域：

本质上是为了提升性能，GPU 是一个专门用于图形渲染的硬件，它可以同时处理大量像素，因此比 CPU 更适合渲染图形。

在 Makepad 中，我们需要处理三种主要的坐标系统:

[-1,1]范围，提供设备无关的标准化空间，确保渲染结果在不同分辨率下保持一致。

实际屏幕像素，用于精确定位屏幕上的实际位置，直接对应物理显示设备。

[0,1]范围的纹理坐标，用于纹理映射和参数化表面， 提供独立于分辨率的相对位置。

在 Makepad 中，颜色使用 vec4 表示，包含 RGBA 四个通道:

**Examples:**

Example 1 (rust):
```rust
1// 一个最基础的 Makepad shader
2MyFirstShader = {{MyFirstShader}} {
3    // 1. 顶点着色器 - 处理位置
4    fn vertex(self) -> vec4 {
5        // 将形状放在正确的位置
6        let position = self.geom_pos * self.rect_size + self.rect_pos;
7        return self.camera_projection * vec4(position, 0.0, 1.0);
8    }
9
10    // 2. 片元着色器 - 处理颜色
11    fn pixel(self) -> vec4 {
12        // 给每个像素上色
13        return vec4(1.0, 0.0, 0.0, 1.0); // 红色
14    }
15};
```

Example 2 (yaml):
```yaml
1顶点着色阶段:                像素着色阶段:
2
3   P1●─────●P2                 ░░░░░░░░
4     │     │                   ░██████░
5     │     │         =>        ░██████░
6     │     │                   ░██████░
7   P3●─────●P4                 ░░░░░░░░
8
9 (确定形状位置)              (填充每个像素)
```

Example 3 (json):
```json
1CPU 渲染:
2- 依次处理每个像素
3- 100万个像素需要依次处理
4- 类似于用一个画笔一点点填充
5
6GPU 渲染:
7- 大量像素并行处理
8- 100万个像素可以同时处理
9- 类似于用喷枪快速覆盖
```

Example 4 (json):
```json
1CPU 渲染进度:              GPU 渲染进度:
2█░░░░░░░░░  10%          ▒▒▒▒▒▒▒▒▒▒  100%
3██░░░░░░░░  20%          (同时处理)
4███░░░░░░░  30%
5...
6██████████  100%
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/animation-system/debug-animation

**Contents:**
- #调试技巧

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/creating-an-imagegrid

**Contents:**
- #5 - 创建 ImageGrid
- #你将学到的内容
- #定义ImageGrid
- #更新App
- #增加rust代码
- #检查我们现在的进度
- #下一步

在上一步中，我们创建了一个 ImageRow 来显示一排图片，作为构建应用程序图片网格的一部分。

在这一步中，我们将把多个 ImageRow 组合到一个 ImageGrid 中，以显示一个图片网格。ImageGrid 的代码与我们为 ImageRow 编写的代码非常相似，因此我们不会详细讲解。

注意：如果你不想手动输入代码，可以在这里找到本步骤的全部代码：https://github.com/makepad/image_viewer/tree/main/step_5

让我们开始添加 ImageGrid 的定义。ImageGrid 负责垂直排列多个 ImageRow。

在 app.rs 中，添加以下代码到 live design 块中，位于 ImageRow 定义之后：

与 ImageRow 一样，在 ImageGrid 中，我们使用 PortalList 来列出其行。唯一的区别是我们使用了 flow: Down，这确保其子项从上到下排列。

现在我们已经有了一个 ImageGrid，让我们更新 App 的定义，使其显示 ImageGrid 而不是 ImageRow。

在 app.rs 中，将 live design 块中的 App 定义替换为下面这个新的定义：

这段代码定义了 ImageGrid 结构体，并为其实现了 Widget trait。这段代码和我们之前为 ImageRow 结构体编写的代码非常相似，所以我们不会详细讲解。唯一的区别是，我们不是绘制 4 个 ImageItem，而是绘制了 3 个 ImageRow。

如果一切正常，屏幕上应该会出现一个包含占位图像网格的窗口：

在这一步中，我们创建了一个 ImageGrid 来显示图像网格。

我们现在已经有了一个图像网格的初始实现。虽然它仍然非常有限 —— 总是显示相同数量的行和每行相同数量的图像，而且只显示占位图像，但这已经是一个不错的开始！

在接下来的几步中，我们将去除这些限制，让图像网格变得更加动态。

**Examples:**

Example 1 (rust):
```rust
1ImageGrid = {{ImageGrid}} {
2        <PortalList> {
3            flow: Down,
4            
5            ImageRow = <ImageRow> {}
6        }
7    }
```

Example 2 (rust):
```rust
1App = {{App}} {
2        ui: <Root> {
3            <Window> {
4                body = <View> {
5                    <ImageGrid> {}
6                }
7            }
8        }
9    }
```

Example 3 (rust):
```rust
1#[derive(Live, LiveHook, Widget)]
2pub struct ImageGrid {
3    #[deref]
4    view: View,
5}
6
7impl Widget for ImageGrid {
8    fn draw_walk(
9        &mut self,
10        cx: &mut Cx2d,
11        scope: &mut Scope,
12        walk: Walk,
13    ) -> DrawStep {
14        while let Some(item) = self.view.draw_walk(cx, scope, walk).step() {
15            if let Some(mut list) = item.as_portal_list().borrow_mut() {
16                list.set_item_range(cx, 0, 3);
17                while let Some(row_idx) = list.next_visible_item(cx) {
18                    let row = list.item(cx, row_idx, live_id!(ImageRow));
19                    row.draw_all(cx, &mut Scope::empty());
20                }
21            }
22        }
23        DrawStep::done()
24    }
25
26    fn handle_event(&mut self, cx: &mut Cx, event: &Event, scope: &mut Scope) {
27        self.view.handle_event(cx, event, scope)
28    }
29}
```

Example 4 (bash):
```bash
1cargo run --release
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/appendix/live-DSL-code-style

**Contents:**
- #Live DSL 建议编码规范
- #命名规范
  - #组件类型命名
  - #实例属性命名
  - #字段属性命名
- #缩进和空格
  - #缩进
  - #空格和换行
- #属性排序
- #组件结构

使用 snake_case 命名所有字段属性

**Examples:**

Example 1 (rust):
```rust
1// Good
2Button = {{Button}} { }
3DropDownList = {{DropDownList}} { }
4ScrollView = {{ScrollView}} { }
5
6// Bad
7button = {{button}} { }
8dropdownlist = {{dropdownlist}} { }
9scroll_view = {{scroll_view}} { }
```

Example 2 (rust):
```rust
1Dialog = {{Dialog}} {
2    // 可交互、需要引用的组件用 PascalCase
3    OkButton = <Button> { }
4    CancelButton = <Button> { }
5
6    // 布局容器用 snake_case
7    content_area = <View> {
8        header = <View> { }
9        body = <View> { }
10        footer = <View> { }
11    }
12}
```

Example 3 (rust):
```rust
1Button = {{Button}} {
2    // Good
3    background_color: #fff,
4    border_width: 1.0,
5    corner_radius: 4.0,
6
7    // Bad
8    backgroundColor: #fff,
9    BorderWidth: 1.0,
10    cornerRadius: 4.0,
11}
```

Example 4 (rust):
```rust
1Container = {{View}} {
2    width: Fill,
3    height: Fill,
4
5    Header = <View> {
6        height: 60,
7
8        Title = <Label> {
9            text: "Hello"
10        }
11    }
12}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/switching-between-views

**Contents:**
- #15 - 在视图之间切换
- #你将学到的内容
- #更新 App
- #处理事件
- #检查目前的进度
- #下一步

在上一步中，我们创建了图片浏览器（ImageBrowser），这是实现图片网格与幻灯片之间切换功能的一部分。

这一步，我们将使用 PageFlip 实现图片浏览器与幻灯片之间的视图切换。

注意：如果你不想自己敲代码，可以在这里找到本步骤的所有代码：https://github.com/makepad/image_viewer/tree/main/step_15

我们先从更新 App 的定义开始，使其使用 PageFlip。

PageFlip 是一个容器，用于包含多个组件（widget），每次只显示其中一个。容器中的每个组件称为一个页面（page），当前显示的那个页面被称为活动页面（active page）。每个页面都有一个名称，可以通过这个名称来设置当前活动页面。

在 app.rs 的 live design 块中，添加以下代码：

这段代码定义了一个包含两个页面的 PageFlip：

当前活动页面被设置为 image_browser。

接下来，我们将更新事件处理代码，以响应以下用户事件：

在 app.rs 中，替换 App 结构体上 MatchEvent trait 的 handle_actions 方法定义，使用如下代码：

这段代码和之前的几乎一样，只是我们添加了一些新的事件处理逻辑。下面我们详细讲解新加的部分。

检查幻灯片按钮是否被点击。如果是，就将翻页组件的活动页面切换到幻灯片页，并确保幻灯片覆盖层拥有键盘焦点。

检查当幻灯片覆盖层拥有键盘焦点时是否按下了 Esc 键。如果是，就将翻页组件的活动页面切换回图片浏览器。

如果一切正常，你应该会看到和之前一样的图片网格，上方带有一个“幻灯片”按钮：

不过这次，点击幻灯片按钮应该会切换到幻灯片视图：

按下 Escape 键时，应该会切换回图片网格视图：

现在我们已经能够在图片网格和幻灯片之间切换了。接下来的几个步骤中，我们将添加图片过滤的功能。

**Examples:**

Example 1 (rust):
```rust
1App = {{App}} {
2        placeholder: (PLACEHOLDER),
3
4        ui: <Root> {
5            <Window> {
6                body = <View> {
7                    page_flip = <PageFlip> {
8                        active_page: image_browser,
9
10                        image_browser = <ImageBrowser> {}
11                        slideshow = <Slideshow> {}
12                    }
13                }
14            }
15        }
16    }
```

Example 2 (rust):
```rust
1fn handle_actions(&mut self, cx: &mut Cx, actions: &Actions) {
2        if self.ui.button(id!(slideshow_button)).clicked(&actions) {
3            self.ui
4                .page_flip(id!(page_flip))
5                .set_active_page(cx, live_id!(slideshow));
6            self.ui.view(id!(slideshow.overlay)).set_key_focus(cx);
7        }
8
9        if self.ui.button(id!(navigate_left)).clicked(&actions) {
10            self.navigate_left(cx);
11        }
12        if self.ui.button(id!(navigate_right)).clicked(&actions) {
13            self.navigate_right(cx);
14        }
15
16        if let Some(event) =
17            self.ui.view(id!(slideshow.overlay)).key_down(&actions)
18        {
19            match event.key_code {
20                KeyCode::Escape => self
21                    .ui
22                    .page_flip(id!(page_flip))
23                    .set_active_page(cx, live_id!(image_browser)),
24                KeyCode::ArrowLeft => self.navigate_left(cx),
25                KeyCode::ArrowRight => self.navigate_right(cx),
26                _ => {}
27            }
28        }
29    }
```

Example 3 (rust):
```rust
1if self.ui.button(id!(slideshow_button)).clicked(&actions) {
2        self.ui
3            .page_flip(id!(page_flip))
4            .set_active_page(cx, live_id!(slideshow));
5        self.ui.view(id!(slideshow.overlay)).set_key_focus(cx);
6    }
```

Example 4 (rust):
```rust
1KeyCode::Escape => self
2            .ui
3            .page_flip(id!(page_flip))
4            .set_active_page(cx, live_id!(image_browser)),
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/start/makepad-studio

**Contents:**
- #Makepad Studio

Makepad Studio 提供 IDE ，供开发者和设计人员使用，而 Makepad Framework 则是 底层UI 框架。

Makepad Studio 很重要，因为它决定了开发者的用户体验。

在 UI 重度项目中，最大的成本因素来自于设计师与开发人员的脱节。设计师需要将它们的想法传达给产品经理，产品经理需要与开发人员沟通。开发人员需要向设计师传达限制。持续反馈需要很多东西，并且信息会缺失。

在未来智能时代，设计师需要给 AI 传达更多信息。随着 Makepad Studio 创作工具的成熟，设计师会越来越有能力构建复杂而精细的界面。

目前 Makepad Studio 已经基本成型，您可以在 https://crates.io/crates/makepad-studio 下载。

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/layout-system/attribute-inheritance-relation

**Contents:**
- #属性继承和覆盖机制
- #Walk 属性不继承
- #Layout 属性部分继承

**Examples:**

Example 1 (rust):
```rust
1// Layout 属性继承示例
2Parent = <View> {
3    layout: {
4        flow: Down,
5        align: {x: 0.5} // 所有子组件默认水平居中
6    },
7
8    <Child1> {
9        // 继承父级的 flow 和 align
10    }
11
12    <Child2> {
13        layout: {
14            align: {x: 1.0} // 覆盖继承的对齐方式，改为右对齐
15        }
16    }
17}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/split-module

**Contents:**
- #19 - 拆分模块
- #你将学到的内容
- #创建组件模块
- #创建组件
  - #创建 ImageItem 组件
  - #创建 ImageRow 和 ImageGrid 组件
- #修改根组件内容
- #检查到目前为止的进度
- #下一步

在上一步中，我们实现了一个模态窗，并成功的把模态窗的触发与关闭绑定到了对应的按钮/事件上。同时，我们的应用代码都在 app.rs 下，显得十分冗长，难以维护。在这一步中，我们将会将应用拆成几个不同的组件，并且放入不同的模块中。

注意: 如果你不想自己敲代码，可以在这里找到本步骤的完整代码： https://github.com/Project-Robius-China/image-viewer-workshop

首先，我们在项目源码 src 文件夹下，创建新的文件夹 components, 并且创建根文件 mod.rs:

此时，整个组件模块需要被注册进 Makepad 的 live 系统中，在根组件 app.rs 中，将 app.rs 中的 LiveRegister 的实现代码改成：

这样，我们定义的组件模块就已经成功注册进了 Makepad 项目中。自定义的组件，只需要在组件模块下完成注册，就可以在项目中直接使用。

在之前的内容中，我们知道整个应用定义了三个组件：ImageItem, ImageRow, ImageGrid。所有这里我们创建组件的目的就是将这三个组件从 app.rs 中抽离出来，做成单独的组件。

在原 app.rs 下，ImageItem 定义如下：

现在我们要把 ImageItem 单独成一个独立的组件，在 components 文件夹下，创建一个新的文件，命名为 image_item.rs, 并将下面的代码，加入带新的文件中：

在之前 第 4 步 和 第 5 步 中已经详细讲解了关于组件定义的相关内容，这里只是把相关组件内容抽取到单独的模块，这里不加以赘述。

之后，在 mod.rs 文件中，注册模块：

同时，我们要把定义的这个 ImageItem 组件注册进入 Makepad 的 live 系统中，我们要在 mod.rs 文件下加入：

这一步，将定义的 ImageItem 组件成功注册进入了 Makepad 项目中，我们可以在应用中引用并且使用该组件。

同样的，在 components 文件夹下，创建一个新的文件，命名为 ImageRow.rs:

引入 ImageItem 组件，由于 ImageItem 完成了注册，可以在项目中直接引入。

同样的，在 components 文件夹下，创建一个新的文件，命名为 ImageGrid.rs:

完成对组件 ImageGrid 的定义。最后在模块 mod.rs 下，声明注册这两个组件:

由于我们将组件拆分到了组件模块中，在 app.rs 的 live_design! 中，我们要将最终使用的组件引入：

其他内容我们没有修改，所以最终 app.rs 的 live_design! 中:

在这一步中，我们将 app.rs 中的组件进行拆分成了不同的模块。下一步，我们会为程序增加一个扩展，当在网格模式下点击图片时候，自动切换到 幻灯片放映 模式

**Examples:**

Example 1 (unknown):
```unknown
1your-app/
2|—— resources
3├── Cargo.lock
4├── Cargo.toml
5├── src
6|   |—— components
7|   |     |—— mod.rs
8│   ├── app.rs
9│   ├── lib.rs
10│   └── main.rs
```

Example 2 (rust):
```rust
1impl LiveRegister for App {
2    fn live_register(cx: &mut Cx) {
3        makepad_widgets::live_design(cx);
4        crate::components::live_design(cx);
5    }
6}
```

Example 3 (rust):
```rust
1ImageItem = <View> {
2        width: 256,
3        height: 256,
4
5        image = <Image> {
6            width: Fill,
7            height: Fill,
8            fit: Biggest,
9            source: (PLACEHOLDER)
10        }
11    }
```

Example 4 (rust):
```rust
1use makepad_widgets::*;
2
3live_design! {
4    use link::widgets::*;
5
6    PLACEHOLDER = dep("crate://self/resources/Rust.jpg");
7
8    pub ImageItem = {{ImageItem}} {
9        width: 256,
10        height: 256,
11        image_index: 0,
12
13        image = <Image> {
14            width: Fill,
15            height: Fill,
16            fit: Biggest,
17            source: (PLACEHOLDER)
18        }
19    }
20}
21
22#[derive(Live, LiveHook, Widget)]
23pub struct ImageItem {
24    #[deref]
25    view: View,
26}
27
28impl Widget for ImageItem {
29    fn handle_event(&mut self, cx: &mut Cx, event: &Event, scope: &mut Scope) {
30        self.view.handle_event(cx, event, scope);
31    }
32
33    fn draw_walk(&mut self, cx: &mut Cx2d, scope: &mut Scope, walk: Walk) -> DrawStep {
34        self.view.draw_walk(cx, scope, walk)
35    }
36}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/creating-a-slideshowoverlay

**Contents:**
- #9 - 创建 SlideshowOverlay
- #你将学到的内容
- #定义 SlideshowOverlay
  - #处理键盘焦点
- #更新应用程序
- #检查到目前为止的进展
- #下一步

在上一步中，我们创建了两个 SlideshowNavigateButtons，作为为我们的应用构建幻灯片播放功能的一部分。

在这一步，我们将创建 SlideshowOverlay。

注意：如果你不想手动输入代码，可以在这里找到本步骤的全部代码：https://github.com/makepad/image_viewer/tree/main/step_9

我们先来添加 SlideshowOverlay 的定义。SlideshowOverlay 是一个透明的容器，位于 Image 之上，包含两个 SlideshowNavigateButtons。

在 app.rs 中，添加以下代码到 live design 块里，紧跟在 SlideshowNavigateButton 的定义之后：

这段代码定义了一个 SlideshowOverlay，具有以下属性：

SlideshowOverlay 包含两个与上一步在 App 中添加的 SlideshowNavigateButtons 相同的按钮，不过这次在它们之间加入了一个 Filler：

Filler 是一个辅助控件，用于填充容器中未使用的空间。这保证了第一个 SlideshowNavigateButton 被布局在左侧，而第二个则被布局在右侧。

稍微展望一下，在后面的步骤中，我们将讨论如何为幻灯片放映处理用户事件。到那个时候，我们希望 SlideshowOverlay 负责处理键盘事件，因此现在讲讲键盘焦点是个好时机。

在 Makepad 中，控件只有在拥有键盘焦点时才响应键盘事件——也就是说，它是接收键盘输入的活动控件。大多数内置控件，比如 View，只有在获得键盘焦点后才会响应按键。

大多数内置控件在被点击时会自动获得键盘焦点——但前提是控件本身被直接点击。如果是它的子控件被点击，则焦点由该子控件获得，而不是父控件本身。

对于 SlideshowOverlay，我们不希望这样：即使点击的是其中的 SlideshowNavigateButton，我们也希望 SlideshowOverlay 能获得键盘焦点。因此，我们之前添加了以下属性：

既然我们已经有了一个 SlideshowOverlay，现在让我们更新 App 的定义，使其显示 SlideshowOverlay，而不是两个 SlideShowNavigateButtons。

在 app.rs 文件中，用下面的代码替换 live design 块中 App 的定义：

如果一切正常，你应该会看到和之前一样的两个按钮，不过这次一个按钮位于左侧，另一个按钮位于右侧：

当然，你真正看到的是覆盖层（overlay），但由于覆盖层本身是透明的，所以你只看到按钮。

在这一步中，我们创建了 SlideshowOverlay。在下一步中，我们将创建 Slideshow。

**Examples:**

Example 1 (rust):
```rust
1SlideshowOverlay = <View> {
2        height: Fill,
3        width: Fill,
4        cursor: Arrow,
5        capture_overload: true,
6
7        navigate_left = <SlideshowNavigateButton> {}
8        <Filler> {}
9        navigate_right = <SlideshowNavigateButton> {}
10    }
```

Example 2 (rust):
```rust
1navigate_left = <SlideshowNavigateButton> {
2        draw_icon: { svg_file: (LEFT_ARROW) }
3    }
4    <Filler> {}
5    navigate_right = <SlideshowNavigateButton> {
6        draw_icon: { svg_file: (RIGHT_ARROW) }
7    }
```

Example 3 (rust):
```rust
1App = {{App}} {
2        ui: <Root> {
3            <Window> {
4                body = <View> {
5                    <SlideshowOverlay> {}
6                }
7            }
8        }
9    }
```

Example 4 (bash):
```bash
1cargo run --release
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/shading-language/shader-animation

**Contents:**
- #Shader 动画
- #MPSL 中的动画状态机
- #MPSL 中的插值与过渡
- #MPSL 中的高级动画技巧
- #正在输入动画完整代码
  - #对上述代码的一些可能的扩展与优化

在 MPSL 中，我们可以通过几种方式来实现动画效果。让我们通过「正在输入」动画的例子来理解：

正在输入动画：就是某些 IM 在用户输入时候提示「xxx 正在输入 ... 」，后面跟随几个小球依次闪现或者依次弹跳的动画。

MPSL 提供了多种内置的插值函数，我们可以在着色器中直接使用：

比如，当你想要一个按钮在被点击时平滑地改变颜色，或者让一个菜单优雅地滑入滑出，都需要使用插值来创造流畅的动画效果。

来自 Robrix 项目：https://github.com/project-robius/robrix/blob/main/src/shared/typing_animation.rs

**Examples:**

Example 1 (rust):
```rust
1draw_bg: {
2    // 1. 通过 uniform 变量来控制动画参数
3    uniform freq: 5.0,        // 动画频率
4    uniform phase_offset: 102.0,  // 相位差
5    uniform dot_radius: 1.5,   // 点半径
6
7    // 2. 在 pixel 着色器中实现动画逻辑
8    fn pixel(self) -> vec4 {
9        let sdf = Sdf2d::viewport(self.pos * self.rect_size);
10
11        // 计算动画位置
12        let amplitude = self.rect_size.y * 0.22;  // 振幅
13        let center_y = self.rect_size.y * 0.5;    // 中心位置
14
15        // 通过 sin 函数创建周期运动
16        // self.time 是由 Rust 端传入的时间参数
17        let y1 = amplitude * sin(self.time * self.freq) + center_y;
18        let y2 = amplitude * sin(self.time * self.freq + self.phase_offset) + center_y;
19        let y3 = amplitude * sin(self.time * self.freq + self.phase_offset * 2.0) + center_y;
20
21        // 使用 SDF 绘制圆点
22        sdf.circle(self.rect_size.x * 0.25, y1, self.dot_radius);
23        sdf.fill(self.color);
24
25        sdf.circle(self.rect_size.x * 0.5, y2, self.dot_radius);
26        sdf.fill(self.color);
27
28        sdf.circle(self.rect_size.x * 0.75, y3, self.dot_radius);
29        sdf.fill(self.color);
30
31        return sdf.result;
32    }
33}
```

Example 2 (rust):
```rust
1fn pixel(self) -> vec4 {
2    // 1. 线性插值
3    let linear_value = mix(start_value, end_value, self.time);
4
5    // 2. 平滑插值
6    let smooth_value = smoothstep(0.0, 1.0, self.time);
7
8    // 3. 自定义缓动函数
9    fn custom_ease(t: float) -> float {
10        return t * t * (3.0 - 2.0 * t); // 平滑的 S 型曲线
11    }
12
13    // 4. 贝塞尔曲线插值
14    let bezier_value = Pal::bezier(
15        self.time,  // 时间参数
16        vec2(0.0, 0.0),  // P0
17        vec2(0.42, 0.0), // P1
18        vec2(0.58, 1.0), // P2
19        vec2(1.0, 1.0)   // P3
20    );
21}
```

Example 3 (rust):
```rust
1draw_bg: {
2    // 1. 使用噪声函数创建随机运动
3    fn noise_movement(pos: vec2, time: float) -> float {
4        return sin(pos.x * 10.0 + time) * cos(pos.y * 10.0 + time) * 0.5;
5    }
6
7    // 2. 使用极坐标实现旋转效果
8    fn rotate_point(p: vec2, angle: float) -> vec2 {
9        let s = sin(angle);
10        let c = cos(angle);
11        return vec2(
12            p.x * c - p.y * s,
13            p.x * s + p.y * c
14        );
15    }
16
17    fn pixel(self) -> vec4 {
18        let sdf = Sdf2d::viewport(self.pos * self.rect_size);
19
20        // 3. 组合多个动画效果
21        let pos = self.pos - vec2(0.5);  // 中心化坐标
22        let rot_pos = rotate_point(pos, self.time);  // 旋转
23        let noise = noise_movement(rot_pos, self.time);  // 添加噪声
24
25        // 4. 使用 SDF 实现形状变形
26        let radius = 0.2 + noise * 0.1;
27        sdf.circle(rot_pos.x, rot_pos.y, radius);
28
29        // 5. 颜色动画
30        let color = mix(
31            #f00,  // 红色
32            #0f0,  // 绿色
33            sin(self.time) * 0.5 + 0.5
34        );
35
36        sdf.fill(color);
37        return sdf.result;
38    }
39}
```

Example 4 (rust):
```rust
1use makepad_widgets::*;
2
3live_design! {
4    import makepad_widgets::base::*;
5    import makepad_widgets::theme_desktop_dark::*;
6    import makepad_draw::shader::std::*;
7
8    TypingAnimation = {{TypingAnimation}} {
9        width: 24,
10        height: 12,
11        flow: Down,
12        show_bg: true,
13        draw_bg: {
14            color: #x000
15            uniform freq: 5.0,  // Animation frequency
16            uniform phase_offset: 102.0, // Phase difference
17            uniform dot_radius: 1.5, // Dot radius
18            fn pixel(self) -> vec4 {
19                let sdf = Sdf2d::viewport(self.pos * self.rect_size);
20                let amplitude = self.rect_size.y * 0.22;
21                let center_y = self.rect_size.y * 0.5;
22                // Create three circle SDFs
23                sdf.circle(
24                    self.rect_size.x * 0.25,
25                    amplitude * sin(self.time * self.freq) + center_y,
26                    self.dot_radius
27                );
28                sdf.fill(self.color);
29                sdf.circle(
30                    self.rect_size.x * 0.5,
31                    amplitude * sin(self.time * self.freq + self.phase_offset) + center_y,
32                    self.dot_radius
33                );
34                sdf.fill(self.color);
35                sdf.circle(
36                    self.rect_size.x * 0.75,
37                    amplitude * sin(self.time * self.freq + self.phase_offset * 2) + center_y,
38                    self.dot_radius
39                );
40                sdf.fill(self.color);
41                return sdf.result;
42            }
43        }
44    }
45}
46
47#[derive(Live, LiveHook, Widget)]
48pub struct TypingAnimation {
49    #[deref] view: View,
50    #[live] time: f32,
51    #[rust] next_frame: NextFrame,
52    #[rust] is_play: bool,
53}
54impl Widget for TypingAnimation {
55    fn handle_event(&mut self, cx: &mut Cx, event: &Event, scope: &mut Scope) {
56        if let Some(ne) = self.next_frame.is_event(event) {
57            // ne.time 是以秒为单位的时间增量
58            self.time += ne.time as f32;
59            self.time = (self.time.round() as u32 % 360) as f32;
60            self.redraw(cx);
61            if !self.is_play {
62                return
63            }
64            // 请求下一帧
65            self.next_frame = cx.new_next_frame();
66        }
67
68        self.view.handle_event(cx, event, scope);
69    }
70
71    fn draw_walk(&mut self, cx: &mut Cx2d, scope: &mut Scope, walk: Walk) -> DrawStep {
72        self.view.draw_walk(cx, scope, walk)
73    }
74}
75
76
77impl TypingAnimationRef {
78    /// Starts animation of the bouncing dots.
79    pub fn animate(&self, cx: &mut Cx) {
80        if let Some(mut inner) = self.borrow_mut() {
81            inner.is_play = true;
82            inner.next_frame = cx.new_next_frame();
83        }
84    }
85    /// Stops animation of the bouncing dots.
86    pub fn stop_animation(&self) {
87        if let Some(mut inner) = self.borrow_mut() {
88            inner.is_play = false;
89        }
90    }
91}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/shading-language/MPSL-shader

**Contents:**
- #MPSL Shader 基本语法
- #MPSL 语法和 Rust 语法的区别
  - #类型系统的区别
  - #内存管理的区别
  - #函数定义和使用
  - #变量作用域
  - #控制流结构
  - #特殊功能支持
  - #错误处理
- #MPSL 基本语法

MPSL 着色器语言与 Rust 语言的写法类似，但它们也有一些本质区别。

MPSL 的类型系统更简单且专注于图形计算：

MPSL 主要关注图形相关的基本类型：float、vec2、vec3、vec4、mat4等，而不支持 Rust 中的复杂数据结构如 Vec、HashMap 等。这种简化是为了适应 GPU 的计算特点。

Rust 的所有权系统、默认不可变等在 MPSL 中完全不存在：

MPSL 不需要考虑内存管理，因为着色器程序的生命周期是由 GPU 执行过程控制的。

MPSL 的函数系统更简单，主要围绕着顶点和像素着色：

MPSL 的 self 参数是特殊的，它包含了着色器的上下文信息，这与 Rust 的 self 概念有很大不同。

这些特殊的变量类型反映了图形渲染管线的特点，在 Rust 中并不存在这样的概念。

MPSL 不支持 match、loop 等 Rust 的高级控制流结构，主要是因为 GPU 执行模型的限制。

这些内置的图形处理功能是 MPSL 的特色，在 Rust 中通常需要依赖外部库。

MPSL 没有 Result 和 Option 类型，也不需要错误处理机制，因为着色器程序要求必须能够正确执行。

MPSL 的着色器程序主要由以下部分组成：

MPSL 提供了强大的 SDF 功能用于形状绘制：

**Examples:**

Example 1 (rust):
```rust
1// Rust 的类型系统
2let x: i32 = 42;
3let y: f64 = 3.14;
4let v: Vec<i32> = vec![1, 2, 3];
5
6// MPSL 的类型系统
7let x: float = 42.0;
8let v: vec4 = vec4(1.0, 0.0, 0.0, 1.0);
```

Example 2 (rust):
```rust
1// Rust 中的所有权和借用
2let mut s = String::from("hello");
3let r = &mut s;  // 可变借用
4r.push_str(" world");
5
6// MPSL 中简单的值传递，默认可变
7let color = vec4(1.0, 0.0, 0.0, 1.0);
8let modified = color * 0.5;  // 直接操作，没有所有权概念
```

Example 3 (rust):
```rust
1// Rust 函数定义
2fn calculate_sum(a: i32, b: i32) -> i32 {
3    a + b
4}
5
6// MPSL 着色器函数
7fn vertex(self) -> vec4 {
8    // 必须返回裁剪空间坐标
9    return self.transform_position();
10}
11
12fn pixel(self) -> vec4 {
13    // 必须返回颜色值
14    return self.color;
15}
```

Example 4 (rust):
```rust
1// MPSL 特有的变量声明
2instance border_width: float  // 实例变量
3varying pos: vec2            // 顶点到像素着色器传递的变量
4uniform time: float          // 统一变量
5
6// Rust 中的变量声明
7let border_width: f32 = 1.0;  // 普通变量
8static TIME: f32 = 0.0;       // 静态变量
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/creating-a-new-cargo-package

**Contents:**
- #1 - 创建一个新的 Cargo 包
- #本步骤你将学到：
- #运行 Cargo
- #检查目前的进展

构建任何 Rust 应用的第一步都是创建一个新的包。

在 Cargo 中，一个包（package）是一个或多个 crate 的集合。Crate 有两种类型：二进制 crate（binary crate） 和 库 crate（library crate）。二进制 crate 是一个程序，可以编译为可执行文件直接运行。而库 crate 不会被编译为可执行文件，而是用于在多个项目之间共享功能。

因为我们要构建的是一个应用程序，所以我们需要创建一个只包含一个二进制 crate 的 Cargo 包。我们将在下面创建这个包。

注意： 如果你不想手动输入，可以在以下地址找到本步骤的完整代码： https://github.com/makepad/image_viewer/tree/main/step_1

要创建一个包含单个二进制 crate 的 Cargo 包，请先进入你希望创建项目的目录，然后在终端中运行以下命令：

这将会创建一个名为 image_viewer 的新目录。进入该目录：

如果一切正常，你的终端中应该会显示如下内容：

**Examples:**

Example 1 (bash):
```bash
1cargo new image_viewer
```

Example 2 (bash):
```bash
1cd image_viewer
```

Example 3 (bash):
```bash
1cargo run --release
```

Example 4 (bash):
```bash
1Compiling image-viewer v0.1.0 (/your/project_dir/image_viewer)
2    Finished `release` profile [optimized] target(s) in 0.19s
3     Running `/your/project/dir/image_viewer/release/image-viewer`
4Hello, world!
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/layout-system/debug-layout

**Contents:**
- #布局调试

当怀疑存在布局循环依赖时，您可以使用 debug 视图

**Examples:**

Example 1 (rust):
```rust
1MyView = <View> {
2    debug: true,  // 显示布局边界
3}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/live-DSL/basic-syntax

**Contents:**
- #语法基础
- #属性
  - #字段属性（Field Properties）
  - #实例属性（Instance Properties）
  - #二者关系

属性用于数据绑定。在 Live DSL 中属性主要分为两类：

使用冒号(:)定义，通常用于定义组件的固有属性：

使用等号(=)定义，通常用于定义可变或动态的属性：

实例属性在运行时会创建实际的组件对象，可以通过模板 ID 来查找和访问。而字段属性会直接影响组件自身的属性值。

理解这两种属性的区别和用途对于正确使用 Live DSL 构建界面非常重要。

字段属性用于配置，实例属性用于组织组件结构。

**Examples:**

Example 1 (rust):
```rust
1MyWidget = {{Widget}} {
2    // 字段属性示例
3    color: #f00,         // 颜色属性
4    width: 100,         // 数值属性
5    visible: true,      // 布尔属性
6    name: "button",     // 字符串属性
7    padding: {          // 嵌套字段属性
8        left: 10,
9        right: 10
10    }
11}
```

Example 2 (rust):
```rust
1Container = {{View}} {
2    // 实例属性示例
3    Button1 = <Button> {  // 创建 Button 实例
4        width: 100,
5        label: "Click me"
6    }
7
8    Panel = <View> {     // 创建 View 实例
9        flow: Down,
10        Button2 = <Button> {
11            width: 200
12        }
13    }
14}
```

Example 3 (rust):
```rust
1MyComponent = {{Component}} {
2    // 字段属性 - 定义组件本身的属性
3    width: Fill,
4    color: #f00,
5
6    // 实例属性 - 创建子组件
7    Header = <View> {
8        height: 50
9    }
10
11    Content = <View> {
12        // 字段属性
13        flow: Down,
14        spacing: 10,
15
16        // 嵌套实例属性
17        button1 = <Button> {
18            label: "OK"
19        }
20        button2 = <Button> {
21            label: "Cancel"
22        }
23    }
24}
25
26// 字段属性会映射到结构体字段
27#[derive(Live)]
28pub struct Component {
29    #[live] pub width: Size,    // 对应 width: Fill
30    #[live] pub color: Vec4,    // 对应 color: #f00
31}
32
33// 实例属性会创建新的组件实例
34impl Widget for Component {
35    fn handle_event(&mut self, cx: &mut Cx, event: &Event) {
36        // 可以通过 button() 访问子实例
37        let button = self.button(id!(button1));
38        button.handle_event(cx, event);
39    }
40}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/filtering-images

**Contents:**
- #17 - 过滤图片
- #更新 State 结构体
  - #更新 State 结构体的 Default Trait 实现
  - #更新 State 结构体的 num_images 方法
- #添加辅助方法
  - #添加 filter_image_paths 方法
  - #更新 set_current_image 方法
  - #更新 load_image_paths 方法
- #更新绘制代码
- #处理事件

在上一步中，我们在图片网格上方的菜单栏添加了一个搜索框，使得基于查询字符串过滤图片成为可能。

这一步，我们将更新应用的状态，实现根据搜索框查询过滤图片。

注意：如果你不想自己敲代码，可以在这里找到本步骤的完整代码：https://github.com/makepad/image_viewer/tree/main/step_17

我们先从更新 State 结构体开始，加入我们需要的状态以便进行图片过滤。

在 app.rs 中，找到 State 结构体的定义，将其替换为下面的代码：

这段内容扩展了 State 结构体，增加了如下字段：

这里的思路是增加一层间接引用。以前，我们直接遍历或索引 image_paths；现在，我们改为遍历或索引 filtered_image_idxs，然后用这些索引去访问 image_paths。这样我们就能定义一个过滤后的图片子集进行显示，同时保持原始图片列表不变。

注意：为了减少混淆，我们采用以下命名规范：

我们还需要更新 State 结构体的 Default trait 实现，以反映新增的字段。

在 app.rs 中，找到 State 结构体对应的 Default trait 实现，替换为下面的代码：

最后，我们需要更新 State 结构体的 num_images 方法，使其返回 filtered_image_idxs 中元素的数量（因为我们将绘制这些过滤后的图片数量）。

在 app.rs 中，找到 State 结构体的 num_images 方法定义，替换为下面的代码：

现在我们已经拥有了过滤图片所需的全部信息。

为了让后续代码更易编写，我们将为 App 结构体添加一些辅助函数，同时更新部分已有函数。

我们先定义一个 filter_image_paths 方法。该方法将根据查询字符串过滤图片路径列表。

在 app.rs 中，找到 App 结构体的 impl 块，添加以下代码：

filter_image_paths 方法的功能说明：

接下来，我们需要更新 set_current_image 方法，使其针对过滤后的图片进行操作。

请在 app.rs 中，找到 App 结构体的 set_current_image 方法定义，替换为下面的代码：

这正是我们之前所说的：不再直接索引 image_paths，而是先索引 filtered_image_idxs，再用它们的值去索引 image_paths。

这个改动的最终效果是，幻灯片放映只会显示经过过滤后的图片。

接下来，我们需要更新 load_image_paths 方法，使其在加载图片路径后执行图片过滤。

请在 app.rs 中，找到 App 结构体的 load_image_paths 方法定义，替换为下面的代码：

这样做的目的是：每当图片列表更新时，都需要重新执行过滤。由于当前显示的图片基于过滤后的列表，因此设置当前图片的操作已在 filter_image_paths 方法中完成，这里就不再需要单独设置了。

请在 app.rs 中，找到 ImageRow 结构体对应的 Widget trait 实现中的 draw_walk 方法定义，替换为下面的代码：

代码虽然比较多，但这里其实只有一个地方发生了变化：

这和我们之前在 set_current_image 方法里做的一样：不再直接索引 image_paths，而是先通过 filtered_image_idxs 索引，再用该索引去访问 image_paths。

这个改动的最终效果是，图片网格只显示过滤后的图片。

接下来，我们将更新事件处理代码，来响应用户在搜索框中搜索图片的行为。

在 app.rs 下，App 结构体中，找到 handle_actions 方法，加入以下代码:

这段代码的含义是，当组件 TextInput 中的内容发生变化时，我们获取到其中的内容 query，并且调用我们定义好的 filter_image_paths 来找到对应的图片。

如果一切正常，你现在应该可以通过在顶部的搜索框中输入内容来过滤图片了：

至此，我们已经完成了整个应用程序的所有功能。接下来，我们会扩展一些内容，为我们的应用程序增加一些交互。

**Examples:**

Example 1 (rust):
```rust
1#[derive(Debug)]
2pub struct State {
3    image_paths: Vec<PathBuf>,
4    filtered_image_idxs: Vec<usize>,
5    max_images_per_row: usize,
6    current_image_idx: Option<usize>,
7}
```

Example 2 (rust):
```rust
1impl Default for State {
2    fn default() -> Self {
3        Self {
4            image_paths: Vec::new(),
5            filtered_image_idxs: Vec::new(),
6            images_per_row: 4,
7            current_image_idx: None,
8        }
9    }
10}
```

Example 3 (rust):
```rust
1fn num_images(&self) -> usize {
2        self.filtered_image_idx.len()
3    }
```

Example 4 (rust):
```rust
1pub fn filter_image_paths(&mut self, cx: &mut Cx, query: &str) {
2        self.state.filtered_image_idxs.clear();
3        for (image_idx, image_path) in self.state.image_paths.iter().enumerate()
4        {
5            if image_path.to_str().unwrap().contains(&query) {
6                self.state.filtered_image_idxs.push(image_idx);
7            }
8        }
9        if self.state.filtered_image_idxs.is_empty() {
10            self.set_current_image(cx, None);
11        } else {
12            self.set_current_image(cx, Some(0));
13        }
14    }
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/introduction

**Contents:**
- #0 - 简介
- #教程准备工作
- #Linux 的额外设置

在本教程中，我们将使用 Makepad 构建一个图像查看器应用。

我们的图像查看器应用将支持图像网格视图和幻灯片视图，并且可以在两者之间切换。到本教程结束时，图像网格视图将如下所示：

此外，我们的应用还将提供基于查询字符串的图像筛选功能。

本教程的目标是帮助我们熟悉 Makepad 这个 UI 框架。在构建应用的过程中，我们将接触到开发 Makepad 应用的大部分核心概念。完成本教程后，你应该对如何构建属于自己的 Makepad 应用有一个比较清晰的认识。

Makepad 是使用 Rust 编写的，因此我们需要确保系统中已经安装了 Rust。推荐的安装方式是使用 Rust 官方的工具链安装器 Rustup。你可以在终端中运行以下命令来安装，并按照屏幕上的提示进行操作：

在 Windows 和 Mac 上，Makepad 可以直接运行。但在 Linux 上，我们需要先安装一些依赖项。

如果你使用的是 Ubuntu，可以通过在终端中运行以下命令来安装所需依赖：

**Examples:**

Example 1 (bash):
```bash
1curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Example 2 (bash):
```bash
1apt-get install clang
2apt-get install libaudio-dev
3apt-get install libpulse-dev
4apt-get install libx11-dev
5apt-get install libxcursor-dev
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/animation-system/example-animation

**Contents:**
- #完整动画实例
- #使用方法

**Examples:**

Example 1 (rust):
```rust
1use makepad_widgets::*;
2
3live_design! {
4    import makepad_widgets::base::*;
5    import makepad_widgets::theme_desktop_dark::*;
6    import crate::shared::styles::*;
7
8    ANIMATION_DURATION = 0.65
9    DEFAULT_SWING_TOP = 10.0
10    DEFAULT_SWING_BOTTOM = 3.0
11
12    // 1. Set the width and height to the same value.
13    // 2. Set the radius to half of the width/height.
14    EllipsisDot = <CircleView> {
15        width: 3
16        height: 3
17        draw_bg: {
18            radius: 1.5
19            color: (TYPING_NOTICE_TEXT_COLOR)
20        }
21    }
22
23    TypingAnimation = {{TypingAnimation}} {
24        width: Fit,
25        height: Fit,
26
27        // 添加可配置的动画参数
28        swing_amplitude: (DEFAULT_SWING_TOP), // 小球摆动的最大高度
29        swing_base: (DEFAULT_SWING_BOTTOM),   // 小球摆动的基础高度
30        animation_duration: (ANIMATION_DURATION), // 动画周期
31
32        flow: Down,
33        align: {x: 0.0, y: 0.5},
34
35        content = <View> {
36            width: Fit,
37            height: Fit,
38            spacing: 2,
39            circle1 = <EllipsisDot> {}
40            circle2 = <EllipsisDot> {}
41            circle3 = <EllipsisDot> {}
42        }
43
44        animator: {
45            circle1 = {
46                default: down,
47                down = {
48                    redraw: true,
49                    from: {all: Forward {duration: (ANIMATION_DURATION * 0.5)}}
50                    ease: InOutQuad
51                    apply: {content = { circle1 = { margin: {top: (DEFAULT_SWING_TOP)} }}}
52                }
53                up = {
54                    redraw: true,
55                    from: {all: Forward {duration: (ANIMATION_DURATION * 0.5)}}
56                    ease: InOutQuad
57                    apply: {content = { circle1 = { margin: {top: (DEFAULT_SWING_BOTTOM)} }}}
58                }
59            }
60
61            circle2 = {
62                default: down,
63                down = {
64                    redraw: true,
65                    from: {all: Forward {duration: (ANIMATION_DURATION * 0.5)}}
66                    ease: InOutQuad
67                    apply: {content = { circle2 = { margin: {top: (DEFAULT_SWING_TOP)} }}}
68                }
69                up = {
70                    redraw: true,
71                    from: {all: Forward {duration: (ANIMATION_DURATION * 0.5)}}
72                    ease: InOutQuad
73                    apply: {content = { circle2 = { margin: {top: (DEFAULT_SWING_BOTTOM)} }}}
74                }
75            }
76
77            circle3 = {
78                default: down,
79                down = {
80                    redraw: true,
81                    from: {all: Forward {duration: (ANIMATION_DURATION * 0.5)}}
82                    ease: InOutQuad
83                    apply: {content = { circle3 = { margin: {top: (DEFAULT_SWING_TOP)} }}}
84                }
85                up = {
86                    redraw: true,
87                    from: {all: Forward {duration: (ANIMATION_DURATION * 0.5)}}
88                    ease: InOutQuad
89                    apply: {content = { circle3 = { margin: {top: (DEFAULT_SWING_BOTTOM)} }}}
90                }
91            }
92        }
93    }
94}
95
96#[derive(Live, Widget)]
97pub struct TypingAnimation {
98    #[deref] view: View,
99    #[animator] animator: Animator,
100
101    #[live(0.65)] animation_duration: f64,
102    #[live(10.0)] swing_amplitude: f64,  // 摆动幅度
103    #[live(3.0)] swing_base: f64,       // 基础高度
104
105    #[rust] next_frame: Option<NextFrame>,
106    #[rust] animation_start_time: f64,
107    #[rust] current_animated_dot: CurrentAnimatedDot,
108
109    #[rust] initialized: bool, // 添加初始化标志
110    #[rust] is_animating: bool, // 添加动画状态标志
111}
112
113impl LiveHook for TypingAnimation {
114    fn after_apply(&mut self, cx: &mut Cx, apply: &mut Apply, index: usize, nodes: &[LiveNode]) {
115        // 在组件完全应用后初始化
116        if !self.initialized {
117            self.initialized = true;
118        }
119    }
120}
121
122#[derive(Copy, Clone, Default)]
123enum CurrentAnimatedDot {
124    #[default]
125    Dot1,
126    Dot2,
127    Dot3,
128}
129impl CurrentAnimatedDot {
130    fn next(&self) -> Self {
131        match self {
132            Self::Dot1 => Self::Dot2,
133            Self::Dot2 => Self::Dot3,
134            Self::Dot3 => Self::Dot1,
135        }
136    }
137}
138
139impl Widget for TypingAnimation {
140    fn handle_event(&mut self, cx: &mut Cx, event: &Event, scope: &mut Scope) {
141        // 先检查初始化
142        if self.initialized && !self.is_animating {
143            self.init(cx);
144        }
145
146        match event {
147            Event::NextFrame(ne) => {
148                // 检查是否是我们的 next_frame
149                if let Some(next_frame) = self.next_frame {
150                    if ne.set.contains(&next_frame) {
151                        let elapsed = ne.time - self.animation_start_time;
152                        if elapsed >= self.animation_duration * 0.5 {
153                            // 更新动画
154                            self.update_animation(cx, ne.time);
155                        }
156                        // 继续请求下一帧
157                        self.next_frame = Some(cx.new_next_frame());
158                    }
159                } else if self.initialized && !self.is_animating {
160                    // 如果组件已初始化但没有动画，重新开始动画
161                    self.animation_start_time = ne.time;
162                    self.next_frame = Some(cx.new_next_frame());
163                    self.update_animation(cx, ne.time);
164                    self.is_animating = true;
165                }
166            }
167            Event::WindowGeomChange(_) => {
168                self.reset_animation(cx);
169            }
170            _ => {}
171        }
172
173        if self.animator_handle_event(cx, event).must_redraw() {
174            self.redraw(cx);
175        }
176
177        self.view.handle_event(cx, event, scope);
178    }
179
180    fn draw_walk(&mut self, cx: &mut Cx2d, scope: &mut Scope, walk: Walk) -> DrawStep {
181        self.view.draw_walk(cx, scope, walk)
182    }
183}
184
185impl TypingAnimation {
186
187    pub fn init(&mut self, cx: &mut Cx) {
188        self.animation_start_time = cx.seconds_since_app_start();
189        self.next_frame = Some(cx.new_next_frame());
190
191        if let Some(state) = &mut self.animator.state {
192            // 设置初始状态
193            state.push_live(live!{
194                state: {
195                    content: {
196                        circle1: { margin: { top: (self.swing_base) }},
197                        circle2: { margin: { top: (self.swing_base) }},
198                        circle3: { margin: { top: (self.swing_base) }}
199                    }
200                }
201            });
202        }
203
204        // 立即开始第一个动画
205        self.update_animation(cx, self.animation_start_time);
206        self.is_animating = true; // 设为 true,表示动画已经开始
207    }
208
209    fn reset_animation(&mut self, cx: &mut Cx) {
210        self.animation_start_time = cx.seconds_since_app_start();
211        self.next_frame = Some(cx.new_next_frame());
212        self.current_animated_dot = CurrentAnimatedDot::default();
213        self.is_animating = false;
214    }
215    pub fn update_animation(&mut self, cx: &mut Cx, time: f64) {
216        self.current_animated_dot = self.current_animated_dot.next();
217        self.animation_start_time = time;
218
219        match self.current_animated_dot {
220            CurrentAnimatedDot::Dot1 => {
221                // 构建状态节点
222                let mut up_nodes = LiveNodeVec::new();
223                up_nodes.push_live(live_object!{
224                    content: {
225                        circle1: {
226                            margin: {top: (self.swing_base)}
227                        }
228                    }
229                });
230
231                let mut down_nodes = LiveNodeVec::new();
232                down_nodes.push_live(live_object!{
233                    content: {
234                        circle3: {
235                            margin: {top: (self.swing_amplitude)}
236                        }
237                    }
238                });
239
240                // 更新动画状态
241                if let Some(state) = &mut self.animator.state {
242                    state.replace_or_insert_last_node_by_path(
243                        0,
244                        &[live_id!(circle1).as_field(), live_id!(up).as_field(), live_id!(apply).as_field()],
245                        &up_nodes
246                    );
247
248                    state.replace_or_insert_last_node_by_path(
249                        0,
250                        &[live_id!(circle3).as_field(), live_id!(down).as_field(), live_id!(apply).as_field()],
251                        &down_nodes
252                    );
253                }
254
255                self.animator_play(cx, id!(circle1.up));
256                self.animator_play(cx, id!(circle3.down));
257            }
258            CurrentAnimatedDot::Dot2 => {
259                let mut up_nodes = LiveNodeVec::new();
260                up_nodes.push_live(live_object!{
261                    content: {
262                        circle2: {
263                            margin: {top: (self.swing_base)}
264                        }
265                    }
266                });
267
268                let mut down_nodes = LiveNodeVec::new();
269                down_nodes.push_live(live_object!{
270                    content: {
271                        circle1: {
272                            margin: {top: (self.swing_amplitude)}
273                        }
274                    }
275                });
276
277                if let Some(state) = &mut self.animator.state {
278                    state.replace_or_insert_last_node_by_path(
279                        0,
280                        &[live_id!(circle2).as_field(), live_id!(up).as_field(), live_id!(apply).as_field()],
281                        &up_nodes
282                    );
283
284                    state.replace_or_insert_last_node_by_path(
285                        0,
286                        &[live_id!(circle1).as_field(), live_id!(down).as_field(), live_id!(apply).as_field()],
287                        &down_nodes
288                    );
289                }
290
291                self.animator_play(cx, id!(circle1.down));
292                self.animator_play(cx, id!(circle2.up));
293            }
294            CurrentAnimatedDot::Dot3 => {
295                let mut up_nodes = LiveNodeVec::new();
296                up_nodes.push_live(live_object!{
297                    content: {
298                        circle3: {
299                            margin: {top: (self.swing_base)}
300                        }
301                    }
302                });
303
304                let mut down_nodes = LiveNodeVec::new();
305                down_nodes.push_live(live_object!{
306                    content: {
307                        circle2: {
308                            margin: {top: (self.swing_amplitude)}
309                        }
310                    }
311                });
312
313                if let Some(state) = &mut self.animator.state {
314                    state.replace_or_insert_last_node_by_path(
315                        0,
316                        &[live_id!(circle3).as_field(), live_id!(up).as_field(), live_id!(apply).as_field()],
317                        &up_nodes
318                    );
319
320                    state.replace_or_insert_last_node_by_path(
321                        0,
322                        &[live_id!(circle2).as_field(), live_id!(down).as_field(), live_id!(apply).as_field()],
323                        &down_nodes
324                    );
325                }
326
327                self.animator_play(cx, id!(circle2.down));
328                self.animator_play(cx, id!(circle3.up));
329            }
330        };
331    }
332
333    pub fn start(&mut self, cx: &mut Cx) {
334        // 如果动画没在运行，就启动它
335        if !self.is_animating {
336            self.initialized = true;     // 确保初始化状态
337            self.is_animating = false;   // 临时设为 false，这样 handle_event 会重新启动动画
338            self.reset_animation(cx);    // 重置动画状态
339
340            // 强制立即开始一次动画循环
341            self.animation_start_time = cx.seconds_since_app_start();
342            self.next_frame = Some(cx.new_next_frame());
343            self.update_animation(cx, self.animation_start_time);
344        }
345    }
346
347    pub fn stop(&mut self, cx: &mut Cx) {
348        if self.is_animating {
349            self.is_animating = false;   // 停止动画循环
350            self.next_frame = None;      // 移除下一帧的请求
351
352            // 将所有点重置到基础位置
353            if let Some(state) = &mut self.animator.state {
354                state.push_live(live!{
355                    state: {
356                        content: {
357                            circle1: { margin: { top: (self.swing_base) }},
358                            circle2: { margin: { top: (self.swing_base) }},
359                            circle3: { margin: { top: (self.swing_base) }}
360                        }
361                    }
362                });
363            }
364            self.redraw(cx);  // 强制重绘以更新视觉状态
365        }
366    }
367
368}
369
370
371/// // 设置小幅度快速摆动
372/// typing_animation.set_swing_parameters(cx, 5.0, 2.0); // 小幅度
373/// typing_animation.set_animation_speed(cx, 0.3); // 更快的速度
374///
375/// // 设置大幅度慢速摆动
376/// typing_animation.set_swing_parameters(cx, 15.0, 3.0); // 大幅度
377/// typing_animation.set_animation_speed(cx, 1.0); // 更慢的速度
378impl TypingAnimationRef {
379    // 提供设置动画参数的方法
380    pub fn set_swing_parameters(&self, cx: &mut Cx, amplitude: f64, base: f64) {
381        if let Some(mut inner) = self.borrow_mut() {
382            inner.swing_amplitude = amplitude;
383            inner.swing_base = base;
384            // 如果动画正在进行，需要重新应用参数
385            if inner.next_frame.is_some() {
386                let time = cx.seconds_since_app_start();
387                inner.update_animation(cx, time);
388            }
389        }
390    }
391
392    pub fn set_animation_speed(&self, cx: &mut Cx, duration: f64) {
393        if let Some(mut inner) = self.borrow_mut() {
394            inner.animation_duration = duration;
395            // 如果动画正在进行，需要重新应用参数
396            if inner.next_frame.is_some() {
397                let time = cx.seconds_since_app_start();
398                inner.update_animation(cx, time);
399            }
400        }
401    }
402
403    pub fn start(&self, cx: &mut Cx) {
404        // self.set_swing_parameters(cx, 5.0, 2.0);
405        self.set_animation_speed(cx, 0.3);
406        if let Some(mut inner) = self.borrow_mut() {
407            inner.start(cx);
408        }
409    }
410
411    pub fn stop(&self, cx: &mut Cx) {
412        if let Some(mut inner) = self.borrow_mut() {
413            inner.stop(cx);
414        }
415    }
416}
```

Example 2 (rust):
```rust
1let typing_animation = self.view.typing_animation(id!(typing_animation));
2// Start
3typing_animation.start(cx);
4// Stop
5typing_animation.stop(cx);
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/creating-an-empty-window

**Contents:**
- #2 - 创建一个空窗口
- #你将学到
- #添加 Makepad 作为依赖
- #添加代码
- #检查当前进展
- #代码解析
  - #Live Design 块
    - #Live 设计系统
  - #App 结构体
    - #Live Trait

在上一步中，我们创建了一个新的空的 Cargo 包。在这一步中，我们将把这个空包变成一个最小的 Makepad 应用，仅包含一个空窗口。

💡 如果你不想跟着手动输入，可以直接查看这一步的完整代码：GitHub 链接

我们从向项目添加 Makepad 依赖开始。

Makepad 的顶层 crate 是 makepad-widgets，它包含了构建应用所需的一切。

将以下文件复制到src目录中（我们随后会解释代码的作用）：

以下代码定义了一个live design块：

Live Design 块用于定义 Makepad 的DSL 代码。DSL 在 Makepad 中用于描述布局和样式，类似 CSS。

use link::widgets::*'; 导入内置组件，比如 Root,Window 和 View。

App = {{App}} { ... } 定义了一个 App。

{{App}} 语法表示该 DSL 定义与 Rust 中的 App 结构体相关联。

在 App 中，我们将 ui 字段定义为组件树的根节点：

<Root> { ... } 是我们的顶层容器。

<Window> { ... } 是屏幕上的一个窗口。

body = <View> {} 是一个命名为 body 的空视图。

当我们运行应用程序时，Makepad 使用一种叫做 Live 设计系统 的机制，将 DSL 代码与 Rust 代码进行桥接。

应用的布局和样式是通过 DSL 代码定义的。

Live 设计系统会将 DSL 中的定义匹配到对应的 Rust 结构体，并用 DSL 中的值初始化这些结构体。

如果稍后在 Makepad Studio 中修改了 DSL 代码，相关的 Rust 结构体会在运行时自动更新，无需重新启动应用程序。

Live 设计系统使 Makepad 拥有强大的 运行时样式编辑能力。你可以在 Makepad Studio 中动态调整 UI 的布局、颜色以及其他样式属性，并且能在应用运行时立即看到修改效果 —— 无需重新编译！

App 结构体是我们应用程序的核心。现在它仅包含我们组件树（widget tree）的根节点。稍后我们还会在其中添加应用所需的各种状态。

注意，我们为 App 结构体派生了两个 trait：Live 和 LiveHook。接下来我们将更详细地了解这两个 trait。

Live trait 使结构体能够与 Live 设计系统进行交互。

要为某个结构体派生 Live trait，该结构体中的每个字段都需要标注以下属性之一：

当 Live 设计系统遇到标有 #[live] 属性的字段时，它会在 DSL（领域特定语言）代码中查找该字段的匹配定义。如果找到了，就使用 DSL 中的定义来实例化该字段；如果没有找到，则会使用默认值来实例化该字段。

相对地，当系统遇到标有 #[rust] 属性的字段时，它会使用该字段类型的 Default::default 构造函数来实例化这个字段。

在我们的例子中，我们用 #[live] 属性标记了 App 结构体中的 ui 字段。由于我们在 DSL 中为该字段提供了相应的定义，Live 设计系统会自动根据 DSL 中的定义填充 ui 字段，构建出组件树：一个包含一个空 View 的 Window，由 Root 组件包裹。

LiveHook trait 提供了一些可被重写（overridable）的方法，这些方法会在应用程序生命周期的不同阶段被调用。

目前我们暂时用不到这些方法，因此可以为 App 结构体定义一个空的 LiveHook 实现：

或者，也可以像我们现在这样为 App 结构体派生（derive）LiveHook trait。这样会自动生成与上面相同的实现，从而省去了手动编写的工作。

等到我们真正需要用到这些方法时，再来详细解释 LiveHook trait 的具体作用。

以下代码为 App 结构体实现了 AppMain trait：

AppMain trait 用于将 App 结构体的一个实例挂接（hook）到主事件循环中。在我们的实现中，我们只是简单地将所有事件转发给组件树的根节点，由它来进行适当的处理。

在 Makepad 中，Scope（作用域） 是一个容器，用于在事件传递过程中携带应用级别的数据以及组件特定的属性。

目前我们还没有任何状态，因此暂时使用 Scope::empty() 来为每个事件创建一个空的作用域。关于作用域的更多内容，我们将在后面进一步介绍。

以下代码为 App 结构体实现了 LiveRegister trait：

LiveRegister trait 用于注册 DSL 代码。通过注册 DSL 代码，我们可以让整个应用访问这些定义。

前面我们提到过，live design 块用于定义 DSL 代码。在底层，每个 live design 块都会生成一个 live_design 函数，当这个函数被调用时，会将该块中的 DSL 代码注册进系统。而我们正是在 LiveRegister trait 的实现中调用这些 live_design 函数。

在我们的实现中，我们调用了 makepad_widgets::live_design 函数，用于注册 makepad_widgets crate 中的 DSL 代码。如果不调用这个函数，我们将无法使用任何内置组件，比如之前看到的 Root、Window 和 View 等组件。

注意：由 app.rs 文件顶部的 live_design 宏生成的 live_design 函数是一个特殊函数。我们不需要在这里手动调用它，因为它会被 app_main 函数自动调用。而这个 app_main 函数是由 app_main 宏生成的，我们接下来会讲解它。

app_main 宏会生成一个 app_main 函数，里面包含启动应用所需的所有代码。

我们现在有了一个最简的 Makepad 应用程序，包含一个空窗口。接下来的步骤中，我们将开始为应用构建一个图片网格。

**Examples:**

Example 1 (bash):
```bash
1cargo add makepad-widgets
```

Example 2 (rust):
```rust
1use makepad_widgets::*;
2
3live_design! {
4    use link::widgets::*;
5
6    App = {{App}} {
7        ui: <Root> {
8            <Window> {
9                body = <View> {}
10            }
11        }
12    }
13}
14
15#[derive(Live, LiveHook)]
16pub struct App {
17    #[live]
18    ui: WidgetRef,
19}
20
21impl AppMain for App {
22    fn handle_event(&mut self, cx: &mut Cx, event: &Event) {
23        self.ui.handle_event(cx, event, &mut Scope::empty());
24    }
25}
26
27impl LiveRegister for App {
28    fn live_register(cx: &mut Cx) {
29        makepad_widgets::live_design(cx);
30    }
31}
32
33app_main!(App);
```

Example 3 (rust):
```rust
1pub mod app;
```

Example 4 (rust):
```rust
1fn main() {
2    ::app::app_main()
3}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/creating-a-menubar

**Contents:**
- #13 - 创建菜单栏
- #定义菜单栏
- #更新 App
- #检查目前的进度
- #下一步

在前面的几个步骤中，我们为应用构建了图片网格和幻灯片。但到目前为止，还不能在两者之间切换。接下来的几个步骤里，我们将实现图片网格和幻灯片之间的切换功能。

菜单栏中的按钮将用于切换到幻灯片视图。当幻灯片可见时，按下 Esc 键将切换回图片网格。

注意：如果你不想自己敲代码，可以在这里找到本步骤的全部代码：https://github.com/makepad/image_viewer/tree/main/step_13

让我们先为菜单栏（MenuBar）添加一个定义。菜单栏是一个宽而矮的条状区域，占满容器的宽度，包含我们用来切换到幻灯片的按钮。

在 app.rs 中，找到 live design 块，在 ImageGrid 定义之前，添加以下代码：

菜单栏包含一个按钮，按钮前面有一个填充组件（Filler），用来确保按钮被布局到右侧。

既然我们已经有了菜单栏（MenuBar），接下来更新 App 的定义，让它显示菜单栏而不是幻灯片（幻灯片我们后面会再放回去）。

在 app.rs 中，找到 live design 块，替换 App 的定义为以下内容：

当然，你实际上看到的是菜单栏，但因为菜单栏本身是透明的，所以你只看到了按钮。

这一步我们创建了菜单栏（MenuBar）。下一步，我们将创建图片浏览器（ImageBrowser）。

**Examples:**

Example 1 (rust):
```rust
1MenuBar = <View> {
2        width: Fill,
3        height: Fit,
4
5        <Filler> {}
6        slideshow_button = <Button> {
7            text: "Slideshow"
8        }
9    }
```

Example 2 (rust):
```rust
1App = {{App}} {
2        placeholder: (PLACEHOLDER),
3
4        ui: <Root> {
5            <Window> {
6                body = <View> {
7                    <MenuBar> {}
8                }
9            }
10        }
11    }
```

Example 3 (bash):
```bash
1cargo run --release
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/loading-images

**Contents:**
- #7 - 加载图片
- #你将学到的内容
- #下载图片
- #导入类型
- #更新 State 结构体
  - #更新 State 结构体的 Default 实现
  - #更新num_images 方法
- #更新 App 结构体
- #增加辅助方法
  - #错误处理

在上一步中，我们为应用添加了状态，使图像网格变得动态。

现在我们已经添加了状态，接下来将用它来加载真实图片并显示在网格中。

注意：如果你不想手动输入代码，可以在这里找到本步骤的完整代码：https://github.com/makepad/image_viewer/tree/main/step_7

由于我们接下来将展示真实的图片，首先我们需要一些图片资源。你可以选择使用你自己的图片， 或者下载我们在本教程中使用的图片压缩包：

在接下来的代码中，我们将使用 Rust 标准库中的 Path 和 PathBuf 类型。在使用这些类型之前，我们需要先将它们导入。

请在 app.rs 文件顶部添加以下代码：

这将使我们可以在接下来的代码中使用 Path 和 PathBuf 类型。

现在我们已经有了一些图片，让我们更新 State 结构体，用来存储图片路径的列表，而不只是图片数量。

请在 app.rs 中，将 State 结构体的定义替换为以下内容：

这段更新将原来的 num_images 字段替换为了 image_paths 字段，image_paths 是一个包含所有图片路径的列表。

我们还需要更新 State 结构体的 Default trait 实现，以反映结构体字段的更改。

请在 app.rs 中，将 Default trait 的实现替换为以下内容：

最后，我们需要更新 State 结构体中的 num_images 方法。因为我们已经不再使用 num_images 字段，而是通过 image_paths 列表来表示图片数量。

请在 app.rs 中，将 num_images 方法替换为以下实现：

我们现在已经拥有加载真实图片所需的全部信息。

在继续之前，我们需要对 App 结构体做一个小改动。

在 app.rs 文件中，将 App 结构体的定义替换为以下内容：

我们刚刚从 App 结构体中移除了 LiveHook trait 的派生。这是因为我们即将为 App 编写我们自己的 LiveHook trait 实现（我们马上会解释原因）。

为了让后续的代码更容易编写，我们将为 App 结构体添加一些辅助函数。

请将以下代码添加到 app.rs 文件中：

load_image_paths 方法的作用如下：

首先，它会清空 image_paths 中已有的路径。

最后，它调用 self.ui.redraw(cx) 来安排界面重绘。

这实际上用指定路径目录中所有文件的路径替换了 image_paths 中原有的路径。

为了简化起见，我们没有在 load_image_paths 方法中添加任何错误处理。该方法可能会失败的情况包括：

如果发生上述任何错误，我们的应用程序会直接崩溃（panic）。这对于教程来说是可以接受的，但在真实的应用中，我们需要更健壮的错误处理机制。

既然我们已经有了一个方法来加载指定目录下所有图片的路径，我们需要确保该方法在应用启动时被调用。

为此，我们可以使用 LiveHook 特性。回想一下，这个特性提供了几个可重写的方法，这些方法会在应用生命周期的不同阶段被调用。

注意：请务必将 "path/to/your/images" 替换为你自己的图片目录路径！

这段代码实现了 App 结构体的 LiveHook 特性。我们重写了其中的一个方法 after_new_from_doc。该方法会在应用被 live design 系统完全初始化之后，但在开始运行之前被调用。这使得它成为调用 load_image_paths 方法的理想时机。

最后一步是在每次绘制图像网格中的每个 Image 之前，使用状态中保存的图片路径列表重新加载对应的图片。为此，我们需要修改 ImageRow 结构体中 Widget 特性下的 draw_walk 方法的实现。

请将 ImageRow 上 Widget 特性的 draw_walk 方法实现替换为以下代码：

代码量比较多，但这里唯一新增的内容是以下几行：

最终效果是，每个 ImageItem 里的 Image 会在绘制之前被重新加载为对应的正确图片。图片加载是异步完成的，细节处理在后台自动完成。

注意：如果图片已经显示的是正确的图片，则不会重复加载。

如果一切正常，你的屏幕上应该会弹出一个包含图片网格的窗口，这次显示的应该是真实的图片，而不是占位符图片：

我们现在已经有了一个相当不错的图片网格实现。它能够显示真实的图片，并且根据图片数量动态调整行数和每行的图片数量。

我们暂时先保持图片网格不变。接下来的几个步骤中，我们将为应用构建一个幻灯片播放功能。

**Examples:**

Example 1 (rust):
```rust
1use std::path::{Path, PathBuf};
```

Example 2 (rust):
```rust
1#[derive(Debug)]
2pub struct State {
3    image_paths: Vec<PathBuf>,
4    max_images_per_row: usize,
5}
```

Example 3 (rust):
```rust
1impl Default for State {
2    fn default() -> Self {
3        Self {
4            image_paths: Vec::new(),
5            max_images_per_row: 4,
6        }
7    }
8}
```

Example 4 (rust):
```rust
1fn num_images(&self) -> usize {
2    self.image_paths.len()
3}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/creating-an-imagebrowser

**Contents:**
- #14 - 创建图片浏览器
- #定义 ImageBrowser
- #更新 App
- #检查目前的进度
- #下一步

在上一步中，我们创建了菜单栏（MenuBar），这是实现图片网格和幻灯片切换功能的一部分。

这一步，我们将创建图片浏览器（ImageBrowser）。

注意：如果你不想自己敲代码，可以在这里找到本步骤的全部代码：https://github.com/makepad/image_viewer/tree/main/step_14

我们先为 ImageBrowser 添加一个定义。ImageBrowser 结合了我们之前创建的 MenuBar 和 ImageGrid。

在 app.rs 中，找到 live design 块，在 ImageGrid 的定义之后，添加以下代码：

现在我们已经有了 ImageBrowser，接下来更新 App 的定义，使其显示 ImageBrowser 而不是 MenuBar（我们之后会把 MenuBar 放回合适的位置）。

在 app.rs 中，找到 live design 块，替换 App 的定义为以下内容：

如果一切正常，你应该会看到一个图片网格，上方有一个用于切换到幻灯片的按钮：

在这一步中，我们创建了图片浏览器（ImageBrowser）。下一步，我们将使用 PageFlip 在图片浏览器和幻灯片（Slideshow）之间进行切换。

**Examples:**

Example 1 (rust):
```rust
1ImageBrowser = <View> {
2        flow: Down,
3
4        <MenuBar> {}
5        <ImageGrid> {}
6    }
```

Example 2 (rust):
```rust
1App = {{App}} {
2        placeholder: (PLACEHOLDER),
3
4        ui: <Root> {
5            <Window> {
6                body = <View> {
7                    <ImageBrowser> {}
8                }
9            }
10        }
11    }
```

Example 3 (bash):
```bash
1cargo run --release
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/creating-an-imagerow

**Contents:**
- #4 - 创建 ImageRow
- #你将学到的内容
- #定义ImageRow
- #定义 ImageRow
  - #模板（Templates）
- #更新 App
- #定义ImageRow 结构体
  - #派生Widget trait
- #Widget trait的实现
  - #在View 中绘制每个项目

在上一步中，我们创建了一个 ImageItem 来显示单张图片，作为构建应用图片网格的一部分。

这一步中，我们将把多个 ImageItem 组合到一个 ImageRow 中，以显示一行图片。

注意：如果你不想跟着敲代码，可以在这里找到本步骤的全部代码：https://github.com/makepad/image_viewer/tree/main/step_4

我们首先添加一个 ImageRow 的定义。ImageRow 负责将多个 ImageItem 水平排列。

在 app.rs 中，将以下代码添加到 live design 块中，紧跟在 ImageItem 的定义之后：

这段代码定义了一个 ImageRow。{{ImageRow}} 语法将我们在 DSL 中定义的 ImageRow 关联到 Rust 代码中的 ImageRow 结构体（我们稍后会介绍这个结构体）。

在 ImageRow 内部，我们使用了一个 PortalList 来列出其中的条目。

这个 PortalList 具有以下属性：

注意：PortalList 类似于一个标准列表，但它支持“无限滚动”：它只会渲染可见的条目，因此可以高效地处理大型列表。尽管我们其实不需要无限滚动的功能，但在撰写本文时，Makepad 还没有标准列表组件，因此我们使用 PortalList 作为替代方案。

与其他组件不同，PortalList 的内容并不是通过 DSL 代码来决定的，而是必须通过 Rust 代码动态生成。

它并不像你可能预期的那样定义了一个 ImageItem 实例。相反，它定义了一个 ImageItem 的模板。稍后，当我们在 Rust 代码中生成这个 PortalList 的内容时，就可以使用这个模板来创建所需的各个项的实例。

注意: 回忆一下，{{ImageRow}} 语法告诉 Makepad，ImageRow 的定义关联到了 Rust 代码中的一个 ImageRow 结构体。正因为 PortalList 的内容必须由 Rust 代码生成，我们才需要使用一个 Rust 结构体来承载它。

现在我们已经定义了 ImageRow，接下来我们将更新 App 的定义，使其显示一个 ImageRow，而不是一个 ImageItem。

在 app.rs 中，将 live design 块中的 App 定义替换为下面这一段代码：

在之前的 DSL 代码中，我们使用 {{ImageRow}} 语法将 ImageRow 链接到了 Rust 代码中的 ImageRow 结构体。通过定义这样一个结构体，我们可以使用 Rust 代码来重写 ImageRow 的行为。

请注意，我们为 ImageRow 结构体派生了多个 trait。我们已经了解了 Live 和 LiveHook 这两个 trait 的作用，但 Widget 是新的。我们来更详细地看看这个 trait 的作用。

Widget trait 允许我们自定义一个小部件（widget）的行为。

有些令人困惑的是，派生 Widget trait 并不会自动生成该 trait 的实现。相反，它会为 Widget 所依赖的一些辅助 trait 生成实现。这使得我们更容易手动实现 Widget trait，但我们仍然需要自己编写具体的实现代码，我们将在下一节中完成这部分工作。

#[deref] 属性在派生 Widget trait 时会用到。将此属性放在 ImageRow 结构体的 view 字段上，可以让我们像使用 View 一样使用 ImageRow：派生 Widget trait 会自动生成将 ImageRow 解引用为 View 的代码，并建立相应的 DSL 绑定。

让我们仔细看看 draw_walk 方法：

在 draw_walk 方法内部，我们首先在一个循环中调用 self.view.draw_walk(cx, scope, walk) 来绘制视图中的每个项目。

view 上的 draw_walk 函数是一个所谓的可恢复函数（resumable function）——它的行为类似于迭代器，在绘制过程中逐个生成项目。

每次调用 draw_walk，它会返回一个特殊的 DrawStep 对象，表示绘制过程的当前状态。然后我们调用该对象的 step 方法，获取下一个应该绘制的项目。调用者（也就是我们）负责绘制每个项目，这使得我们可以自定义它的绘制方式。

当没有更多项目需要绘制时，调用 step 方法会返回 None，循环结束。

draw_walk 方法中以下代码负责绘制 PortalList（回想一下，在 ImageRow 中，我们使用 PortalList 来列出其项目。）

对于每个要绘制的项目，我们首先调用 as_portal_list 来检查该项目是否是一个 PortalList。一旦获得对 PortalList 的引用：

每个 PortalList 有自己独立的索引命名空间 —— 这意味着每个列表中每个项目的索引是唯一的。调用 list.item(cx, item_index, live_id!(ImageItem)) 会检查给定索引是否已有对应的项目实例。如果没有，它会创建该项目的实例。

那么，调用 list.item(cx, item_index, live_id!(ImageItem)) 是如何知道要实例化什么呢？很简单：它使用我们之前在 DSL 中定义的 ImageItem 模板：

注意：live_id! 宏用于在 Makepad 中生成唯一标识符。在这里，live_id!(ImageItem) 指的是 ImageItem 的模板。

如果一切正常，屏幕上应该会出现一个包含一排占位符图片的窗口：

在这一步中，我们创建了一个 ImageRow 用来显示一排图片。下一步，我们将把多个 ImageRow 组合到一个 ImageGrid 中，以显示一个图片网格。

**Examples:**

Example 1 (rust):
```rust
1ImageRow = {{ImageRow}} {
2        <PortalList> {
3            height: 256,
4            flow: Right,
5            
6            ImageItem = <ImageItem> {}
7        }
8	}
```

Example 2 (rust):
```rust
1ImageItem = <ImageItem> {}
```

Example 3 (rust):
```rust
1App = {{App}} {
2        ui: <Root> {
3            <Window> {
4                body = <View> {
5                    <ImageRow> {}
6                }
7            }
8        }
9    }
```

Example 4 (rust):
```rust
1#[derive(Live, LiveHook, Widget)]
2pub struct ImageRow {
3    #[deref]
4    view: View,
5}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/layout-system/index

**Contents:**
- #基本概念
- #详细对比
  - #Walk 属性
  - #Layout 属性
- #协同工作机制

Layout 和 Walk 是 Makepad 布局系统中最核心的两个概念，它们共同决定了组件的布局行为。

这里使用 Walk 这个术语，是因为 Turtle Draw 是 Makepad 布局系统的核心机制,它的灵感来源于经典的海龟绘图(Turtle Graphics)。这种布局方式允许我们通过一个"海龟"在画布上行走（Walk）来确定组件的位置和尺寸。

让我们通过一个实例来看它们是如何协同工作的：

**Examples:**

Example 1 (rust):
```rust
1walk: {
2    width: Fill,      // 在父容器中的宽度行为
3    height: Fixed(50), // 在父容器中的高度行为
4    margin: {         // 与其他组件的间距
5        left: 10,
6        right: 10,
7        top: 5,
8        bottom: 5
9    }
10}
```

Example 2 (rust):
```rust
1layout: {
2    flow: Down,       // 子组件排列方向
3    spacing: 10,      // 子组件之间的间距
4    padding: {        // 内部留白
5        left: 10,
6        right: 10,
7        top: 5,
8        bottom: 5
9    },
10    align: {          // 子组件对齐方式
11        x: 0.5,       // 水平居中
12        y: 0.0        // 顶部对齐
13    }
14}
```

Example 3 (rust):
```rust
1Container = <View> {
2    // 决定容器自身在父级中的表现
3    walk: {
4        width: Fill,        // 填充父容器宽度
5        height: Fit,        // 高度适应内容
6        margin: 10         // 四周留出10px间距
7    },
8
9    // 决定如何排列其子组件
10    layout: {
11        flow: Down,        // 子组件垂直排列
12        spacing: 5,        // 子组件间距5px
13        padding: 15,       // 内部留白15px
14        align: {x: 0.5}    // 子组件水平居中
15    },
16
17    // 子组件
18    <Button> {
19        walk: {
20            width: Fixed(100),  // 固定宽度100px
21            height: Fixed(40)   // 固定高度40px
22        }
23    }
24
25    <Button> {
26        walk: {
27            width: Fixed(100),
28            height: Fixed(40)
29        }
30    }
31}
```

Example 4 (json):
```json
1┌─── Container(Fill × Fit) ───┐
2│    ┌── padding: 15px ──┐    │
3│    │   ┌─────────┐     │    │
4│    │   │ Button1 │     │    │
5│    │   └─────────┘     │    │
6│    │      5px          │    │
7│    │   ┌─────────┐     │    │
8│    │   │ Button2 │     │    │
9│    │   └─────────┘     │    │
10│    └───────────────────┘    │
11└─────────────────────────────┘
12    ↑     ↑          ↑      ↑
13 margin  对齐       flow   margin
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/adding-more-state

**Contents:**
- #11 - 添加更多状态
- #你将学到的内容
- #更新 State 结构体
  - #更新 State 结构体的 Default Trait 实现
- #引用依赖项
- #添加辅助函数
  - #添加 set_current_image 方法
  - #更新 load_image_paths 方法
- #检查到目前为止的进度
- #下一步

在前几步中，我们为应用创建了幻灯片的初步实现。

为了保持初步实现的简单性，我们的幻灯片只显示了占位图片，还不能响应用户事件。

在接下来的几步中，我们将去除这些限制，让幻灯片变得动态。

注意：如果你不想跟着敲代码，可以在这里找到本步骤的全部代码：https://github.com/makepad/image_viewer/tree/main/step_11

让我们从更新 State 结构体开始，使其包含幻灯片放映所需的状态。

在 app.rs 文件中，用下面的代码替换 State 结构体的定义：

我们还需要更新 State 结构体的 Default trait 实现，以反映这个新字段。

在 app.rs 文件中，用下面的代码替换 State 结构体的 Default trait 实现：

在接下来的代码中，我们将从 Rust 代码中引用作为资源打包到应用中的占位图片。

回想一下，像占位图片这样的打包资源在 Makepad 中称为依赖项（dependencies），我们可以在 DSL 代码中使用如下表达式来引用它们：

但是我们如何在 Rust 代码中引用依赖项呢？为此，我们需要使用一些小技巧。

我们先从给 App 结构体添加一个字段开始。

在 app.rs 文件中，用下面的代码替换 App 结构体的定义：

注意，我们给该字段加上了 #[live] 属性，这意味着它会从 DSL 代码中初始化。为了使其生效，我们需要在 DSL 代码中为该字段添加相应的定义。

在 app.rs 文件的 live design 块中，用下面的代码替换 App 的定义：

这为 App 结构体的 placeholder 字段添加了定义。

有了这个定义，live design 系统会自动用正确的路径初始化 App 结构体中的 placeholder 字段。正如我们在下一节将看到的，我们可以在 Rust 代码中使用这个路径来加载占位图片。

既然我们已经有了从 Rust 代码引用占位图片的方法，现在是时候为 App 结构体添加一些辅助函数，并更新一些已有函数了。这会让后续的代码更容易编写。

我们先定义一个 set_current_image 方法。该方法用于更改幻灯片当前显示的图片。

在 app.rs 文件中，添加以下代码到 App 结构体的 impl 块中：

注意，当使用占位图片重新加载 Image 时，set_current_image 方法会使用之前定义的 App 结构体中的 placeholder 字段。

接下来，我们将更新已有的 load_image_paths 方法，使其在加载一组新图片时，调用 set_current_image 方法设置当前显示的图片。

在 app.rs 文件中，替换 App 结构体中 load_image_paths 方法的定义为以下代码：

此更改确保幻灯片始终显示当前图片，或者在没有图片时显示占位图片。

如果一切正常，你应该会看到和之前一样的幻灯片，不过这次它会显示一张真实的图片：

我们现在已经拥有了幻灯片所需的状态。下一步，我们将使用这些状态来响应用户事件。

**Examples:**

Example 1 (rust):
```rust
1#[derive(Debug)]
2pub struct State {
3    image_paths: Vec<PathBuf>,
4    max_images_per_row: usize,
5    current_image_idx: Option<usize>,
6}
```

Example 2 (rust):
```rust
1impl Default for State {
2    fn default() -> Self {
3        Self {
4            image_paths: Vec::new(),
5            max_images_per_row: 4,
6            current_image_idx: None,
7        }
8    }
9}
```

Example 3 (rust):
```rust
1dep("crate://self/resources/placeholder.jpg")
```

Example 4 (rust):
```rust
1#[derive(Live)]
2pub struct App {
3    #[live]
4    placeholder: LiveDependency,
5    #[live]
6    ui: WidgetRef,
7    #[rust]
8    state: State,
9}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/shading-language/draw-basic-graphics

**Contents:**
- #绘制基础图形
- #几何图元与变换
  - #基本图元类型
  - #坐标变换
- #距离场技术
  - #什么是距离场？
  - #使用有符号距离场绘制形状
- #SDF 视口介绍 (Sdf2d::viewport)
  - #1. 初始坐标系 (self.pos)
  - #2. 乘以尺寸 (self.pos * self.rect_size)

在开始绘制复杂图形之前，我们需要先理解最基本的构建块。在计算机图形学中，所有复杂的形状最终都是由基本图元构建而成的。

你会需要对这些图形进行各种操作：移动物体的位置、旋转物体的角度、改变物体的大小。这个时候就需要使用变换矩阵，一种用数学方式来精确描述这些操作的工具。

理解变换矩阵是处理图形的基础。主要有三种基本变换：

距离场（Distance Field）是 Makepad 中实现高质量图形渲染的核心技术。让我们深入理解它：

距离场是一个函数，它告诉我们空间中任意一点到形状边界的最短距离：

把距离场想象成一堆不太透明的圆是很有用的。这个距离场中，表示纯白色的区域(也就是数值为1的部分)就在对象上。纯黑色部分，就是距离物体最远的点。中间的灰色部分，数值在0-1之间。这是一种可视化0到1之间距离的方法。

有号距离场 (Signed Distance Field, SDF) 是一个三维标量场，其中每个点的值表示从该点到最近表面的距离。这些距离值具有“有号”的特性：

通过这种方式，SDF不仅能够提供从任意点到物体表面的最近距离，还能够区分点在物体内部还是外部。

有符号距离场 (SDF) 基于以下几个关键概念：

想象我们正在绘制一个按钮，尺寸是 200x100 像素。整个转换过程可以分为三个关键阶段：

为了更好地理解这个转换过程，我们可以看一些具体的坐标点是如何转换的：

**Examples:**

Example 1 (rust):
```rust
1// 1. 点
2fn draw_point(self) -> vec4 {
3    let point_size = 5.0;
4    // 在 pixel shader 中我们需要判断是否在点的范围内
5    let distance = length(self.pos - vec2(0.5));
6    return vec4(1.0, 0.0, 0.0, step(distance, point_size));
7}
8
9// 2. 线段
10fn draw_line(self) -> vec4 {
11    let line_width = 2.0;
12    let p1 = vec2(0.0, 0.0);
13    let p2 = vec2(1.0, 1.0);
14    // 计算点到线的距离
15    let dist = distance_to_line(self.pos, p1, p2);
16    return vec4(0.0, 1.0, 0.0, step(dist, line_width));
17}
18
19// 3. 三角形
20fn draw_triangle(self) -> vec4 {
21    let vertices = [
22        vec2(0.0, 0.0),
23        vec2(1.0, 0.0),
24        vec2(0.5, 1.0)
25    ];
26    // 判断点是否在三角形内
27    let inside = point_in_triangle(self.pos, vertices);
28    return vec4(0.0, 0.0, 1.0, inside);
29}
```

Example 2 (unknown):
```unknown
1点       线段             三角形
2
3●         ●               ▲
4         ╱               / ╲
5        ●               /   ╲
6                       ▔▔▔▔▔▔▔
```

Example 3 (rust):
```rust
1fn translate(pos: vec2, offset: vec2) -> vec2 {
2    // 简单的向量加法
3    return pos + offset;
4}
```

Example 4 (rust):
```rust
1fn rotate(pos: vec2, angle: float) -> vec2 {
2    let s = sin(angle);
3    let c = cos(angle);
4    return vec2(
5        pos.x * c - pos.y * s,
6        pos.x * s + pos.y * c
7    );
8}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/animation-system/index

**Contents:**
- #概述及关键概念
- #动画系统概述
- #关键概念
  - #动画状态管理
  - #动画更新机制的关键代码结构
  - #关键帧插值计算
  - #动画轨道（track）系统

Makepad 的动画系统是一个声明式的动画框架，它通过状态(State)和过渡(Transition)的概念来管理 UI 动画。

让我们通过一个「正在输入」动画组件来深入理解整个系统。

让我们先看 Animator 的核心数据结构：

这个结构看起来很简单，但它实际上是一个非常精巧的设计：

**Examples:**

Example 1 (rust):
```rust
1pub struct Animator {
2    // 是否忽略找不到的动画状态
3    pub ignore_missing: bool,
4    // 指向 Live DSL 中动画定义的引用
5    pub live_ptr: LiveRef,
6    // 当前动画状态数据
7    pub state: Option<Vec<LiveNode>>,
8    // 下一帧的调度器
9    pub next_frame: NextFrame,
10}
```

Example 2 (rust):
```rust
1impl Animator {
2    // 硬切换到指定状态
3    pub fn cut_to(&mut self, cx: &mut Cx, state_pair: &[LiveId; 2], index: usize, nodes: &[LiveNode]) {
4        // 获取当前状态或创建新状态
5        let mut state = self.swap_out_state().unwrap_or(Vec::new());
6        let track = state_pair[0];
7
8        // 初始化状态结构
9        if state.len() == 0 {
10            state.push_live(live!{
11                tracks: {},
12                state: {}
13            });
14        }
15
16        // 更新轨道状态
17        state.replace_or_insert_last_node_by_path(0, &[live_id!(tracks).as_field(), track.as_field()],
18            live_object!{
19                [track]: {state_id: (state_pair[1]), ended: 1}
20            }
21        );
22
23        // 应用状态值
24        let mut path = Vec::new();
25        path.push(live_id!(state).as_field());
26
27        // ... 读取和应用状态值的代码 ...
28
29        self.swap_in_state(state);
30    }
31
32    // 动画过渡到指定状态
33    pub fn animate_to(&mut self, cx: &mut Cx, state_pair: &[LiveId; 2], index: usize, nodes: &[LiveNode]) {
34        // 类似 cut_to，但会创建插值动画
35        let mut state = self.swap_out_state().unwrap_or(Vec::new());
36
37        // ... 设置动画轨道
38        state.replace_or_insert_last_node_by_path(0, &[live_id!(tracks).as_field(), track.as_field()],
39            live_object!{
40                [track]: {
41                    state_id: (state_pair[1]),
42                    ended: 0,
43                    time: void
44                }
45            }
46        );
47
48        // ... 创建插值动画
49
50        self.swap_in_state(state);
51        self.next_frame = cx.new_next_frame();
52    }
53}
```

Example 3 (rust):
```rust
1impl Animator {
2    pub fn handle_event(&mut self, cx: &mut Cx, event: &Event) -> AnimatorAction {
3        if let Event::NextFrame(nf) = event {
4            // 检查是否是我们的下一帧
5            if !nf.set.contains(&self.next_frame) {
6                return AnimatorAction::None
7            }
8
9            // 更新所有活跃的动画轨道
10            let state_nodes = self.state.as_mut().unwrap();
11            let mut ended = true;
12            let mut redraw = false;
13
14            // ... 遍历并更新所有动画轨道
15
16            // 如果还有动画在运行，继续请求下一帧
17            if !ended {
18                self.next_frame = cx.new_next_frame();
19            }
20
21            return AnimatorAction::Animating {redraw}
22        }
23        AnimatorAction::None
24    }
25}
```

Example 4 (rust):
```rust
1impl Animator {
2    fn update_timeline_value(
3        cx: &mut Cx,
4        index: usize,
5        nodes: &mut [LiveNode],
6        ext_time: f64
7    ) -> (bool, bool) {
8        // 提取关键帧
9        let mut prev_kf: Option<KeyFrame> = None;
10
11        // 计算当前时间点的插值
12        for key_frame in key_frames {
13            if time >= prev_kf.time && time <= key_frame.time {
14                let normalized_time = (time - prev_kf.time) /
15                    (key_frame.time - prev_kf.time);
16
17                // 应用缓动函数
18                let mix = key_frame.ease.map(normalized_time);
19
20                // 根据值类型进行插值
21                let new_val = match (prev_kf.value, key_frame.value) {
22                    // 数值插值
23                    (LiveValue::Float64(a), LiveValue::Float64(b)) => {
24                        LiveValue::Float64((b - a) * mix + a)
25                    },
26                    // 颜色插值
27                    (LiveValue::Color(a), LiveValue::Color(b)) => {
28                        LiveValue::Color(Vec4::from_lerp(
29                            Vec4::from_u32(a),
30                            Vec4::from_u32(b),
31                            mix as f32
32                        ).to_u32())
33                    },
34                    // ... 其他类型插值
35                };
36
37                // 更新当前值
38                nodes[last_child_index].value = new_val;
39                return (ended, redraw)
40            }
41        }
42    }
43}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/appendix/packaging-guide

**Contents:**
- #Makepad 项目打包指南
- #一、桌面打包
  - #预打包测试
    - #1. 安装cargo-packager
    - #2. 配置cargo-packager
  - #各个桌面端平台打包
    - #1. Linux (Debian/Ubuntu)
    - #2. Windows(Windows-2022)
    - #3. MacOS
- #二、移动平台打包

打包 Makepad 项目的要求取决于目标平台。本指南提供了如何为不同平台（包括桌面和移动平台）打包 Makepad 应用程序的概述。

Makepad 官方并没有提供为桌面环境打包的工具， 因此我们使用 cargo-packager这一常用的社区打包工具来打包您的 Makepad 应用程序。

此工具帮助您为 Makepad 应用程序创建包，包括为不同平台生成必要的文件和目录。

更多可配置的参数详见cargo-packager的官方文档：https://docs.crabnebula.dev/packager/#configuration

本指南选择使用cargo-packager大部分应用配置选择的位置： Cargo.toml 中作为示例：

我们使用小型的 robius-packaging-commands CLI 工具在打包 Makepad 应用程序之前运行打包命令。

您可以使用以下命令安装 robius-packaging-commands 工具：

注意robius-packaging-commands工具的版本为 0.2.0

如果您不想使用 robius-packaging-commands 工具，您也可以手动自己编写打包命令，将你的资源和 makepad 按照你自己的逻辑导入也可以，但如果是单纯的资源的导入建议你使用该工具以方便操作。

事实上robius-packaging-commands工具就是完成以上的导入工作，如果你实在是需要自定义顺序的导入，才建议你重新一个新的库钩子在cargo-packager运行时进行资源的自定义处理。

如果您已经按照上述说明配置了 Cargo.toml 文件，您可以运行打包命令：

如果您已经按照上述说明配置了 Cargo.toml 文件，您可以运行打包命令：

查看更多关于 robius-packaging-commands 的信息：[https://github.com/project-robius/robius-packaging-commands/blob/main/README.md]

幸运的是，在移动平台上打包 Makepad 并不需要像桌面端一样复杂，因为 makepad 官方就提供了 cargo-makepad 作为移动平台应用打包的工具。

Android 可以使用 cargo-makepad 轻松打包：

使用 cargo-makepad 构建 iOS 应用：

由于 cargo-makepad 并没有单独的 build 命令，您可以直接使用 run-sim 或 run-device 命令来构建应用程序, 然后进行打包。

Makepad 还支持将应用程序编译为 WebAssembly (Wasm)，这使得您的应用可以在浏览器中运行。

要为 Wasm 打包应用程序，您可以使用以下命令：

这将构建您的 Makepad 应用程序为 Wasm 模块，并在浏览器中运行, 同时在/target/makepad-wasm-app/release/your_profile_name/目录下生成相应的 HTML 和 JavaScript 文件。

它包括：makepad静态资源和您的应用程序的资源文件, 以及相关的js_bridge文件以及 wasm 模块。

**Examples:**

Example 1 (bash):
```bash
1cargo install cargo-packager --locked
```

Example 2 (unknown):
```unknown
1[package.metadata.packager]
2product_name = "YourAppName"
3identifier = "com.yourcompany.yourapp"
4authors = ["Your Name or Team name"]
5description = "A brief description of your Makepad application"
6### 注意：`long_description` 的每行最多 80 个字符。
7long_description = "..."
8icons = ["./assets/icon.png"]
9out_dir = "./dist"
10# ... 其他打包选项，请查看 cargo-packager 文档了解更多详情。
11
12before-packaging-command = """
13robius-packaging-commands before-packaging \
14    --force-makepad \
15    --binary-name <main-binary-name> \
16    --path-to-binary ./target/release/<main-binary-name>
17"""
18
19# 我们需要指定将包含在包中的资源，打包器将把它们复制到输出目录。
20# 注意：如果您使用的是 Makepad v1.0 或更高版本，则需要指定更多资源文件：
21resources = [
22    ######### ⚠️ Makepad 自带的资源文件，当前情况下你同样需要导入 #########
23    { src = "./dist/resources/makepad_widgets", target = "makepad_widgets" },
24    { src = "./dist/resources/makepad_fonts_chinese_bold", target = "makepad_fonts_chinese_bold" },
25    { src = "./dist/resources/makepad_fonts_chinese_bold_2", target = "makepad_fonts_chinese_bold_2" },
26    { src = "./dist/resources/makepad_fonts_chinese_regular", target = "makepad_fonts_chinese_regular" },
27    { src = "./dist/resources/makepad_fonts_chinese_regular_2", target = "makepad_fonts_chinese_regular_2" },
28    { src = "./dist/resources/makepad_fonts_emoji", target = "makepad_fonts_emoji" },
29
30    ######### ⚠️ 此处是你自己项目中的资源文件 #########
31    { src = "./dist/resources/you_app_resource", target = "you_app_resource" },
32]
33
34before-each-package-command = """
35robius-packaging-commands before-each-package \
36    --force-makepad \
37    --binary-name <main-binary-name> \
38    --path-to-binary ./target/release/<main-binary-name> \
39"""
```

Example 3 (bash):
```bash
1cargo install --version 0.2.0 --locked --git https://github.com/project-robius/robius-packaging-commands.git robius-packaging-commands
```

Example 4 (go):
```go
1# packaging/before-packaging-command 你自己的编写的导入资源的库工具 ⚠️ 你依旧需要自己处理 makepad 的资源导入，列如字体。
2[package.metadata.packager]
3
4before-packaging-command = """
5cargo run --manifest-path packaging/before-packaging-command/Cargo.toml before-packaging
6"""
7
8before-each-package-command = """
9cargo run --manifest-path packaging/before-packaging-command/Cargo.toml before-each-package
10"""
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/shading-language/shader-skill

**Contents:**
- #着色技巧详解
- #颜色与渐变
  - #基本颜色表示
  - #线性渐变
  - #径向渐变
- #纹理映射
  - #基础纹理采样
  - #UV坐标系统
- #特效处理
  - #高斯模糊

在深入了解着色器中的颜色处理之前，让我们先建立对颜色在计算机图形学中的基本认知

在 Makepad 着色器中，我们使用 vec4 (RGBA向量) 来表示颜色：

线性渐变是在两种颜色之间创建平滑过渡的技术。以下是一个水平渐变的实现：

径向渐变创建从中心点向外扩散的圆形颜色过渡：

纹理映射是将 2D 图像应用到几何图形表面的技术。

**Examples:**

Example 1 (rust):
```rust
1// 基础纯色着色器
2fn pixel(self) -> vec4 {
3    // 格式: vec4(红, 绿, 蓝, 透明度)
4    return vec4(1.0, 0.0, 0.0, 1.0); // 纯红色
5}
```

Example 2 (rust):
```rust
1fn pixel(self) -> vec4 {
2    // self.pos.x 给出从0.0到1.0的水平位置
3    let mix_factor = self.pos.x;
4
5    let color1 = vec4(1.0, 0.0, 0.0, 1.0); // 红色
6    let color2 = vec4(0.0, 0.0, 1.0, 1.0); // 蓝色
7
8    // 在两种颜色之间线性插值
9    return mix(color1, color2, mix_factor);
10}
```

Example 3 (r):
```r
1红色 [================>] 蓝色
2        <- 混合因子 ->
```

Example 4 (rust):
```rust
1fn pixel(self) -> vec4 {
2    // 组合x和y坐标实现对角线方向
3    let mix_factor = (self.pos.x + self.pos.y) * 0.5;
4
5    let color1 = vec4(1.0, 0.0, 0.0, 1.0); // 红色
6    let color2 = vec4(0.0, 0.0, 1.0, 1.0); // 蓝色
7
8    return mix(color1, color2, mix_factor);
9}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/adding-state

**Contents:**
- #6 - 添加状态
- #你将学到什么
  - #定义 State 结构体
- #实现 Default trait
- #添加辅助函数
- #更新 App 结构体
- #在 Widgets 中使用 State
  - #在 App 中使用 State
  - #在 ImageGrid 中使用 State
  - #在 ImageRow 中使用 State

在之前的几个步骤中，我们为应用程序创建了一个图像网格的初始实现。

为了保持初始实现的简单性，我们的图像网格存在以下限制：

在接下来的几个步骤中，我们将去除这些限制，使图像网格变得更加动态。

在本步骤中，我们将从为应用程序添加状态开始。

注意： 如果你不想手动输入代码，可以在此处找到本步骤的全部代码：https://github.com/makepad/image_viewer/tree/main/step_6

我们首先定义一个结构体，用于存储图像网格所需的状态。

这段代码定义了一个名为 State 的结构体，其中包含以下两个字段：

接下来，我们将为 State 结构体实现 Default trait。 为 State 结构体定义 Default trait 可以让它作为 App 结构体中的一个字段使用。

这段代码将创建一个 State 实例，其字段值与我们之前硬编码的值相同：

为了让后续的代码更易于编写，我们将在 State 结构体中添加一些辅助函数。

注意： 通常每行的图像数量由 State 结构体中的 max_images_per_row 字段决定，但在最后一行中，如果剩余的图像数量少于该值，那么这一行的图像数量也会相应减少。

现在我们已经创建了 State 结构体，接下来将它添加到 App 结构体中。

在 app.rs 中，用下面的定义替换 live design 块中的 App 定义：

这将为 App 结构体添加一个名为 state 的字段，其类型为 State 结构体的实例。

请注意，我们使用了 #[rust] 属性来标记 state 字段为普通的 Rust 字段。回忆一下，当 live design 系统遇到带有 #[rust] 标记的字段时，它会使用该类型的 Default::default 构造函数来初始化该字段。这正是我们之前为 State 结构体实现 Default trait 的原因。

现在我们已经有了用于存储应用状态的位置，接下来来看看如何在应用中的小部件（widgets）中使用这个状态。

我们现在面临的直接问题是：状态保存在 App 结构体中，但我们的各个小部件却无法访问这个状态。为了让小部件能够访问状态，我们需要将状态传递给整个小部件树（widget tree）。

让我们先来看一下当前 App 结构体中 AppMain trait 的实现：

注意我们目前使用的是 Scope::empty，为每个事件创建了一个空的作用域（scope）。回忆一下，在 Makepad 中，scope 是一个容器，它用于在事件传递过程中传递应用级数据（可变的）以及小部件的 props（不可变的）。

之前，我们还没有状态，因此只是简单地创建了空的作用域。而现在我们有了实际的状态数据，是时候做出修改了。

在 app.rs 中，用下面的代码替换 AppMain trait 的实现：

这段代码使用 Scope::with_data 创建了一个包含对 state 字段的可变引用的新作用域。然后将这个作用域传递给我们小部件树的根部。从这里开始，它会自动向下传递到 ImageGrid，我们就可以在其中访问状态；接着，状态还会进一步传递到 ImageRow 中。

接下来，我们将在 ImageGrid 中使用状态。

在 app.rs 中，用下面的代码替换 ImageGrid 结构体中 Widget trait 的 draw_walk 方法的实现：

在这个新实现中，我们首先从作用域（scope）中获取状态：

接着，我们从状态中获取行数，并告诉列表我们需要这么多项目：

最后，我们使用 Scope::with_data_props 创建一个新的作用域，其中包含对状态的可变引用以及当前行索引的引用，然后将这个新的作用域传递给当前的 ImageRow：

是必要的，因为 set_item_range 只是指定了项目的最小数量：next_visible_item 可能会返回更多的项目，如果界面空间允许的话。当我们只绘制占位图时，这不会有问题，但现在我们是根据索引访问数组元素，如果项目数量超出预期，可能会导致数组越界错误，所以必须对这个情况进行保护。

最后，我们将在 ImageRow 中使用状态。

在 app.rs 中，用下面的代码替换 ImageRow 结构体中 Widget trait 的 draw_walk 方法的实现：

这段代码同样会从作用域中获取状态和当前行索引，并根据状态中每行图像的数量，动态绘制该行的所有图片。

在这个新实现中，我们首先从作用域（scope）中获取状态和当前行索引：

接着，我们从状态中获取当前行的图像数量，并告诉列表我们需要这么多项目：

是必须的，用来确保项目数量不会超过预期（因为 set_item_range 只指定了项目的最小数量）。

确保你当前处于你的包目录下，然后运行以下命令：

如果一切正常，你的屏幕上应该会出现一个窗口，里面显示的占位图像网格与之前一样：

不同的是，现在图像网格是基于状态动态绘制的，而不是硬编码的值。为了说明这一点，我们暂时将 State 结构体中 Default trait 的实现改成如下代码：

重新编译后，屏幕上显示的占位图像网格应该会变成如下样子：

我们现在已经拥有了绘制图像网格所需的状态。下一步，我们将使用这些状态来加载真实的图片。

**Examples:**

Example 1 (rust):
```rust
1#[derive(Debug)]
2pub struct State {
3    num_images: usize,
4    max_images_per_row: usize,
5}
```

Example 2 (rust):
```rust
1impl Default for State {
2    fn default() -> Self {
3        Self {
4            num_images: 12,
5            max_images_per_row: 4,
6        }
7    }
8}
```

Example 3 (rust):
```rust
1impl State {
2    fn num_images(&self) -> usize {
3        self.num_images
4    }
5
6    fn num_rows(&self) -> usize {
7        self.num_images().div_ceil(self.max_images_per_row)
8    }
9
10    fn first_image_idx_for_row(&self, row_idx: usize) -> usize {
11        row_idx * self.max_images_per_row
12    }
13
14    fn num_images_for_row(&self, row_idx: usize) -> usize {
15        let first_image_idx = self.first_image_idx_for_row(row_idx);
16        let num_remaining_images = self.num_images() - first_image_idx;
17        self.max_images_per_row.min(num_remaining_images)
18    }
19}
```

Example 4 (rust):
```rust
1#[derive(Live, LiveHook)]
2pub struct App {
3    #[live]
4    ui: WidgetRef,
5    #[rust]
6    state: State,
7}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/widget/widget-render

**Contents:**
- #Widget 渲染机制
- #绘制流程控制
  - #DrawStep 状态机
    - #1.支持增量渲染
    - #2.状态保持
    - #3.控制流管理
    - #4.内存效率
  - #绘制流程
  - #绘制模型
  - #绘制区域（Area）管理

下面通过一个简单 Button widget 来说明绘制流程：

Makepad 采用了一个复杂但高效的延迟绘制系统。

这个系统的核心特点是:绘制命令不会立即执行，而是被收集到绘制列表(DrawList)中，最后统一处理。

DrawList 是 Makepad 绘制系统的核心。让我们看看它的结构：

当我们调用 redraw 命令时，实际发生的是：

比如 Makepad 内置的 View Widget ，就用了 ViewOptimize 优化。

1.DrawList 模式会为视图创建一个独立的绘制列表，这个列表可以被缓存和重用。

2.Texture 模式会将整个视图渲染到一个纹理中，然后将这个纹理作为一个整体来使用。

在 Makepad 中，区域管理的核心是 Area 类型，它用于跟踪和管理 widget 在屏幕上的绘制区域。

每个 Widget 都有一个关联的 Area，这个 Area 不仅用于确定绘制位置，还用于事件处理（Event）和命中（Hit）检测。

裁剪实现细节核心的裁剪功能在 Area 源码中的 clipped_rect() 方法中实现。

下面是一个简单的 Button 事件处理的示例：

Makepad 分为主 UI 线程和其他多个 Worker 线程。

Makepad 提供 Event 机制，用于自底向上传播(由系统/框架分发给组件)来自系统底层的事件(如鼠标、键盘、触摸等)。

此外，Makepad 也提供 Action 机制，用于自顶向下传播(由组件发送给父组件/监听者)的内部业务动作。

这些 Action 是可以同步也可以异步。

总结 Event 和 Action 区别：

Makepad 通过 widget_action 提供了一个统一的 Action 发送和处理机制，并且 Action 可以携带数据和状态。

组件通过 widget_action 发送 Action：

Action 被收集到 context 的 action buffer :

这套机制让 Makepad 的组件能够灵活地进行状态传递和事件通信,同时保持了良好的解耦性和可维护性。

因为 Widget trait 中的 handle_event 主要关注两个方面:

但实际有很多事件是与 Area 无关的，比如:

如果这些事件都在每个 Widget 中处理的话， match event 分支就会很冗余。

所以，Makepad 提供 MatchEvent trait，来提供一系列默认实现的方法，让代码更清晰:

这样组件就可以专注于实现自己关注的事件处理逻辑，而不用写大量的匹配代码。

**Examples:**

Example 1 (rust):
```rust
1pub trait DrawStepApi {
2    fn done() -> DrawStep {
3        Result::Ok(())
4    }
5    fn make_step_here(arg: WidgetRef) -> DrawStep {
6        Result::Err(arg)
7    }
8    fn make_step() -> DrawStep {
9        Result::Err(WidgetRef::empty())
10    }
11    fn is_done(&self) -> bool;
12    fn is_step(&self) -> bool;
13    fn step(self) -> Option<WidgetRef>;
14}
15
16impl DrawStepApi for DrawStep {
17    fn is_done(&self) -> bool {
18        match *self {
19            Result::Ok(_) => true,
20            Result::Err(_) => false,
21        }
22    }
23    fn is_step(&self) -> bool {
24        match *self {
25            Result::Ok(_) => false,
26            Result::Err(_) => true,
27        }
28    }
29
30    fn step(self) -> Option<WidgetRef> {
31        match self {
32            Result::Ok(_) => None,
33            Result::Err(nd) => Some(nd),
34        }
35    }
36}
37
38pub type DrawStep = Result<(), WidgetRef>;
```

Example 2 (rust):
```rust
1pub struct Button {
2    // 绘制状态机
3    #[rust] draw_state: DrawStateWrap<DrawState>,
4    // 布局信息
5    #[layout] layout: Layout,
6    // 定位信息
7    #[walk] walk: Walk,
8    // 背景绘制
9    #[live] draw_bg: DrawColor,
10    // 文本绘制
11    #[live] draw_text: DrawText,
12}
13
14impl Widget for Button {
15    fn draw_walk(&mut self, cx: &mut Cx2d, scope: &mut Scope, walk: Walk) -> DrawStep {
16        // 1. 初始化绘制状态
17        if self.draw_state.begin(cx, DrawState::DrawBackground) {
18            // 开始布局计算
19            self.draw_bg.begin(cx, walk, self.layout);
20            return DrawStep::make_step();
21        }
22
23        // 2. 绘制背景
24        if let Some(DrawState::DrawBackground) = self.draw_state.get() {
25            self.draw_bg.end(cx);
26            // 切换到文本绘制状态
27            self.draw_state.set(DrawState::DrawText);
28            return DrawStep::make_step();
29        }
30
31        // 3. 绘制文本
32        if let Some(DrawState::DrawText) = self.draw_state.get() {
33            let text_walk = Walk::size(Size::Fill, Size::Fit);
34            self.draw_text.draw_walk(cx, scope, text_walk)?;
35            self.draw_state.end();
36        }
37
38        DrawStep::done()
39    }
40}
41
42// 绘制状态枚举
43#[derive(Clone)]
44enum DrawState {
45    DrawBackground,
46    DrawText
47}
```

Example 3 (rust):
```rust
1// 设置布局
2self.draw_bg.begin(cx, walk, self.layout);
3
4// Turtle 会自动处理:
5// - 元素位置计算
6// - Margin/Padding 处理
7// - 子元素布局
```

Example 4 (rust):
```rust
1// 返回继续标记
2return DrawStep::make_step();
3
4// 返回完成标记
5return DrawStep::done();
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/start/makepad-framework-architecture

**Contents:**
- #Makepad Framework 架构
- #Makepad Platform
  - #代码组织结构
  - #Live 系统
    - #MPSL 着色器 DSL 语言
    - #为什么不使用脚本语言？
  - #绘图层
- #Makepad Widget

Makepad Framework 主要包含两大部分：

Makepad Platform 是平台抽象层。这意味着，它对跨平台的各种操作做了抽象和封装。

platform 的代码组织结构分为如下部分：

源码地址：https://github.com/makepad/makepad/tree/dev/platform

在 Makepad 的 platform/src 目录下，我们找到了多个 Rust 源文件（.rs 文件），以及一些子目录，大概分类如下。

看得出来，src 中主要还是封装了与 事件、UI 渲染、音视频、网络等相关跨平台的底层接口。我们暂且不去深入细节，接下来了解下什么是 Live 系统。

Live 系统是 makepad 提供的一种实时系统。因为 Rust 语言是静态编译语言，不像动态语言那样能实现运行时实时编码，所以 makepad 提供这套 Live 系统来实现实时编码的效果。正如第一章中对 makepad 愿景展示视频里所见，对代码的实时修改，无需重新编译代码，即可更新 UI。

目前，使用 makepad studio 和 vscode，可以看到实时重载（live reload）所见即所得的效果，后续支持更多 IDE 和 编辑器。

Live 系统是依赖于 Rust 的宏来实现的。这里有一个 Live 语法示例：

上面这段代码的作用是定义了一个 可设置颜色的 按钮（Button）。

其中的 live_design! 、#[derive(Live)] 和 #[live] 这些过程宏就属于 Live 系统，它们共同作用于 Button 结构体，就创建了这个「按钮」小部件（Widget）。

#[derive(Live)] 是 Rust 中的派生宏，它会自动为 Button 结构体实现 Live trait，这是 「按钮」控件能与 Live 系统的交互需要的所有必要的机制。

#[live] 属性定义了 Button 结构体的那个字段要参与实时更新，这里是颜色（color）字段。

live_design! 宏则像是 CSS 样式那样，为「按钮」小部件定义了基本样式，比如设置了默认的颜色为 #f00 。

我们暂且不去深入 Live 系统的工作原理，我们只需要理解这套系统的作用和其语法即可。

具体更多细节将在后面Live DSL章节介绍。

Live 系统还包含了 Makepad 自定义的着色器语言 MPSL (Makepad’s custom shader language)，它是一种统一的语言，可以转换为图形 API （openGL/ DirectX / Metal）的着色器语言。

MPSL 代码是可以嵌入到 Live DSL 代码中的。其中 DrawColor 的字段 draw_super 类型 DrawQuad ，代表该结构体继承了 DrawQuad 的行为 （为这个字段实现了 deref trait）。这算是一个 Rust/C 中的技巧，算是一种行为代理。DrawQuad 是 Makepad 框架中 draw crate 提供的功能，代表了绘制任意四边形的基本着色器。

利用 Deref trait 来实现继承，被认为是一种反模式。

因为 Deref 是隐式行为，这和 Rust 的显式哲学是相反的，滥用它容易出错。 但是这里的例子是 OK 的，因为这种方法可以方便地利用框架本身的能力，不属于滥用。

示例代码中 #[repr(C)] 也是 MPSL 着色器语言的硬性要求，这是为结构体指定了兼容 C-ABI 的布局，目的是为了阻止 Rust 编译器对字段重新排序。

同样，本章不会去深入着色器语法和其渲染机制的更多细节，您看查看着色语言章节介绍。

你可能会对 Live System 产生这样的疑问：为什么不用 javascript/ lua ，亦或像 slint 那样自己开发一套脚本语言呢，为什么一定要使用现在这种 Rust 宏的写法？

用脚本语言编写的代码可以在运行中进行解释或重新编译，从而让开发周期变得更短。遗憾的是，脚本语言缺乏编译语言的性能优势，这又与 Makepad 力求支撑的重渲染应用产生冲突。将脚本语言和 Rust 结合使用时，代码的归属又极难平衡，因为最佳方案总是因应用程序而异的。

为了解决这一困境才选择使用现在的混合方法的方案：

Makepad 应用程序使用 Rust 编写，但负责应用程序外观（比如样式）的部分则用 Live DSL 编写。

Live 语言的主要作用类似于 CSS：它描述了应用程序的用户界面应该如何呈现。特别是与 CSS 相似，Live 语言可以方便地覆盖样式。

相较于 Rust 代码，Live 代码是在运行时编译和执行的。

另外，将来为 Makepad 构建的可视化设计器（visual designer，比如 Makepad studio 之类）能感知代码中哪些部分是 Live 代码。当使用可视化设计器来修改一段 Live 代码时，可视化设计器不会重新编译应用程序，而是将更新后的 Live 代码发送给运行中的应用程序。这样，应用程序就可以重新编译并执行新的 Live 代码，从而立刻反映出代码中的变化而无需重新编译 Rust 代码或者重启应用程序。

Makepad 的 2D 和 3D 绘图层（drawing layer）也包含在 Makepad platform 中。

与 Makepad Platform 并行存在的另一部分是 Makepad Widget 。

Makepad Widget 构建在 makepad draw 之上。它包含以下内容：

makepad-widget 的 src 组织结构中包含了很多文件，代表了不同的 UI 控件大概可以作如下分类：

更多细节我们在后面 Makepad Widget 章节中介绍。

**Examples:**

Example 1 (rust):
```rust
1live_design! {
2    Button = {{Button}} {
3		color: #f00
4    }
5}
6
7#[derive(Live)]
8pub struct Button {
9	#[live] color: DVec4,
10}
```

Example 2 (rust):
```rust
1live_design!{
2		DrawColor = {{DrawColor}} {
3				fn pixel(self) -> vec4 {
4					return vec4(self.color.rgb * self.color.a, self.color.a);
5				}
6		}
7}
8
9#[derive(Live)]
10#[repr(C)]
11pub struct DrawColor {
12		#[deref] pub draw_super: DrawQuad,
13		#[live] pub color: Vec4,
14}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/quick-start/image-viewer-app/create-a-modaltip

**Contents:**
- #18 - 创建模态框
- #你将学到的内容
- #定义模态框（AlertDialog）
  - #调试技巧
- #更新根组件
- #更新 State 结构体
  - #更新 State 结构体的 Default Trait 实现
- #增加控制方法
  - #将模态控制方法绑定到按钮
- #增加事件

在前面的步骤中。我们完成了整个应用的全部功能。接下来，我们将在目前功能的基础上，扩展一些内容，使得我们的应用给更加完善。在这一章中，我们将增加一个模态提示的功能。

这一步中。我们将实现一个模态框，并且学习如何触发和关闭模态框。

注意: 如果你不想自己敲代码，可以在这里找到本步骤的完整代码： https://github.com/Project-Robius-China/image-viewer-workshop

让我们先为模态框（AlertDialog）增加一个定义。模态框是覆盖在父窗体上的子窗体，可以在不离开父窗体的情况下与用户有一些互动。

在 app.rs 中，找到 live design 块，在 SearchBox 定义之前，添加以下代码：

该定义创建了一个具有以下属性的 AlertDialog :

关于 MPSL 着色语言的部分，这里不详细展开，在本教程 着色语言 部分有详细介绍。

这里讲解一下背景值：Alpha 值的范围是 0.0 (完全透明) 到 1.0 (完全不透明)。十六进制的 cc 转换为十进制是 204。Alpha 的计算方式是 十进制值 / 255。所以 204 / 255 ≈ 0.8。如果你想让它更透明，你需要一个更小的 alpha 值。例如，如果你想要大约 50% 的透明度：255 * 0.5 = 127.5。最接近的十六进制值是 80 (十进制 128)。所以颜色会是 #00000080。如果你想要大约 30% 的透明度：255 * 0.3 ≈ 76.5。最接近的十六进制值是 4D (十进制 77) 或 4C (十进制 76)。所以颜色会是 #0000004D 或 #0000004C。

为 <Modal> 组件 content 字段赋值新的 <View> 来定义模态框中间的显示区域。

中间显示文字的区域使用 <RoundedView> 并命名为 dialog。

以上我们给出的代码都为正确的代码，在日常开发中，难免会遇到一些 DSL 字段错误的问题，目前 Makepad 还没有 LSP 的支持，但 Makepad 提供了提示错误信息，我们可以通过终端的提示来确定错误。

比如将这段代码添加到 AlertDialog 定义中：

在 <Modal> 的属性定义中，并没有 color 的字段，当我们保存更改时，终端提示报错：

提示我们在 app.rs 下的第19行第9列没有匹配的字段 color，之后我们可以通过核对对应的属性，对这个字段进行修改。

注意: 如果在保存文件后，并没有出现提示，并且在修改其他属性代码界面并没有出现变化时，请重新运行程序，这样报错系统会重新加载。

在 app.rs 文件中，找到跟组件 App，用下面这段代码替换原来的代码:

因为我们想要的效果是使用模态窗覆盖整个窗口，所以将我们刚定义的组件 AlertDialog 引入根组件使用。

在 app.rs 文件中，用下面的代码替换 State 结构体的定义：

我们还需要更新 State 结构体的 Default trait 实现，以反映这个新字段。

在 app.rs 文件中，用下面的代码替换 State 结构体的 Default trait 实现：

我们将在 App 结构体中增加控制模态框打开，关闭方法。

在 app.rs 文件中，添加以下代码到 App 结构体的 impl 块中：

show_alert 方法控制模态框的开启：

close_alert 方法控制模态框的关闭，其中内容与开启时类似，这里不加赘述。

回到控制图片左右导航的按钮的代码下，将一下代码替换到原始的代码：

其实只是在 navigate_left 和 navigate_right 方法中增加了：

的代码，确保在图片预览到最左侧和最右侧没有图片时，打开模态框进行提示。

目前我们已经确保模态框能在用户需要的时候打开。同样，我们也需要绑定事件来关闭模态框。

在 handle_actions 中添加：

Modal 组件中的 dismissed() 函数确保了用户鼠标点击模态框中的透明背景区域或者键盘上按 Esc 键时响应，只需要在这两个事件响应时，调用上面我们定义的 close_alert() 函数关闭模态框即可。

在 Makepad 组件的设计哲学中，将非用户触发的系统事件例如：鼠标事件，按键事件。都封装在了 Widget 组件内容，如果想要详细了解内容如何响应，请查看 组件的实现 ，这里不加赘述。

如果一切正常，在放映模式下第一张图片时点击向左按钮或者在最后一张图片时点击向右按钮，弹出模态框提示。 点击透明背景或者按下 Esc 键模态关闭。

在这一步中，我们为程序实现了模态窗。下一步，我们将会把应用模块进行拆分。

**Examples:**

Example 1 (rust):
```rust
1AlertDialog = <Modal> {
2        width: Fill
3        height: Fill
4
5        bg_view: <View> {
6            width: Fill
7            height: Fill
8            show_bg: true
9            draw_bg: {
10                fn pixel(self) -> vec4 {
11                    return #00000080;
12                }
13            }
14        }
15
16        content: <View> {
17            width: Fit
18            height: Fit
19            align: {x: 0.5, y: 0.5}
20            padding: 20
21            flow: Down
22
23            draw_bg: {
24                color: #333
25            }
26
27            dialog = <RoundedView> {
28                width: 300
29                height: 150
30                align: {x: 0.5, y: 0.5}
31                draw_bg: {
32                    color: #333
33                    border_color: #555
34                    border_size: 1.0
35                    border_radius: 4.0
36                }
37                padding: 20
38
39
40                message = <Label> {
41                    width: Fill
42                    height: Fit
43                    align: {x: 0.5}
44                    margin: {bottom: 20}
45                    draw_text: {
46                        text_style: { font_size: 12.0 }
47                        color: #fff
48                    }
49                    text: "默认消息"
50                }
51            }
52        }
53    }
```

Example 2 (rust):
```rust
1color: #0000
```

Example 3 (json):
```json
1Apply error: src/app.rs:19:9 - no matching field: color
2   origin: C:\Users\10245\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\makepad-platform-1.0.0\src\live_traits.rs:20  Color(0)
```

Example 4 (rust):
```rust
1App = {{App}} {
2        placeholder: (PLACEHOLDER),
3
4        ui: <Root> {
5            <Window> {
6                body = <View> {
7                    flow: Overlay,
8
9                    page_flip = <PageFlip> {
10                        active_page: image_browser,
11
12                        image_browser = <ImageBrowser> {}
13                        slideshow = <Slideshow> {}
14                    }
15
16                    alert_dialog = <AlertDialog> {}
17                }
18            }
19        }
20    }
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/layout-system/circular-dependency

**Contents:**
- #布局中产生的循环依赖问题
- #布局循环依赖
  - #父子循环依赖
  - #兄弟组件循环依赖
- #如何避免布局循环依赖
  - #1. 明确尺寸约束
  - #2. 使用 Fixed 或 Max/Min 约束
  - #3. Layout 方向性原则
- #当遇到潜在循环依赖时布局引擎的解决策略
  - #1. 优先级顺序

布局循环依赖是指在组件布局计算过程中出现的相互依赖关系，导致布局引擎无法确定最终尺寸的情况。这通常发生在以下场景：

当布局引擎检测到潜在的循环依赖时，会采取以下策略：

**Examples:**

Example 1 (rust):
```rust
1// 错误示例：父子循环依赖
2ParentView = <View> {
3    walk: {
4        width: Fill,
5        height: Fit  // 父组件高度依赖子组件
6    },
7    layout: {
8        flow: Down
9    },
10
11    <ChildView> {
12        walk: {
13            width: Fill,
14            height: Percentage(0.5)  // 子组件高度依赖父组件
15        }
16    }
17}
```

Example 2 (unknown):
```unknown
1┌─ Parent (height: Fit) ─┐
2│     ?                  │
3│  ┌─ Child ─┐           │
4│  │  height:│           │
5│  │  50%    │   ?       │
6│  │  of ?   │           │
7│  └─────────┘           │
8└────────────────────────┘
```

Example 3 (rust):
```rust
1// 错误示例：兄弟组件循环依赖
2Container = <View> {
3    layout: {
4        flow: Right
5    },
6
7    <LeftView> {
8        walk: {
9            width: Fill,  // 左侧组件填充剩余空间
10            height: Fit
11        }
12    }
13
14    <RightView> {
15        walk: {
16            width: Fit,   // 右侧组件宽度自适应
17            height: Fit
18        },
19        layout: {
20            flow: Down
21        }
22    }
23}
```

Example 4 (rust):
```rust
1// 正确示例：明确尺寸约束
2ParentView = <View> {
3    walk: {
4        width: Fill,
5        height: Fixed(500)  // 明确父组件高度
6    },
7
8    <ChildView> {
9        walk: {
10            width: Fill,
11            height: Percentage(0.5)  // 现在可以正确计算
12        }
13    }
14}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/live-DSL/type-system

**Contents:**
- #类型系统
- #LiveValue 类型
  - #字符串相关类型
  - #结构组织相关
  - #DSL 相关
  - #模块系统
  - #特殊值处理
- #扩展阅读：序列化与反序列化格式
  - #1. 编码紧凑
  - #2. 自描述和类型安全

了解 Live DSL 中的类型系统，有助于编写和调试 Live 代码。

LiveValue 中这些特殊的变体代表了 Live DSL 中支持的类型：

编译器会根据上下文和使用场景自动选择最合适的字符串表示形式。

Makepad Live 值在运行时会经过序列化和反序列化的过程。为此，Makepad 采用了 CBOR (Concise Binary Object Representation，简明二进制对象表示法) 格式。

CBOR 有几个关键特点使其非常适合 Makepad:

总的来说，CBOR 为 Makepad 提供了:

**Examples:**

Example 1 (rust):
```rust
1#[derive(Clone, Debug, PartialEq)]
2pub enum LiveValue {
3    None,
4    // string types
5    Str(&'static str),
6    String(Arc),
7    InlineString(InlineString),
8    Dependency(Arc),
9    Bool(bool),
10    Int64(i64),
11    Uint64(u64),
12    Float32(f32),
13    Float64(f64),
14    Color(u32),
15    Vec2(Vec2),
16    Vec3(Vec3),
17    Vec4(Vec4),
18    Id(LiveId),
19    IdPath(Arc>),
20    ExprBinOp(LiveBinOp),
21    ExprUnOp(LiveUnOp),
22    ExprMember(LiveId),
23    ExprCall {ident: LiveId, args: usize},
24     // enum thing
25    BareEnum (LiveId),
26    // tree items
27    Root{id_resolve:Box>},
28    Array,
29    Expr,// {expand_index: Option},
30    TupleEnum (LiveId),
31    NamedEnum (LiveId),
32    Object,
33    Clone{clone:LiveId, design_info:LiveDesignInfoIndex},
34    Deref{live_type: LiveType, clone:LiveId, design_info:LiveDesignInfoIndex},
35    Class {live_type: LiveType, class_parent: LivePtr, design_info:LiveDesignInfoIndex},
36    Close,
37
38    // shader code and other DSLs
39    DSL {
40        token_start: u32,
41        token_count: u32,
42        expand_index: Option
43    },
44    Import (Box),
45}
```

Example 2 (rust):
```rust
1Component = {
2    // 基础类型的使用
3    name: "Component",         // Str
4    enabled: true,            // Bool
5    width: 100,              // Int64
6    height: 50.5,            // Float64
7    color: #ff0000,          // Color
8    position: vec2(10, 20),  // Vec2
9
10    // 资源依赖
11    icon: dep("crate://self/icons/home.svg"),  // Dependency
12
13    // 表达式
14    scale: (base_size * 2),   // ExprBinOp
15    visible: (!hidden),       // ExprUnOp
16
17    // 枚举
18    status: Active,           // BareEnum
19    point: Point(10, 20),     // TupleEnum
20    user: User {              // NamedEnum
21        name: "John",
22        age: 30
23    },
24}
```

Example 3 (rust):
```rust
1// 解码字符串后,根据长度选择合适的 LiveValue 类型
2if let Some(v) = decode_str(data, &mut o)? {
3    let value = if let Some(inline_str) = InlineString::from_str(v) {
4        // 如果字符串够短,使用 InlineString
5        LiveValue::InlineString(inline_str)
6    } else {
7        // 否则使用 Arc<String>
8        LiveValue::String(Arc::new(v.to_string()))
9    };
10    self.push(LiveNode {id, origin, value});
11}
12
13// example
14struct Button {
15    id: InlineString,  // 栈上固定大小
16    text: Arc<String>, // 共享堆内存
17    class: &'static str, // 零开销
18}
```

Example 4 (rust):
```rust
1live_design!{
2    // 导入其他模块
3    use link::button::Button; // 生成 Import 变体
4
5	// LiveValue::Class 创建一个组件
6    App = {{App}} {
7        ui: <Root> {
8            flow: Right,
9            // Object 变体开始
10            buttons: {
11                save_button: <Button> {
12                    text: "Save"
13                    onClick: Save
14                }
15                cancel_button: <Button> {
16                    text: "Cancel"
17                }
18            } // Close 变体结束
19
20            // Array 变体示例
21            colors: [
22                #f00,
23                #0f0,
24                #00f
25            ]
26        }
27    }
28
29
30	// 根节点下定义各种组件
31    MyButton = <Button> {
32        // Button 的类型信息会被编译为 LiveValue::Deref
33        width: 100
34        height: 40
35        draw_bg: {color: #f00}
36    }
37
38    // 克隆已有组件
39    DefaultButton = <Button>{}  // 生成 Clone 变体
40
41    // 使用解引用
42    ButtonRef = <ButtonBase> {  // 生成 Deref 变体
43        walk: {width: Fill}
44    }
45
46
47}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/animation-system/best-practice

**Contents:**
- #最佳实践与优化

在实现 Makepad 动画时，要把握两个原则：可控与平滑。

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/layout-system/turtle-draw-layout

**Contents:**
- #Turtle Draw 布局系统
- #基本概念
- #基本绘制流程
- #位置计算实例
- #复杂布局示例
  - #嵌套布局
  - #延迟布局（Defer）机制
- #Turtle 布局对齐系统
  - #对齐方式
  - #实际对齐示例

当布局需要等待某些条件满足时，可以使用延迟布局：

**Examples:**

Example 1 (rust):
```rust
1pub struct Turtle {
2    // 当前海龟位置
3    walk: Walk,         // 布局行为
4    layout: Layout,     // 布局规则
5    wrap_spacing: f64,  // 换行间距
6    align_start: usize, // 对齐开始位置
7    defer_count: usize, // 延迟计数
8    shift: DVec2,      // 位移量
9    pos: DVec2,        // 当前位置
10    origin: DVec2,     // 起始位置
11    width: f64,        // 宽度
12    height: f64,       // 高度
13    width_used: f64,   // 已使用宽度
14    height_used: f64,  // 已使用高度
15}
```

Example 2 (rust):
```rust
1// 开始一个 turtle 布局
2cx.begin_turtle(walk, layout);
3
4// 绘制过程示例
5turtle.move_to(10.0, 10.0);  // 移动到指定位置
6turtle.line_to(100.0, 100.0); // 画线到指定位置
7turtle.rect(0.0, 0.0, 50.0, 30.0); // 绘制矩形
8
9// 结束布局
10cx.end_turtle();
```

Example 3 (rust):
```rust
1impl Turtle {
2    // 计算下一个位置
3    pub fn next_pos(&mut self, size: DVec2) -> DVec2 {
4        match self.layout.flow {
5            Flow::Right => {
6                let pos = self.pos;
7                self.pos.x += size.x + self.layout.spacing;
8                self.width_used = self.width_used.max(self.pos.x);
9                pos
10            }
11            Flow::Down => {
12                let pos = self.pos;
13                self.pos.y += size.y + self.layout.spacing;
14                self.height_used = self.height_used.max(self.pos.y);
15                pos
16            }
17            // ... 其他流向的处理
18        }
19    }
20}
```

Example 4 (rust):
```rust
1MyView = <View> {
2    // 外层 turtle
3    walk: {width: Fill, height: Fit},
4    layout: {flow: Down, spacing: 10},
5
6    // 内层 turtle
7    <View> {
8        walk: {width: Fill, height: Fit},
9        layout: {
10            flow: Right,
11            spacing: 5,
12            padding: {left: 10, right: 10}
13        },
14
15        // 内容组件
16        <Button> {}
17        <Button> {}
18    }
19}
```

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/start/makepad-framework

**Contents:**
- #Makepad Framework

目前为止，我们说 Makepad ，主要是指 Makepad Framework ，本书的内容也主要是围绕 Makepad Framework。

它正在积极开发中，但已经可以用于构建快速原型和简单（甚至复杂的 UI）应用程序。

目前 makepad 最活跃的分支是 https://github.com/makepad/makepad/tree/dev

---

## 快速入门

**URL:** https://book.makepad.rs/zh/guide/layout-system/best-practice

**Contents:**
- #最佳实践建议

通过理解和遵循这些原则，我们能够创建更稳定和高效的布局系统，避免陷入循环依赖的陷阱。

在实际开发中，保持布局的简单性和可预测性是关键。

---
