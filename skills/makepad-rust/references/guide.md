# Makepad-Rust - Guide

**Pages:** 12

---

## 开发指南

**URL:** https://book.makepad.rs/zh/moly-kit/index

**Contents:**
- #Moly Kit 介绍

Moly Kit 是一个 Rust 程序库，包含用于简化 Makepad 框架人工智能应用程序开发的组件和实用工具。

它拥有非常出色的新特性，为您在开发 Makepad 应用程序时提供了许多必要的参考和组件的设计思路，同时加速您的开发与使用体验。

官方仓库开源地址：https://github.com/moly-ai/moly-ai/tree/main/moly-kit

---

## 开发指南

**URL:** https://book.makepad.rs/zh/moly-kit/widgets/widgets-content

**Contents:**
- #消息内容与呈现组件
- #MessageMarkdown (widgets/message_markdown.rs)
- #StandardMessageContent (widgets/standard_message_content.rs)
- #MessageLoading (widgets/message_loading.rs)
- #MessageThinkingBlock (widgets/message_thinking_block.rs)
- #ChatLine (widgets/chat_line.rs)

**Examples:**

Example 1 (rust):
```rust
1live_design! {
2    import crate::widgets::message_markdown::*;
3    Preview = <MessageMarkdown> { text: "Hello **Markdown**" }
4}
```

Example 2 (rust):
```rust
1let content = MessageContent {
2    text: "Hi there".into(),
3    attachments: vec![],
4    ..Default::default()
5};
6item.slot(ids!(content))
7    .current()
8    .as_standard_message_content()
9    .set_content(cx, &content);
```

Example 3 (rust):
```rust
1let loading = list.item(cx, index, live_id!(LoadingLine));
2loading.message_loading(ids!(content_section.loading)).animate(cx);
```

Example 4 (rust):
```rust
1thinking_block.set_content(cx, &MessageContent {
2    text: "Reasoning...".into(),
3    ..Default::default()
4});
5thinking_block.update_animation(cx);
```

---

## 开发指南

**URL:** https://book.makepad.rs/zh/moly-kit/widgets/widgets-core

**Contents:**
- #核心对话组件
- #Chat (widgets/chat.rs)
- #Messages (widgets/messages.rs)
- #PromptInput (widgets/prompt_input.rs)

先决条件：入口调用 moly_kit::widgets::live_design(cx); 以加载主题与 Live DSL。Chat 需启用 async-rt 特性，实时语音需 realtime 特性（非 wasm）。

**Examples:**

Example 1 (rust):
```rust
1// 入口初始化
2moly_kit::widgets::live_design(cx);
3
4// 创建并绑定 ChatController
5let controller = Arc::new(Mutex::new(ChatController::new(/* ... */)));
6let mut chat = view.chat(ids!(chat));
7chat.write().set_chat_controller(cx, Some(controller.clone()));
```

Example 2 (rust):
```rust
1if let Event::Actions(actions) = event {
2    for action in actions {
3        if let Some(msg_action) = action.as_widget_action().and_then(|wa| wa.cast::<MessagesAction>()) {
4            match msg_action {
5                MessagesAction::Copy(idx) => { /* copy to clipboard */ }
6                MessagesAction::Delete(idx) => controller.lock().unwrap()
7                    .dispatch_mutation(VecMutation::<Message>::RemoveOne(idx)),
8                MessagesAction::EditRegenerate(_) => { /* 自行实现 */ }
9                _ => {}
10            }
11        }
12    }
13}
```

Example 3 (rust):
```rust
1let mut input = view.prompt_input(ids!(prompt));
2if input.read().submitted(event.actions()) && input.read().has_send_task() {
3    let text = input.text();
4    controller.lock().unwrap().dispatch_mutation(VecMutation::Push(Message {
5        from: EntityId::User,
6        content: MessageContent { text, ..Default::default() },
7        ..Default::default()
8    }));
9    input.write().reset(cx);
10    controller.lock().unwrap().dispatch_task(ChatTask::Send);
11}
```

---

## #贡献指南

**URL:** https://book.makepad.rs/zh/contribute/index

**Contents:**
- #贡献指南
- #提交pull request
- #PR 标题格式

我们非常感谢您为Makepad之书做出贡献！每一项的贡献都是对我们的支持，使我们能够不断完善Makepad之书，让更多喜欢Makepad的人受益。

提交 Pull Request 时，请注意以下几点：

PR 标题的格式遵循 Conventional Commits 规范。

**Examples:**

Example 1 (unknown):
```unknown
1update(contribute): Add `fooBar` config
2^           ^     ^
3|           |     |__ Subject
4|           |_______ Scope (可选)
5|____________ Type
```

---

## 开发指南

**URL:** https://book.makepad.rs/zh/moly-kit/widgets/widgets-references

**Contents:**
- #引用与链接
- #Citation (widgets/citation.rs)
- #CitationList (widgets/citation_list.rs)

**Examples:**

Example 1 (rust):
```rust
1let mut citation = view.citation(ids!(c1));
2citation.borrow_mut().unwrap().set_url_once(cx, "https://example.com".into());
3citation.label(ids!(title)).set_text(cx, "Example");
```

Example 2 (rust):
```rust
1item.citation_list(ids!(citations)).with_items(|list| {
2    // 假定内部提供接口设置条目，或通过 Live DSL 预定义
3});
```

---

## 开发指南

**URL:** https://book.makepad.rs/zh/moly-kit/widgets/widgets-layout

**Contents:**
- #布局/模态/主题
- #MolyModal (widgets/moly_modal.rs)
- #Slot (widgets/slot.rs)
- #Theme (widgets/theme_moly_kit_light.rs)

**Examples:**

Example 1 (rust):
```rust
1let modal = view.moly_modal(ids!(settings_modal));
2modal.open_as_dialog(cx);
3if modal.dismissed(event.actions()) { /* 关闭后逻辑 */ }
```

Example 2 (rust):
```rust
1let mut slot = item.slot(ids!(content));
2slot.replace(custom_widget);
3// 恢复默认
4slot.restore();
```

---

## 开发指南

**URL:** https://book.makepad.rs/zh/moly-kit/start/index

**Contents:**
- #快速上手
- #先决条件
- #安装
- #注册 Moly Kit Widgets
- #在 DSL 中使用

本指南假设您熟悉 Makepad，并且您已经准备好一个基本的应用程序，以便按照本指南开始集成 Moly Kit。

将 Moly Kit 添加到您的Cargo.toml依赖项中：

如果想使用稳定版本，请更改branch = "main"为tag = "<具体版本>"

因为目前 Moly Kit 的贡献比较频繁，你在切换不同版本时，可能会遇到兼容性问题。

建议 makepad widgets 和 moly kit 的版本如下, 使用目前 moly-kit 相同的版本

live_register 与任何 Makepad 应用一样，我们需要在应用中使用任何使用 Moly Kit 的小部件之前，先注册我们想要使用的小部件。

**Examples:**

Example 1 (unknown):
```unknown
1moly-kit = { git = "https://github.com/moly-ai/moly-ai.git", features = ["full"], branch = "main" }
```

Example 2 (unknown):
```unknown
1makepad-widgets = { git = "https://github.com/wyeworks/makepad", rev = "b8b65f4fa" }
2moly-kit = { git = "https://github.com/moxin-org/moly.git", features = ["full"], branch = "main" }
```

Example 3 (rust):
```rust
1impl LiveRegister for App {
2    fn live_register(cx: &mut Cx) {
3        // 注册Makepad 自带的 widgets
4        makepad_widgets::live_design(cx);
5
6        // !!! 注册Moly Kit的 widgets
7        moly_kit::widgets::live_design(cx);
8
9        // 注册你自定义的 widgets
10        crate::your_amazing_widgets::live_design(cx);
11    }
12}
```

Example 4 (rust):
```rust
1live_design! {
2    use link::theme::*;
3    use link::widgets::*;
4
5    // Add this line
6    use moly_kit::widgets::chat::Chat;
7
8    pub YourAmazingWidget = {{YourAmazingWidget}} {
9        // And this line
10        chat = <Chat> {}
11    }
12}
```

---

## 开发指南

**URL:** https://book.makepad.rs/zh/moly-kit/widgets/widgets-attachments

**Contents:**
- #附件与媒体组件
- #AttachmentList / DenseAttachmentList (widgets/attachment_list.rs)
- #AttachmentView (widgets/attachment_view.rs)
- #AttachmentViewerModal (widgets/attachment_viewer_modal.rs)
- #ImageView (widgets/image_view.rs)

小贴士：用 AttachmentView::can_preview 判断是否可预览，可预览则配合 AttachmentViewerModal 打开。

**Examples:**

Example 1 (rust):
```rust
1let mut list = view.attachment_list(ids!(attachments));
2list.write().attachments.push(Attachment::from_text("note.txt", "hi").unwrap());
```

Example 2 (rust):
```rust
1attachment_view.set_attachment(cx, attachment.clone());
2if AttachmentView::can_preview(&attachment) {
3    // 显示“预览”按钮
4}
```

Example 3 (rust):
```rust
1if AttachmentView::can_preview(&attachment) {
2    viewer_modal.open(cx, attachment);
3}
```

Example 4 (rust):
```rust
1if ImageView::can_load("image/png") {
2    image_view.load_png(cx, &png_bytes)?;
3}
```

---

## 开发指南

**URL:** https://book.makepad.rs/zh/moly-kit/widgets/widgets-realtime

**Contents:**
- #实时语音组件
- #Realtime (widgets/realtime.rs)

**Examples:**

Example 1 (rust):
```rust
1match upgrade {
2    Upgrade::Realtime(channel) => {
3        let mut realtime = chat.realtime(ids!(realtime));
4        realtime.set_bot_entity_id(cx, EntityId::Bot(bot_id.clone()));
5        realtime.set_realtime_channel(channel.clone());
6        chat.moly_modal(ids!(audio_modal)).open_as_dialog(cx);
7        None
8    }
9    _ => Some(upgrade),
10}
```

---

## 开发指南

**URL:** https://book.makepad.rs/zh/moly-kit/widgets/widgets-identity

**Contents:**
- #身份与头像
- #Avatar (widgets/avatar.rs)

**Examples:**

Example 1 (rust):
```rust
1let mut avatar = widget.avatar(ids!(avatar)).borrow_mut().unwrap();
2avatar.avatar = Some(EntityAvatar::Text("B".into()));
```

Example 2 (rust):
```rust
1avatar.avatar = Some(EntityAvatar::Image("res/avatar.png".into()));
```

---

## Sdf2d

**URL:** https://book.makepad.rs/zh/api/Makepad-Quick-Reference-Guide

**Contents:**
- #Makepad 速查手册
- #Live DSL 速查手册
  - #核心概念
  - #基础布局与尺寸 (Walk & Layout)
  - #基础绘制 (Draw* 类型)
  - #SDF 绘制 (Sdf2d in fn pixel)
  - #动画 (animator)
    - #基本结构
    - #状态定义 (state_name = { ... })
    - #状态转换参数

Live DSL 中的对齐坐标 (align)

作用域: layout 属性块内，用于控制父容器如何对齐其子元素。

类型: Align { x: f64, y: f64 }

坐标系: 归一化相对坐标 (Normalized Relative Coordinates)，范围通常是 0.0 到 1.0。

在 fn pixel 中使用 Sdf2d 进行矢量绘制。

SDF 绘制 (Sdf2d in fn pixel) - 基本形状

在 fn pixel 函数中，你可以使用 Sdf2d 对象来定义和组合形状。以下是常用的基本形状函数及其说明：

sdf.circle(cx: float, cy: float, radius: float)

sdf.rect(x: float, y: float, w: float, h: float)

sdf.box(x: float, y: float, w: float, h: float, radius: float)

sdf.box_x(x: float, y: float, w: float, h: float, r_left: float, r_right: float)

sdf.box_y(x: float, y: float, w: float, h: float, r_top: float, r_bottom: float)

sdf.box_all(x: float, y: float, w: float, h: float, r_left_top: float, r_right_top: float, r_right_bottom: float, r_left_bottom: float)

sdf.hexagon(cx: float, cy: float, radius: float)

sdf.hline(y: float, half_thickness: float)

sdf.move_to(x: float, y: float)

sdf.line_to(x: float, y: float)

sdf.arc2(cx: float, cy: float, radius: float, start_angle: float, end_angle: float)

这些函数仅仅是定义形状的边界（通过计算 Signed Distance Field）。

要实际看到这些形状，你必须在定义形状之后调用一个绘制函数，如 sdf.fill(color), sdf.stroke(color, width), sdf.glow(color, width) 等。

sdf.shape 存储了当前已定义形状的最小 SDF 值（用于 union），而 sdf.old_shape 用于布尔运算（intersect, subtract, gloop）。

sdf.dist 存储了最新定义的那个基本形状的 SDF 值。

清除: sdf.clear(color): 用指定颜色覆盖当前结果。

结果: sdf.result (最终的 vec4 颜色)。

Shader 中的坐标 (self.pos, self.geom_pos, Sdf2d::viewport 参数)

作用域: draw_* 块内的 fn pixel 和 fn vertex 函数。

类型: vec2 (通常是 f32 类型)

坐标系: 归一化局部坐标 (Normalized Local Coordinates)，范围通常是 0.0 到 1.0。

理解这两种坐标系的区别对于精确控制 Makepad UI 的布局和视觉效果至关重要。align 用于宏观布局，而 Shader 中的 self.pos 用于微观的像素级绘制。

animator 块用于定义 Widget 的状态以及这些状态之间的过渡动画。它允许你以声明式的方式创建交互式的视觉反馈。

在每个 track_name 内部，通常定义至少两个状态，最常见的是 on 和 off，也可以是自定义的状态名。

在每个具体的状态定义块（如 on = { ... }）内部，可以设置以下参数来控制动画如何进入该状态：

示例：按钮的 Hover 和 Down 状态

在 Makepad 中，实现平滑的视觉动画（例如 UI 元素的位置、大小、颜色、透明度等的渐变）强烈推荐使用 NextFrame 事件机制，而不是 Timer。

Animator 系统内部就是基于 NextFrame 来驱动动画更新和重绘的。

动画过程 (Animation Process): 指的是一个属性值从 A 平滑地过渡到 B 的整个过程。例如：

对于这种平滑的、逐帧变化的视觉过渡，你应该依赖 Animator 和 NextFrame。Animator 会在每一帧（由 NextFrame 触发）计算出属性的中间值，并更新它，然后请求下一帧重绘，直到动画完成。你不应该使用 Timer 来手动控制这种逐帧更新，因为 Timer 的触发间隔与屏幕刷新率无关，会导致动画卡顿。

延时触发 (Delayed Trigger): 指的是在一段时间之后执行一个一次性的操作。例如：

对于这种在特定时间点触发某个动作的需求，你应该使用 Timer。cx.start_timeout(duration) 就是为此设计的。当定时器触发时 (my_timer.is_event(event).is_some())，你执行相应的操作，比如调用 self.close(cx) 来启动关闭动画。

比如：robrix 项目中 RobrixPopupNotification 组件弹框动画:

示例 (自定义 Shader & 优化):

<View> 是唯一可以设置 event_order 属性的内置 Widget。

如果你需要对特定组件（非 <View>）的子元素事件顺序进行精细控制，你可能需要：

示例 (自定义背景和动画): (参考文档中的 Advanced 示例)

示例 (自定义背景和光标): (参考文档中的 Advanced 示例)

示例 (自定义 Rotary 外观): (参考文档中的 RotarySolid 示例)

示例 (ImageBlend 切换): (参考文档中的 ImageBlend 示例和 App 代码)

最佳实践通常是在 DSL 中尽可能多地进行声明式定义，并将复杂的逻辑和状态管理保留在 Rust 代码中。

重点关注在 live_design! 的 draw_* 块中 fn pixel 和 fn vertex 函数内可用的特性和内置函数。

Sdf2d (Signed Distance Field 2D): 用于矢量绘图。

Pal (Palette): 颜色处理函数。

GaussShadow: 高斯模糊阴影计算。

(基于文档中 builtin 目录下的文件)

想象一个父容器，比如一个 <View>，它有一定的宽度和高度。当你在里面放置一个子元素时，如果父容器的空间比子元素大，子元素就需要知道放在这个“剩余空间”的哪个位置。

“归一化相对坐标”就是一种描述这个位置的方式，它不使用具体的像素值，而是使用比例。

具体到 align: {x: f64, y: f64}:

可用空间 (Available Space):

这是理解相对坐标的关键。可用空间是指父容器在放置完所有非对齐子元素（例如，按 flow: Right 或 flow: Down 排列的元素）以及考虑了自身的 padding 之后，剩余的用于放置对齐子元素（通常是那些 width: Fit, height: Fit 或 width: Fixed, height: Fixed 的元素）的空间。

总之，归一化相对坐标提供了一种与分辨率无关、基于比例的方式来定义子元素在父容器可用空间内的对齐位置。

**Examples:**

Example 1 (css):
```css
1<View> { // 父容器
2    width: 200, height: 100,
3    show_bg: true, draw_bg: { color: #eee }
4    padding: 10,
5    // 子元素将在父容器的可用空间内右下角对齐
6    align: { x: 1.0, y: 1.0 }
7
8    <Button> { // 子元素
9        text: "OK",
10        width: Fit, height: Fit // 尺寸由内容决定
11    }
12}
```

Example 2 (rust):
```rust
1<View> {
2    show_bg: true,
3    draw_bg: {
4        fn pixel(self) -> vec4 {
5            // self.pos.x 从左到右从 0.0 渐变到 1.0
6            // self.pos.y 从上到下从 0.0 渐变到 1.0
7            // 创建一个从左(红)到右(绿)的水平渐变
8            return mix(#f00, #0f0, self.pos.x);
9        }
10    }
11}
```

Example 3 (rust):
```rust
1<View> {
2    show_bg: true,
3    draw_bg: {
4        fn pixel(self) -> vec4 {
5            let sdf = Sdf2d::viewport(self.pos * self.rect_size);
6            // 在局部像素坐标 (10, 10) 处绘制一个半径为 5 的圆
7            sdf.circle(10.0, 10.0, 5.0);
8            sdf.fill(#fff);
9            return sdf.result;
10        }
11    }
12}
```

Example 4 (json):
```json
1MyWidget = {{MyWidget}} {
2    // ... 其他属性 ...
3    animator: {
4        // 定义一个或多个状态动画轨迹
5        track_name1 = { ... }
6        track_name2 = { ... }
7        // ...
8    }
9}
```

---

## 开发指南

**URL:** https://book.makepad.rs/zh/moly-kit/widgets/widgets-models

**Contents:**
- #模型选择组件
- #ModelSelector (widgets/model_selector.rs)
- #ModelSelectorList (widgets/model_selector_list.rs)
- #ModelSelectorItem (widgets/model_selector_item.rs)

典型：PromptInput 已包含 ModelSelector。若独立使用，创建后设 controller；如需定制分组/搜索，直接操作 ModelSelectorList。

**Examples:**

Example 1 (rust):
```rust
1let controller = Arc::new(Mutex::new(ChatController::new(...)));
2selector.borrow_mut().unwrap().set_chat_controller(Some(controller.clone()));
```

Example 2 (rust):
```rust
1list.borrow_mut().unwrap().set_search_filter(cx, "gpt");
2list.borrow_mut().unwrap().set_selected_bot_id(Some(bot_id.clone()));
```

Example 3 (rust):
```rust
1item.borrow_mut().unwrap().set_bot(bot.clone());
2item.borrow_mut().unwrap().set_selected_bot_id(Some(bot.id.clone()));
```

---
