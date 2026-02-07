# Riverpod-V3 - Codegen

**Pages:** 1

---

## About code generation

**URL:** https://riverpod.dev/docs/concepts/about_code_generation

**Contents:**
- About code generation
- Should I use code generation?​
- What are the benefits of using code generation?​
- The Syntax​
  - Defining a provider:​
  - Enabling/disable autoDispose:​
  - Passing parameters to a provider (family):​
- Migrate from non-code-generation variant:​

Code generation is the idea of using a tool to generate code for us. In Dart, it comes with the downside of requiring an extra step to "compile" an application. Although this problem may be solved in the near future, as the Dart team is working on a potential solution to this problem.

In the context of Riverpod, code generation is about slightly changing the syntax for defining a "provider". For example, instead of:

Using code generation, we would write:

When using Riverpod, code generation is completely optional. It is entirely possible to use Riverpod without. At the same time, Riverpod embraces code generation and recommends using it.

For information on how to install and use Riverpod's code generator, refer to the Getting started page. Make sure to enable code generation in the documentation's sidebar.

Code generation is optional in Riverpod. With that in mind, you may wonder if you should use it or not.

The answer is: Only if you already use code-generation for other things. (cf Freezed, json_serializable, etc.) When the Dart team was working on a feature called "macros", using code generation was the recommended way to use Riverpod. Unfortunately, those have been cancelled.

While code-generation brings many benefits, it currently is still fairly slow. The Dart team is working on improving the performance of code generation, but it is unclear when that will be available and how much it will improve. As such, if you are not already using code generation in your project, it is probably not worth it to start using it just for Riverpod.

At the same time, many applications already use code generation with packages such as Freezed or json_serializable. In that case, your project probably is already set up for code generation, and using Riverpod should be simple.

You may be wondering: "If code generation is optional in Riverpod, why use it?"

As always with packages: To make your life easier. This includes but is not limited to:

When defining a provider using code generation, it is helpful to keep in mind the following points:

When using code generation, providers are autoDispose by default. That means that they will automatically dispose of themselves when there are no listeners attached to them (ref.watch/ref.listen). This default setting better aligns with Riverpod's philosophy. Initially with the non-code generation variant, autoDispose was off by default to accommodate users migrating from package:provider.

If you want to disable autoDispose, you can do so by passing keepAlive: true to the annotation.

When using code generation, we no-longer need to rely on the family modifier to pass parameters to a provider. Instead, the main function of our provider can accept any number of parameters, including named, optional, or default values. Do note however that these parameters should still have a consistent ==. Meaning either the values should be cached, or the parameters should override ==.

When using non-code-generation variant, it is necessary to manually determine the type of your provider. The following are the corresponding options for transitioning into code-generation variant:

**Examples:**

Example 1 (dart):
```dart
final fetchUserProvider = FutureProvider.autoDispose.family<User, int>((  ref,  userId,) async {  final json = await http.get('api/user/$userId');  return User.fromJson(json);});
```

Example 2 (dart):
```dart
@riverpodFuture<User> fetchUser(Ref ref, {required int userId}) async {  final json = await http.get('api/user/$userId');  return User.fromJson(json);}
```

Example 3 (dart):
```dart
@riverpodString example(Ref ref) {  return 'foo';}
```

Example 4 (dart):
```dart
@riverpodclass Example extends _$Example {  @override  String build() {    return 'foo';  }  // Add methods to mutate the state}
```

---
