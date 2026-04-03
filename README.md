# Flutter 动画（AnimationController + RotationTransition）

## 简介

`StatefulWidget` 混入 **`SingleTickerProviderStateMixin`**，创建 `AnimationController` 周期 `repeat`，驱动 **`RotationTransition`** 持续旋转一颗大号图标。演示「控制器驱动隐式动画 Widget」的最小闭环。

## 快速开始

### 环境要求

Flutter SDK。

### 运行

```bash
flutter pub get
flutter run
```

## 概念讲解

### 第一部分：Ticker 与 vsync

`AnimationController(vsync: this, duration: Duration(seconds: 2))` 需要 `TickerProvider`；单控制器用 `SingleTickerProviderStateMixin`。`vsync` 把动画帧与屏幕刷新对齐，后台时自动暂停（默认行为随生命周期略有差异，生产需再细调）。

### 第二部分：`RotationTransition` 吃 `Animation<double>`

`turns` 可直接接 `AnimationController`（其本身是 `Animation<double>`一种驱动）。其它如 `FadeTransition`、`ScaleTransition` 模式相同。

```dart
_controller = AnimationController(vsync: this, duration: Duration(seconds: 2))..repeat();
// ...
RotationTransition(
  turns: _controller,
  child: Icon(Icons.refresh, size: 100),
)
```

## 完整示例

见 `lib/main.dart`：`initState` 创建控制器，`build` 里挂 `RotationTransition`。正式项目别忘了在 `dispose` 里 `dispose()` 控制器（本 Demo 为极简可再补）。

## 注意事项

- 多控制器改用 `TickerProviderStateMixin`。
- 复杂曲线用 `CurvedAnimation` 或 `Tween` 包装。

## 完整讲解（中文）

Flutter 动画可以拆成三问：**谁在算时间？**（Controller）**时间怎么映到属性？**（Tween / Curve）**谁负责画？**（Transition 或手动 `AnimatedBuilder`）。本 Demo 只用前两者的最简形态，第三层交给 `RotationTransition`。搞懂这条链以后，再看 `Hero`、`implicitly animated` 组件，都只是换了「第三层」。
