# Riverpod-V3 - Hooks

**Pages:** 1

---

## About hooks

**URL:** https://riverpod.dev/docs/concepts/about_hooks

**Contents:**
- About hooks
- Should you use hooks?​
- What are hooks?​
- The rules of hooks​
- Hooks and Riverpod​
  - Installation​
  - Usage​

This page explains what hooks are and how they are related to Riverpod.

"Hooks" are utilities common from a separate package, independent from Riverpod: flutter_hooks. Although flutter_hooks is a completely separate package and does not have anything to do with Riverpod (at least directly), it is common to pair Riverpod and flutter_hooks together.

Hooks are a powerful tool, but they are not for everyone. If you are a newcomer to Riverpod, avoid using hooks.

Although useful, hooks are not necessary for Riverpod. You shouldn't start using hooks because of Riverpod. Rather, you should start using hooks because you want to use hooks.

Using hooks is a tradeoff. They can be great for producing robust and reusable code, but they are also a new concept to learn, and they can be confusing at first. Hooks aren't a core Flutter concept. As such, they will feel out of place in Flutter/Dart.

Hooks are functions used inside widgets. They are designed as an alternative to StatefulWidgets, to make logic more reusable and composable.

Hooks are a concept coming from React, and flutter_hooks is merely a port of the React implementation to Flutter. As such, yes, hooks may feel a bit out of place in Flutter. Ideally, in the future we would have a solution to the problem that hooks solves, designed specifically for Flutter.

If Riverpod's providers are for "global" application state, hooks are for local widget state. Hooks are typically used for dealing with stateful UI objects, such as TextEditingController, AnimationController. They can also serve as a replacement to the "builder" pattern, replacing widgets such as FutureBuilder/TweenAnimatedBuilder by an alternative that does not involve "nesting" – drastically improving readability.

In general, hooks are helpful for:

As an example, we could use hooks to manually implement a fade-in animation, where a widget starts invisible and slowly appears.

If we were to use StatefulWidget, the code would look like this:

Using hooks, the equivalent would be:

There are a few interesting things to note in this code:

There is no memory leak. This code does not recreate a new AnimationController whenever the widget rebuilds, and the controller is correctly released when the widget is unmounted.

It is possible to use hooks as many time as we want within the same widget. As such, we can create multiple AnimationController if we want:

This creates two controllers, without any sort of negative consequence.

If we wanted, we could refactor this logic into a separate reusable function:

We could then use this function inside our widgets, as long as that widget is a HookWidget:

Note how our useFadeIn function is completely independent from our FadeIn widget. If we wanted, we could use that useFadeIn function in a completely different widget, and it would still work!

Hooks comes with unique constraints:

They can only be used within the build method of a widget that extends HookWidget:

They cannot be used conditionally or in a loop.

For more information about hooks, see flutter_hooks.

Since hooks are independent from Riverpod, it is necessary to install hooks separately. If you want to use them, installing hooks_riverpod is not enough. You will still need to add flutter_hooks to your dependencies. See Getting started) for more information.

In some cases, you may want to write a Widget that uses both hooks and Riverpod. But as you may have already noticed, both hooks and Riverpod provide their own custom widget base type: HookWidget and ConsumerWidget. But classes can only extend one superclass at a time.

To solve this problem, you can use the hooks_riverpod package. This package provides a HookConsumerWidget class that combines both HookWidget and ConsumerWidget into a single type. You can therefore subclass HookConsumerWidget instead of HookWidget:

Alternatively, you can use the "builders" provided by both packages. For example, we could stick to using StatelessWidget, and use both HookBuilder and Consumer.

This approach would work without using hooks_riverpod. Only flutter_riverpod is needed.

If you like this approach, hooks_riverpod streamlines it by providing HookConsumer, which is the combination of both builders in one:

**Examples:**

Example 1 (dart):
```dart
class FadeIn extends StatefulWidget {  const FadeIn({Key? key, required this.child}) : super(key: key);  final Widget child;  @override  State<FadeIn> createState() => _FadeInState();}class _FadeInState extends State<FadeIn> with SingleTickerProviderStateMixin {  late final AnimationController animationController = AnimationController(    vsync: this,    duration: const Duration(seconds: 2),  );  @override  void initState() {    super.initState();    animationController.forward();  }  @override  void dispose() {    animationController.dispose();    super.dispose();  }  @override  Widget build(BuildContext context) {    return AnimatedBuilder(      animation: animationController,      builder: (context, child) {        return Opacity(          opacity: animationController.value,          child: widget.child,        );      },    );  }}
```

Example 2 (dart):
```dart
class FadeIn extends HookWidget {  const FadeIn({Key? key, required this.child}) : super(key: key);  final Widget child;  @override  Widget build(BuildContext context) {    // Create an AnimationController. The controller will automatically be    // disposed when the widget is unmounted.    final animationController = useAnimationController(      duration: const Duration(seconds: 2),    );    // useEffect is the equivalent of initState + didUpdateWidget + dispose.    // The callback passed to useEffect is executed the first time the hook is    // invoked, and then whenever the list passed as second parameter changes.    // Since we pass an empty const list here, that's strictly equivalent to `initState`.    useEffect(() {      // start the animation when the widget is first rendered.      animationController.forward();      // We could optionally return some "dispose" logic here      return null;    }, const []);    // Tell Flutter to rebuild this widget when the animation updates.    // This is equivalent to AnimatedBuilder    useAnimation(animationController);    return Opacity(      opacity: animationController.value,      child: child,    );  }}
```

Example 3 (dart):
```dart
@overrideWidget build(BuildContext context) {  final animationController = useAnimationController(    duration: const Duration(seconds: 2),  );  final anotherController = useAnimationController(    duration: const Duration(seconds: 2),  );  ...}
```

Example 4 (dart):
```dart
double useFadeIn() {  final animationController = useAnimationController(    duration: const Duration(seconds: 2),  );  useEffect(() {    animationController.forward();    return null;  }, const []);  useAnimation(animationController);  return animationController.value;}
```

---
