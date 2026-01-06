---
name: makepad-rust
description: Makepad - 用 Rust 编写的开源跨平台 UI 框架，支持 Windows、Linux、macOS、iOS、Android 和 Web。基于 GPU 着色器渲染，提供实时样式化和高性能 UI 开发体验。
---

# Makepad Rust UI 框架

Makepad 是一个用 Rust 编写的开源跨平台 UI 框架，支持所有主流平台（Windows、Linux、macOS、iOS、Android、Web）。它基于 GPU 着色器渲染，提供超快编译速度和实时样式化功能。

## 核心特性

- **跨平台**: 一套代码运行于所有主流平台
- **GPU 加速**: 基于着色器的高性能 UI 渲染
- **实时样式化**: UI 变化即时反映，无需重新编译
- **混合即时模式**: 结合即时模式和保留模式的优势
- **Live DSL**: 声明式 UI 语法，简化样式定义

## 何时使用此技能

- 使用 Makepad 框架开发跨平台应用
- 编写 Live DSL 声明式 UI
- 使用 Sdf2d 进行矢量图形绘制
- 实现 Widget 组件和事件处理
- 创建动画和着色器效果
- 集成 Moly Kit AI 组件

## 快速开始

### 创建新项目

```bash
cargo new my_makepad_app
```

### Cargo.toml 依赖

```toml
[dependencies]
makepad-widgets = { git = "https://github.com/makepad/makepad" }
```

### 基本应用结构

```rust
use makepad_widgets::*;

live_design! {
    use link::theme::*;
    use link::widgets::*;

    App = {{App}} {
        ui: <Root> {
            main_window = <Window> {
                show_bg: true,
                width: Fill,
                height: Fill,
                draw_bg: {
                    fn pixel(self) -> vec4 {
                        return #2a2a2a;
                    }
                }
                body = <View> {
                    flow: Down,
                    spacing: 10,
                    align: { x: 0.5, y: 0.5 }
                    
                    <Label> {
                        text: "Hello Makepad!"
                    }
                    <Button> {
                        text: "Click Me"
                    }
                }
            }
        }
    }
}

#[derive(Live, LiveHook)]
pub struct App {
    #[live] ui: WidgetRef,
    #[rust] counter: i32,
}

impl LiveRegister for App {
    fn live_register(cx: &mut Cx) {
        makepad_widgets::live_design(cx);
    }
}

impl MatchEvent for App {
    fn handle_actions(&mut self, cx: &mut Cx, actions: &Actions) {
        if self.ui.button(id!(button)).clicked(&actions) {
            self.counter += 1;
            // 更新 UI
        }
    }
}

impl AppMain for App {
    fn handle_event(&mut self, cx: &mut Cx, event: &Event) {
        self.match_event(cx, event);
        self.ui.handle_event(cx, event, &mut Scope::empty());
    }
}

app_main!(App);
```

## Live DSL 语法

### 布局属性

```rust
live_design! {
    MyView = <View> {
        // 尺寸
        width: Fill,        // 填充父容器
        height: Fit,        // 适应内容
        width: 200,         // 固定像素
        
        // 布局方向
        flow: Down,         // 垂直排列
        flow: Right,        // 水平排列
        flow: Overlay,      // 叠加
        
        // 间距和内边距
        spacing: 10,
        padding: { left: 10, top: 5, right: 10, bottom: 5 }
        
        // 对齐 (0.0-1.0 归一化坐标)
        align: { x: 0.5, y: 0.5 }  // 居中
        align: { x: 1.0, y: 1.0 }  // 右下角
    }
}
```

### 背景绘制

```rust
live_design! {
    MyView = <View> {
        show_bg: true,
        draw_bg: {
            color: #ff0000,  // 纯色背景
            
            // 或自定义着色器
            fn pixel(self) -> vec4 {
                // 渐变效果
                return mix(#f00, #00f, self.pos.x);
            }
        }
    }
}
```

### 动画系统

```rust
live_design! {
    MyButton = <Button> {
        animator: {
            hover = {
                default: off,
                off = {
                    from: { all: Forward { duration: 0.2 } }
                    apply: { draw_bg: { color: #333 } }
                }
                on = {
                    from: { all: Forward { duration: 0.2 } }
                    apply: { draw_bg: { color: #555 } }
                }
            }
            pressed = {
                default: off,
                off = {
                    from: { all: Forward { duration: 0.1 } }
                    apply: { draw_bg: { color: #333 } }
                }
                on = {
                    from: { all: Snap } }
                    apply: { draw_bg: { color: #222 } }
                }
            }
        }
    }
}
```

## Sdf2d 矢量绘图

Sdf2d 是 Makepad 的 2D 矢量绘图 API，基于 Signed Distance Field 技术。

### 基本形状

```rust
fn pixel(self) -> vec4 {
    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
    
    // 圆形
    sdf.circle(50.0, 50.0, 30.0);
    sdf.fill(#f00);
    
    // 矩形
    sdf.rect(10.0, 10.0, 80.0, 60.0);
    sdf.fill(#0f0);
    
    // 圆角矩形
    sdf.box(10.0, 10.0, 80.0, 60.0, 5.0);
    sdf.fill(#00f);
    
    // 六边形
    sdf.hexagon(50.0, 50.0, 30.0);
    sdf.fill(#ff0);
    
    return sdf.result;
}
```

### 路径绘制

```rust
fn pixel(self) -> vec4 {
    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
    
    // 绘制星形
    sdf.move_to(30.0, 30.0);
    sdf.line_to(50.0, 70.0);
    sdf.line_to(70.0, 30.0);
    sdf.line_to(10.0, 50.0);
    sdf.line_to(90.0, 50.0);
    sdf.close_path();
    
    sdf.fill(#f00);
    return sdf.result;
}
```

### 布尔运算

```rust
fn pixel(self) -> vec4 {
    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
    
    // 并集
    sdf.circle(40.0, 50.0, 30.0);
    sdf.union();
    sdf.circle(60.0, 50.0, 30.0);
    sdf.fill(#f00);
    
    // 交集
    sdf.circle(40.0, 50.0, 30.0);
    sdf.intersect();
    sdf.circle(60.0, 50.0, 30.0);
    sdf.fill(#0f0);
    
    // 差集
    sdf.circle(50.0, 50.0, 40.0);
    sdf.subtract();
    sdf.circle(50.0, 50.0, 20.0);
    sdf.fill(#00f);
    
    return sdf.result;
}
```

### 描边和发光

```rust
fn pixel(self) -> vec4 {
    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
    
    sdf.circle(50.0, 50.0, 30.0);
    sdf.stroke(#fff, 2.0);  // 描边
    
    sdf.circle(50.0, 50.0, 30.0);
    sdf.glow(#0ff, 10.0);   // 发光效果
    
    return sdf.result;
}
```

### 变换

```rust
fn pixel(self) -> vec4 {
    let sdf = Sdf2d::viewport(self.pos * self.rect_size);
    
    sdf.translate(10.0, 10.0);           // 平移
    sdf.rotate(PI * 0.25, 50.0, 50.0);   // 旋转 (弧度, 中心点)
    sdf.scale(1.5, 50.0, 50.0);          // 缩放
    
    sdf.rect(20.0, 20.0, 60.0, 60.0);
    sdf.fill(#f00);
    
    return sdf.result;
}
```

## Widget 开发

### 自定义 Widget 结构

```rust
#[derive(Live, LiveHook, Widget)]
pub struct MyWidget {
    #[live] draw_bg: DrawQuad,
    #[live] draw_text: DrawText,
    #[layout] layout: Layout,
    #[walk] walk: Walk,
    #[rust] value: f32,
}

impl Widget for MyWidget {
    fn handle_event(&mut self, cx: &mut Cx, event: &Event, scope: &mut Scope) {
        match event.hits(cx, self.draw_bg.area()) {
            Hit::FingerDown(_) => {
                cx.widget_action(uid, &scope.path, MyWidgetAction::Clicked);
            }
            _ => {}
        }
    }
    
    fn draw_walk(&mut self, cx: &mut Cx2d, scope: &mut Scope, walk: Walk) -> DrawStep {
        self.draw_bg.begin(cx, walk, self.layout);
        self.draw_text.draw_walk(cx, walk, Align::default(), "Hello");
        self.draw_bg.end(cx);
        DrawStep::done()
    }
}
```

### Action 定义

```rust
#[derive(Clone, Debug, DefaultNone)]
pub enum MyWidgetAction {
    None,
    Clicked,
    ValueChanged(f32),
}
```

## Moly Kit 集成

Moly Kit 是用于 AI 应用开发的组件库。

### 安装

```toml
[dependencies]
moly-kit = { git = "https://github.com/moly-ai/moly-ai.git", features = ["full"], branch = "main" }
```

### 注册组件

```rust
impl LiveRegister for App {
    fn live_register(cx: &mut Cx) {
        makepad_widgets::live_design(cx);
        moly_kit::widgets::live_design(cx);
    }
}
```

### 使用 Chat 组件

```rust
live_design! {
    use moly_kit::widgets::chat::Chat;
    
    MyApp = {{MyApp}} {
        chat = <Chat> {}
    }
}
```

## 常用内置函数 (Shader)

### 数学函数
- `abs(x)` - 绝对值
- `sin(x)`, `cos(x)`, `tan(x)` - 三角函数
- `pow(x, y)` - 幂运算
- `sqrt(x)` - 平方根
- `min(x, y)`, `max(x, y)` - 最小/最大值
- `clamp(x, min, max)` - 限制范围
- `mix(a, b, t)` - 线性插值
- `step(edge, x)` - 阶跃函数
- `smoothstep(e0, e1, x)` - 平滑阶跃

### 向量函数
- `length(v)` - 向量长度
- `distance(a, b)` - 两点距离
- `normalize(v)` - 归一化
- `dot(a, b)` - 点积
- `cross(a, b)` - 叉积

## 参考文件

详细文档位于 `references/` 目录：

- `getting_started.md` - 入门指南、Widget 基础、应用结构
- `guide.md` - Moly Kit 组件使用指南
- `api.md` - Sdf2d API 完整参考
- `contribute.md` - 贡献指南

## 资源链接

- 官网: https://makepad.nl/
- GitHub: https://github.com/makepad/makepad
- 在线体验: https://makepad.dev/
- Makepad Book: https://book.makepad.rs/zh/
