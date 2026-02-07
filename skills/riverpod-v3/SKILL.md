---
name: riverpod-v3
description: Riverpod v3 - Flutter 响应式状态管理框架。包含 Provider、Consumer、自动销毁、Family、Mutations、离线持久化、代码生成等核心概念和最佳实践。
---

# Riverpod v3 状态管理框架

Riverpod 是 Flutter/Dart 的响应式缓存和数据绑定框架，v3 版本带来了重大改进，包括简化的 API、实验性的 Mutations 和离线持久化支持。

## 核心特性

- **响应式状态管理**: 自动追踪依赖，状态变化时自动重建
- **类型安全**: 编译时检查，无运行时错误
- **可测试性**: 轻松 mock 和覆盖 providers
- **代码生成**: 可选的 riverpod_generator 简化代码
- **自动销毁**: 智能管理资源生命周期
- **Family**: 参数化 providers
- **Mutations (实验性)**: 简化异步操作
- **离线持久化 (实验性)**: 本地缓存支持

## 何时使用此技能

- 使用 Riverpod v3 进行 Flutter 状态管理
- 创建和使用 Provider、Notifier
- 处理异步数据 (FutureProvider, AsyncNotifier)
- 实现依赖注入和测试
- 从 v2 迁移到 v3
- 使用代码生成简化开发

## 快速开始

### 安装

```bash
# Flutter 项目
flutter pub add flutter_riverpod

# 使用代码生成 (推荐)
flutter pub add flutter_riverpod
flutter pub add riverpod_annotation
flutter pub add dev:riverpod_generator
flutter pub add dev:build_runner

# 运行代码生成
dart run build_runner watch -d
```

### pubspec.yaml

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_riverpod: ^3.1.0
  riverpod_annotation: ^4.0.0

dev_dependencies:
  build_runner:
  riverpod_generator: ^4.0.0+1
```

### Hello World 示例

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

// 创建一个 Provider
final helloWorldProvider = Provider((_) => 'Hello world');

void main() {
  runApp(
    // 用 ProviderScope 包裹应用
    ProviderScope(
      child: MyApp(),
    ),
  );
}

// 使用 ConsumerWidget 代替 StatelessWidget
class MyApp extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // 使用 ref.watch 监听 provider
    final String value = ref.watch(helloWorldProvider);

    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: const Text('Example')),
        body: Center(child: Text(value)),
      ),
    );
  }
}
```

## Provider 类型

### 基础 Provider

```dart
// 简单值
final nameProvider = Provider((ref) => 'John');

// 使用代码生成
@riverpod
String name(Ref ref) => 'John';
```

### FutureProvider (异步数据)

```dart
final userProvider = FutureProvider<User>((ref) async {
  final response = await http.get(Uri.parse('api/user'));
  return User.fromJson(jsonDecode(response.body));
});

// 代码生成
@riverpod
Future<User> user(Ref ref) async {
  final response = await http.get(Uri.parse('api/user'));
  return User.fromJson(jsonDecode(response.body));
}
```

### StreamProvider

```dart
final messagesProvider = StreamProvider<List<Message>>((ref) {
  return firestore.collection('messages').snapshots();
});
```

### Notifier (可变状态)

```dart
// 手动定义
class CounterNotifier extends Notifier<int> {
  @override
  int build() => 0;

  void increment() => state++;
  void decrement() => state--;
}

final counterProvider = NotifierProvider<CounterNotifier, int>(
  CounterNotifier.new,
);

// 代码生成
@riverpod
class Counter extends _$Counter {
  @override
  int build() => 0;

  void increment() => state++;
  void decrement() => state--;
}
```

### AsyncNotifier (异步可变状态)

```dart
@riverpod
class TodoList extends _$TodoList {
  @override
  Future<List<Todo>> build() async {
    return fetchTodos();
  }

  Future<void> addTodo(Todo todo) async {
    state = const AsyncLoading();
    final newTodos = await saveTodo(todo);
    state = AsyncData(newTodos);
  }
}
```

## 在 Widget 中使用

### ConsumerWidget

```dart
class MyWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.watch(counterProvider);
    return Text('Count: $count');
  }
}
```

### Consumer Builder

```dart
Consumer(
  builder: (context, ref, child) {
    final count = ref.watch(counterProvider);
    return Text('Count: $count');
  },
)
```

### 处理异步状态

```dart
class UserWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final userAsync = ref.watch(userProvider);

    return switch (userAsync) {
      AsyncData(:final value) => Text('Hello ${value.name}'),
      AsyncError(:final error) => Text('Error: $error'),
      _ => const CircularProgressIndicator(),
    };
  }
}
```

## Ref 方法

### ref.watch - 监听并重建

```dart
// 在 build 方法中使用，状态变化时自动重建
final value = ref.watch(myProvider);
```

### ref.read - 一次性读取

```dart
// 在回调中使用，不会触发重建
onPressed: () {
  final value = ref.read(myProvider);
  ref.read(counterProvider.notifier).increment();
}
```

### ref.listen - 监听副作用

```dart
ref.listen(myProvider, (previous, next) {
  // 状态变化时执行副作用
  ScaffoldMessenger.of(context).showSnackBar(
    SnackBar(content: Text('Value changed to $next')),
  );
});
```

### ref.invalidate - 重置状态

```dart
// 重置 provider，下次读取时重新计算
ref.invalidate(myProvider);

// 重置并立即获取新值
final newValue = ref.refresh(myProvider);
```

## 自动销毁 (Auto Dispose)

```dart
// 代码生成默认启用自动销毁
@riverpod
String hello(Ref ref) => 'Hello';

// 禁用自动销毁
@Riverpod(keepAlive: true)
String hello(Ref ref) => 'Hello';

// 手动定义
final provider = Provider.autoDispose((ref) => 'Hello');

// 条件保持存活
@riverpod
Future<String> example(Ref ref) async {
  final response = await http.get(Uri.parse('api/data'));
  // 成功后保持存活
  ref.keepAlive();
  return response.body;
}
```

### 生命周期回调

```dart
@riverpod
Stream<int> example(Ref ref) {
  final controller = StreamController<int>();

  // 销毁时清理资源
  ref.onDispose(controller.close);

  // 最后一个监听者移除时
  ref.onCancel(() => print('No listeners'));

  // 新监听者添加时
  ref.onResume(() => print('Listener added'));

  return controller.stream;
}
```

## Family (参数化 Provider)

```dart
// 代码生成 - 直接添加参数
@riverpod
Future<User> user(Ref ref, int userId) async {
  return fetchUser(userId);
}

// 使用
final user = ref.watch(userProvider(123));

// 手动定义
final userProvider = FutureProvider.family<User, int>((ref, userId) async {
  return fetchUser(userId);
});
```

## 测试

### 单元测试

```dart
void main() {
  test('counter increments', () {
    final container = ProviderContainer.test();

    expect(container.read(counterProvider), 0);

    container.read(counterProvider.notifier).increment();

    expect(container.read(counterProvider), 1);
  });
}
```

### Mock Provider

```dart
final container = ProviderContainer.test(
  overrides: [
    userProvider.overrideWith((ref) async => User(name: 'Test')),
  ],
);
```

### Widget 测试

```dart
testWidgets('displays user name', (tester) async {
  await tester.pumpWidget(
    ProviderScope(
      overrides: [
        userProvider.overrideWith((ref) async => User(name: 'John')),
      ],
      child: const MyApp(),
    ),
  );

  await tester.pumpAndSettle();
  expect(find.text('Hello John'), findsOneWidget);
});
```

## 最佳实践

### DO ✅

```dart
// 使用 ref.watch 在 build 中监听
final value = ref.watch(provider);

// 使用 ref.read 在回调中读取
onPressed: () => ref.read(provider.notifier).doSomething()

// 使用 select 优化重建
final name = ref.watch(userProvider.select((u) => u.name));
```

### DON'T ❌

```dart
// 不要在 build 中使用 ref.read
Widget build(context, ref) {
  final value = ref.read(provider); // ❌ 不会响应变化
}

// 不要在 initState 中使用 ref
void initState() {
  ref.read(provider); // ❌ 可能导致问题
}
```

## 从 v2 迁移到 v3

主要变化：
- `Ref` 类型统一，不再区分 `AutoDisposeRef`
- 代码生成默认启用 `autoDispose`
- 新增实验性 Mutations 和离线持久化

```dart
// v2
final provider = Provider.autoDispose<String>((ref) => 'Hello');

// v3 (代码生成)
@riverpod
String provider(Ref ref) => 'Hello'; // 默认 autoDispose
```

## 参考文件

详细文档位于 `references/` 目录：

- `getting_started.md` - 安装和入门
- `concepts.md` - Provider、Consumer、Ref 等核心概念
- `advanced.md` - Mutations、离线持久化、自动重试
- `migration.md` - 版本迁移指南
- `best_practices.md` - DO/DON'T 最佳实践
- `codegen.md` - 代码生成使用指南
- `hooks.md` - Flutter Hooks 集成

## 资源链接

- 官网: https://riverpod.dev/
- GitHub: https://github.com/rrousselGit/riverpod
- pub.dev: https://pub.dev/packages/flutter_riverpod
- riverpod_lint: https://pub.dev/packages/riverpod_lint
