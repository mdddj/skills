# Riverpod-V3 - Concepts

**Pages:** 18

---

## FAQ

**URL:** https://riverpod.dev/docs/root/FAQ

**Contents:**
- FAQ
- What is the difference between ref.refresh and ref.invalidate?​
- Why is there no shared interface between Ref and WidgetRef?​
- Why do we need to extend ConsumerWidget instead of using the raw StatelessWidget?​
- Why doesn't hooks_riverpod export flutter_hooks?​
- Is there a way to reset all providers at once?​
- I have the error "Using "ref" when a widget is about to or has been unmounted is unsafe." after the widget was disposed", what's wrong?​

Here are some commonly asked questions from the community:

You may have noticed that ref has two methods to force a provider to recompute, and wonder how they differ.

It's simpler than you think: ref.refresh is nothing but syntax sugar for invalidate + read:

If you do not care about the new value of a provider after recomputing it, then invalidate is the way to go. If you do, use refresh instead.

This logic is automatically enforced through lint rules. If you tried to use ref.refresh without using the returned value, you would get a warning.

The main difference in behavior is by reading the provider right after invalidating it, the provider immediately recomputes. Whereas if we call invalidate but don't read it right after, then the update will trigger later.

That "later" update is generally at the start of the next frame. Yet, if a provider that is currently not being listened to is invalidated, it will not be updated until it is listened to again.

Riverpod voluntarily dissociates Ref and WidgetRef. This is done on purpose to avoid writing code which conditionally depends on one or the other.

One issue is that Ref and WidgetRef, although similar looking, have subtle differences. Code relying on both would be unreliable in ways that are difficult to spot.

At the same time, relying on WidgetRef is equivalent to relying on BuildContext. It is effectively putting your logic in the UI layer, which is not recommended.

Such code should be refactored to always use Ref.

The solution to this problem is typically to move your logic into a Notifier (see Providers), and then have your logic be a method of that Notifier.

This way, when your widgets want to invoke this logic, they can write something along the lines of:

yourMethod would use the Notifier's Ref to interact with other providers.

This is due to an unfortunate limitation in the API of InheritedWidget.

There are a few problems:

It is not possible to implement an "on change" listener with InheritedWidget. That means that something such as ref.listen cannot be used with BuildContext.

State.didChangeDependencies is the closest thing to it, but it is not reliable. One issue is that the life-cycle can be triggered even if no dependency changed, especially if your widget tree uses GlobalKeys (and some Flutter widgets already do so internally).

Widgets listening to an InheritedWidget never stop listening to it. This is usually fine for pure metadata, such as "theme" or "media query".

For business logic, this is a problem. Say you use a provider to represent a paginated API. When the page offset changes, you wouldn't want your widget to keep listening to the previously visible pages.

InheritedWidget has no way to track when widgets stop listening to them. Riverpod sometimes relies on tracking whether or not a provider is being listened to.

This functionality is crucial for both the auto dispose mechanism and the ability to pass arguments to providers. Those features are what make Riverpod so powerful.

Maybe in a distant future, those issues will be fixed. In that case, Riverpod would migrate to using BuildContext instead of Ref. This would enable using StatelessWidget instead of ConsumerWidget. But that's for another time!

This is to respect good versioning practices.

While you cannot use hooks_riverpod without flutter_hooks, both packages are versioned independently. A breaking change could happen in one but not the other.

No, there is no way to reset all providers at once.

This is on purpose, as it is considered an anti-pattern. Resetting all providers at once will often reset providers that you did not intend to reset.

This is commonly asked by users who want to reset the state of their application when the user logs out. If this is what you are after, you should instead have everything dependent on the user's state to ref.watch the "user" provider.

Then, when the user logs out, all providers depending on it would automatically be reset but everything else would remain untouched.

You might also see "Bad state: No ProviderScope found", which is an older error message of the same issue.

This error happens when you try to use ref in a widget that is no longer mounted. This generally happens after an await:

The solution is to, like with BuildContext, check mounted before using ref:

**Examples:**

Example 1 (dart):
```dart
T refresh<T>(provider) {  invalidate(provider);  return read(provider);}
```

Example 2 (dart):
```dart
ref.read(yourNotifierProvider.notifier).yourMethod();
```

Example 3 (dart):
```dart
ElevatedButton(  onPressed: () async {    await future;    ref.read(...); // May throw "Using "ref" when a widget is about to or has been unmounted is unsafe."  })
```

Example 4 (dart):
```dart
ElevatedButton(  onPressed: () async {    await future;    if (!context.mounted) return;    ref.read(...); // No longer throws  })
```

---

## Implementing pull-to-refresh

**URL:** https://riverpod.dev/docs/how_to/pull_to_refresh

**Contents:**
- Implementing pull-to-refresh
- Making a bare-bones application.​
- Adding RefreshIndicator​
- Adding the refresh logic​
- Showing a spinner only during initial load and handling errors.​
- Wrapping up: full application​

Riverpod natively supports pull-to-refresh thanks to its declarative nature.

In general, pull-to-refreshes can be complex due as there are multiple problems to solve:

Let's see how to solve this using Riverpod. For this, we will make a simple example which recommends a random activity to users. And doing a pull-to-refresh will trigger a new suggestion:

Before implement a pull-to-refresh, we first need something to refresh. We can make a simple application which uses Bored API to suggests a random activity to users.

First, let's define an Activity class:

That class will be responsible for representing a suggested activity in a type-safe manner, and handle JSON encoding/decoding. Using Freezed/json_serializable is not required, but it is recommended.

Now, we'll want to define a provider making a HTTP GET request to fetch a single activity:

We can now use this provider to display a random activity. For now, we will not handle the loading/error state, and simply display the activity when available:

Now that we have a simple application, we can add a RefreshIndicator to it. That widget is an official Material widget responsible for displaying a refresh indicator when the user pulls down the screen.

Using RefreshIndicator requires a scrollable surface. But so far, we don't have any. We can fix that by using a ListView/GridView/SingleChildScrollView/etc:

Users can now pull down the screen. But our data isn't refreshed yet.

When users pull down the screen, RefreshIndicator will invoke the onRefresh callback. We can use that callback to refresh our data. In there, we can use ref.refresh to refresh the provider of our choice.

Note: onRefresh is expected to return a Future. And it is important for that future to complete when the refresh is done.

To obtain such a future, we can read our provider's .future property. This will return a future which completes when our provider has resolved.

We can therefore update our RefreshIndicator to look like this:

At the moment, our UI does not handle the error/loading states. Instead the data magically pops up when the loading/refresh is done.

Let's change this by gracefully handling those states. There are two cases:

Fortunately, when listening to an asynchronous provider in Riverpod, Riverpod gives us an AsyncValue, which offers everything we need.

That AsyncValue can then be combined with Dart 3.0's pattern matching as follows:

We use valueOrNull here, as currently, using value throws if in error/loading state.

Riverpod 3.0 will change this to have value behave like valueOrNull. But for now, let's stick to valueOrNull.

Notice the usage of the :final valueOrNull? syntax in our pattern matching. This syntax can be used only because activityProvider returns a non-nullable Activity.

If your data can be null, you can instead use AsyncValue(hasData: true, :final valueOrNull). This will correctly handle cases where the data is null, at the cost of a few extra characters.

Here is the combined source of everything we've covered so far:

**Examples:**

Example 1 (dart):
```dart
class Activity {  Activity({    required this.activity,    required this.type,    required this.participants,    required this.price,  });  factory Activity.fromJson(Map<Object?, Object?> json) {    return Activity(      activity: json['activity']! as String,      type: json['type']! as String,      participants: json['participants']! as int,      price: json['price']! as double,    );  }  final String activity;  final String type;  final int participants;  final double price;}
```

Example 2 (dart):
```dart
@freezedsealed class Activity with _$Activity {  factory Activity({    required String activity,    required String type,    required int participants,    required double price,  }) = _Activity;  factory Activity.fromJson(Map<String, dynamic> json) =>      _$ActivityFromJson(json);}
```

Example 3 (dart):
```dart
final activityProvider = FutureProvider.autoDispose<Activity>((ref) async {  final response = await http.get(    Uri.https('www.boredapi.com', '/api/activity'),  );  final json = jsonDecode(response.body) as Map;  return Activity.fromJson(json);});
```

Example 4 (dart):
```dart
@riverpodFuture<Activity> activity(Ref ref) async {  final response = await http.get(    Uri.https('www.boredapi.com', '/api/activity'),  );  final json = jsonDecode(response.body) as Map;  return Activity.fromJson(Map.from(json));}
```

---

## Refs

**URL:** https://riverpod.dev/docs/concepts2/refs

**Contents:**
- Refs
- How to obtain a Ref​
- Using Refs to interact with providers​
  - Listening to a provider's state​
    - Ref.watch​
    - Ref.listen​
  - Resetting a provider's state​
  - Interacting with the provider's state inside user interactions​
  - Listening to life-cycle events​

Refs are the primary way to interact with Providers. Refs are fairly similar to the BuildContext in Flutter, but for providers instead of widgets. A non-exhaustive list of things you can do with a ref:

On top of that, Ref also enables a provider to observe life-cycles about its own state. Think "initState" and "dispose", but for providers. This includes methods such as:

Obtaining a Ref depends on where you are in your app.

Providers naturally have access to a Ref. You can find it as parameter of the initializer function, or as a property of Notifier classes.

To obtain a Ref inside widgets, you need Consumers.

I am never inside a widget, nor a provider. How do I get a Ref then? If you are neither inside widgets nor providers, chances are whatever you are using is still loosely connected to a widget/provider.

In that case, simply pass the ref you obtained from your widget/provider to your function/object of choice:

Interactions with providers generally fall under two categories:

Riverpod offers two ways to listen to a provider's state:

For the following examples, consider a provider that updates every second:

Ref.watch is Riverpod's defining feature. It enables combining providers together seamlessly, and easily have your UI update when a provider's state changes.

Using Ref.watch is similar to using an InheritedWidget in Flutter. In Flutter, when you call Theme.of(context), your widget subscribes to the Theme and will rebuild whenever the Theme changes. Similarly, when you call ref.watch(myProvider), your widget/provider subscribes to myProvider, and will rebuild whenever myProvider changes.

The following code shows a Consumers that automatically updates whenever our Tick provider updates:

The most interesting part of Ref.watch is that providers can use it too! For example, we could create a provider that returns "is tick divisible by 4?":

Then, we could listen to this new provider in our UI instead:

Now, instead of updating every second, our UI will only update when the boolean value changes.

Ref.listen is a more manual way of listening to providers. It is similar to the addListener method of ChangeNotifier, or the Stream.listen method.

This method is useful when you want to perform a side-effect when a provider's state changes, such as

It is safe to use WidgetRef.listen inside the build method of a widget. This is how the method is designed to be used. If you want to listen to providers outside of build (such as State.initState), use WidgetRef.listenManual instead.

Using Ref.invalidate, you can reset a provider's state. This will tell Riverpod to discard the current state and re-evaluate the provider the next time it is read.

The following example will reset the tick to 0:

If you need to obtain the new state right after resetting it, you can call Ref.read:

Alternatively, you can use Ref.refresh to reset the provider and read its new state in one go:

Both codes are strictly equivalent. Ref.refresh is syntax sugar for Ref.invalidate followed by Ref.read.

A last use-case is to interact with a provider's state inside button clicks. In this scenario, we do not want to "listen" to the state. For this case, Ref.read exists.

You can safely call Ref.read button clicks to perform work. The following example will print the current tick value when the button is clicked:

Do not use Ref.read as a mean to "optimize" your code by avoiding Ref.watch. This will make your code more brittle, as changes in your provider's behavior could cause your UI to be out of sync with the provider's state.

Either use Ref.watch anyway (as the difference is negligible) or use select:

A provider-specific feature of Ref is the ability to listen to life-cycle events. These events are similar to the initState, dispose, and other life-cycle methods in Flutter widgets.

Life-cycles listeners are registered using an "addListener" style API. Listeners are methods with a name that starts with on, such as onDispose or onCancel.

You do not need to "unregister" these listeners. Riverpod automatically cleans them up when the provider is reset.

Although if you wish to unregister them manually, you can do so by using the return value of the listener method.

**Examples:**

Example 1 (dart):
```dart
final myProvider = Provider<int>((ref) {  // ref is available here  ...});final myNotifierProvider = NotifierProvider<MyNotifier, int>(MyNotifier.new);class MyNotifier extends Notifier<int> {  @override  int build() {    // this.ref is available anywhere inside notifiers    ref.watch(someProvider);    ...  }}
```

Example 2 (dart):
```dart
@riverpodint myProvider(Ref ref) {  // ref is available here  ...}@riverpodclass MyNotifier extends _$MyNotifier {  @override  int build() {    // this.ref is available anywhere inside notifiers    ref.watch(someProvider);    ...  }}
```

Example 3 (dart):
```dart
Consumer(  builder: (context, ref, _) {    // ref is available here    final value = ref.watch(myProvider);    return Text('$value');  },);
```

Example 4 (dart):
```dart
void myFunction(WidgetRef ref) {  // You can pass the ref around!}...Consumer(  builder: (context, ref, _) {    return ElevatedButton(      onPressed: () => myFunction(ref), // Pass the ref to your function      child: Text('Click me'),    );  },);
```

---

## How to eagerly initialize providers

**URL:** https://riverpod.dev/docs/how_to/eager_initialization

**Contents:**
- How to eagerly initialize providers
  - FAQ​
    - Won't this rebuild our entire application when the provider changes?​
    - Using this approach, how can I handle loading and error states?​
    - I've handled loading/error states, but other Consumers still receive an AsyncValue! Is there a way to not have to handle loading/error states in every widget?​

All providers are initialized lazily by default. This means that the provider is only initialized when it is first used. This is useful for providers that are only used in certain parts of the application.

Unfortunately, there is no way to flag a provider as needing to be eagerly initialized due to how Dart works (for tree shaking purposes). One solution, however, is to forcibly read the providers you want to eagerly initialize at the root of your application.

The recommended approach is to simply "watch" a provider in a Consumer placed right under your ProviderScope:

Consider putting the initialization consumer in your "MyApp" or in a public widget. This enables your tests to use the same behavior, by removing logic from your main.

No, this is not the case. In the sample given above, the consumer responsible for eagerly initializing is a separate widget, which does nothing but return a child.

The key part is that it returns a child, rather than instantiating MaterialApp itself. This means that if _EagerInitialization ever rebuilds, the child variable will not have changed. And when a widget doesn't change, Flutter doesn't rebuild it.

As such, only _EagerInitialization will rebuild, unless another widget is also listening to that provider.

You can handle loading/error states as you normally would in a Consumer. Your _EagerInitialization could check if a provider is in a "loading" state, and if so, return a CircularProgressIndicator instead of the child:

Rather than trying to have your provider not expose an AsyncValue, you can instead have your widgets use AsyncValue.requireValue. This will read the data without having to do pattern matching. And in case a bug slips through, it will throw an exception with a clear message.

Although there are ways to not expose the loading/error states in those cases (relying on scoping), it is generally discouraged to do so. The added complexity of making two providers and using overrides is not worth the trouble.

**Examples:**

Example 1 (dart):
```dart
void main() {  runApp(ProviderScope(child: MyApp()));}class MyApp extends StatelessWidget {  @override  Widget build(BuildContext context) {    return const _EagerInitialization(      // TODO: Render your app here      child: MaterialApp(),    );  }}class _EagerInitialization extends ConsumerWidget {  const _EagerInitialization({required this.child});  final Widget child;  @override  Widget build(BuildContext context, WidgetRef ref) {    // Eagerly initialize providers by watching them.    // By using "watch", the provider will stay alive and not be disposed.    ref.watch(myProvider);    return child;  }}
```

Example 2 (dart):
```dart
class _EagerInitialization extends ConsumerWidget {  const _EagerInitialization({required this.child});  final Widget child;  @override  Widget build(BuildContext context, WidgetRef ref) {    final result = ref.watch(myProvider);    // Handle error states and loading states    if (result.isLoading) {      return const CircularProgressIndicator();    } else if (result.hasError) {      return const Text('Oopsy!');    }    return child;  }}
```

Example 3 (dart):
```dart
// An eagerly initialized provider.final exampleProvider = FutureProvider<String>((ref) async => 'Hello world');class MyConsumer extends ConsumerWidget {  @override  Widget build(BuildContext context, WidgetRef ref) {    final result = ref.watch(exampleProvider);    /// If the provider was correctly eagerly initialized, then we can    /// directly read the data with "requireValue".    return Text(result.requireValue);  }}
```

Example 4 (dart):
```dart
// An eagerly initialized provider.@riverpodFuture<String> example(Ref ref) async => 'Hello world';class MyConsumer extends ConsumerWidget {  @override  Widget build(BuildContext context, WidgetRef ref) {    final result = ref.watch(exampleProvider);    /// If the provider was correctly eagerly initialized, then we can    /// directly read the data with "requireValue".    return Text(result.requireValue);  }}
```

---

## Automatic disposal

**URL:** https://riverpod.dev/docs/concepts2/auto_dispose

**Contents:**
- Automatic disposal
- Enabling/disabling automatic disposal​
- When is automatic disposal triggered?​
- Reacting to state disposal​
- Manually forcing the destruction of a provider, using ref.invalidate​
- Fine-tuned disposal with ref.keepAlive​
- Example: keeping state alive for a specific amount of time​

In Riverpod, it is possible to tell the framework to automatically destroy resources associated with a provider when it is no longer used.

If you're using code-generation, this is enabled by default, and can be opted out in the annotation:

If you're not using code-generation, you can enable it by using isAutoDispose: true when creating the provider:

Enabling/disabling automatic disposal has no impact on whether or not the state is destroyed when the provider is recomputed. The state will always be destroyed when the provider is recomputed.

When providers receive parameters, it is recommended to enable automatic disposal. That is because otherwise, one state per parameter combination will be created, which can lead to memory leaks.

When automatic disposal is enabled, Riverpod will track whether a provider has listeners or not. This happens by tracking Ref.watch/Ref.listen calls (and a few others).

When that counter reaches zero, the provider is considered "not used", and Ref.onCancel is triggered. At that point, Riverpod waits for one frame (cf. await null). If, after that frame, the provider is still not used, then the provider is destroyed and Ref.onDispose will be triggered.

In Riverpod, there are a few built-in ways for state to be destroyed:

In both cases, you may want to execute some logic when that happens. This can be achieved with ref.onDispose. This method enables registering a listener for whenever the state is destroyed.

For example, you may want to use it to close any active StreamController:

The callback of ref.onDispose must not trigger side-effects. Modifying providers inside onDispose could lead to unexpected behavior.

There are other useful life-cycles such as:

You can call ref.onDispose as many times as you wish. Feel free to call it once per disposable object in your provider. This practice makes it easier to spot when we forget to dispose of something.

Sometimes, you may want to force the destruction of a provider. This can be done by using ref.invalidate, which can be called from another provider or a widget.

Using ref.invalidate will destroy the current provider state. There are then two possible outcomes:

It is possible for providers to invalidate themselves by using ref.invalidateSelf. Although in this case, this will always result in a new state being created.

When trying to invalidate a provider which receives parameters, it is possible to either invalidate one specific parameter combination, or all parameter combinations at once:

As mentioned above, when automatic disposal is enabled, the state is destroyed when the provider has no listeners for a full frame.

But you may want to have more control over this behavior. For instance, you may want to keep the state of successful network requests, but not cache failed requests.

This can be achieved with ref.keepAlive, after enabling automatic disposal. Using it, you can decide when the state stops being automatically disposed.

If the provider is recomputed, automatic disposal will be re-enabled.

It is also possible to use the return value of ref.keepAlive to revert to automatic disposal.

Currently, Riverpod does not offer a built-in way to keep state alive for a specific amount of time. But implementing such a feature is easy and reusable with the tools we've seen so far.

By using a Timer + ref.keepAlive, we can keep the state alive for a specific amount of time. To make this logic reusable, we could implement it in an extension method:

Then, we can use it like so:

This logic can be tweaked to fit your needs. For example, you could use ref.onCancel/ref.onResume to destroy the state only if a provider hasn't been listened to for a specific amount of time.

**Examples:**

Example 1 (dart):
```dart
// Disable automatic disposal@Riverpod(keepAlive: true)String helloWorld(Ref ref) => 'Hello world!';
```

Example 2 (dart):
```dart
final helloWorldProvider = Provider<String>(  // Opt-in to automatic disposal  isAutoDispose: true,  (ref) => 'Hello world!',);
```

Example 3 (dart):
```dart
final provider = StreamProvider<int>((ref) {  final controller = StreamController<int>();  // When the state is destroyed, we close the StreamController.  ref.onDispose(controller.close);  // TO-DO: Push some values in the StreamController  return controller.stream;});
```

Example 4 (dart):
```dart
@riverpodStream<int> example(Ref ref) {  final controller = StreamController<int>();  // When the state is destroyed, we close the StreamController.  ref.onDispose(controller.close);  // TO-DO: Push some values in the StreamController  return controller.stream;}
```

---

## Providers

**URL:** https://riverpod.dev/docs/concepts2/providers

**Contents:**
- Providers
- What is a provider?​
- Creating a provider​
- Using providers​

Providers are the central feature of Riverpod. If you use Riverpod, you use it for its providers.

Providers are essentially "memoized functions", with sugar on top. What this means is, providers are functions which will return a cached value when called with the same parameters.

The most common use-case for using providers is to perform a network request. Consider a function that fetches a user from an API:

One issue with this function is, if we were to try using it inside widgets, we'd have to cache the result ourselves ; then figure out a way to share the value across all widgets that need it.

That is where providers come in. Providers are wrappers around functions. They will cache the result of said function and allow multiple widgets to access the same value:

On top of basic caching, providers then add various features to make them more powerful:

Providers come 6 variants:

This may seem overwhelming at first. Let's break it down.

Sync vs Future vs Stream: The columns of this table represent the built-in Dart types for functions.

Unmodifiable vs Modifiable: By default, providers are not modifiable by widgets. The "Notifier" variant of providers make them externally modifiable. This is similar to a private setter ("unmodifiable" providers)

VS a public setter ("modifiable" providers)

You can also view unmodifiable vs modifiable as respectively StatelessWidget vs StatefulWidget in principle.

This is not entirely accurate, as providers are not widgets and both kinds store "state". But the principle is similar: "One object, immutable" vs "Two objects, mutable".

Providers should be created as "top-level" declarations. This means that they should be declared outside of any class or function.

The syntax for creating a provider depends on whether it is "modifiable" or "unmodifiable", as per the table above.

This variable is what will be used to interact with our provider. The variable must be final and "top-level" (global).

Do not be frightened by the global aspect of providers. Providers are fully immutable. Declaring a provider is no different from declaring a function, and providers are testable and maintainable.

Generally either Provider, FutureProvider or StreamProvider. The type of provider used depends on the return value of your function. For example, to create a Future<Activity>, you'll want a FutureProvider<Activity>.

FutureProvider is the one you'll want to use the most.

Don't think in terms of "Which provider should I pick". Instead, think in terms of "What do I want to return". The provider type will follow naturally.

Often, after the type of the provider you may see a "modifier". Modifiers are optional, and are used to tweak the behavior of the provider in a way that is type-safe.

There are currently two modifiers available:

An object used to interact with other providers. All providers have one; either as parameter of the provider function, or as a property of a Notifier.

This is where we place the logic of our providers. This function will be called when the provider is first read. Subsequent reads will not call the function again, but instead return the cached value.

All generated providers must be annotated with @riverpod or @Riverpod(). This annotation can be placed on global functions or classes. Through this annotation, it is possible to configure the provider.

For example, we can disable "auto-dispose" (which we will see later) by writing @Riverpod(keepAlive: true).

The name of the annotated function determines how the provider will be interacted with. For a given function myFunction, a myFunctionProvider will be generated.

Annotated functions must specify a Ref as first parameter. Besides that, the function can have any number of parameters, including generics. The function is also free to return a Future/Stream if it wishes to.

This function will be called when the provider is first read. Subsequent reads will not call the function again, but instead return the cached value.

An object used to interact with other providers. All providers have one; either as parameter of the provider function, or as a property of a Notifier. The type of this object is determined by the name of the function/class.

This variable is what will be used to interact with our provider. The variable must be final and "top-level" (global).

Do not be frightened by the global aspect of providers. Providers are fully immutable. Declaring a provider is no different from declaring a function, and providers are testable and maintainable.

Generally either NotifierProvider, AsyncNotifierProvider or StreamNotifierProvider. The type of provider used depends on the return value of your function. For example, to create a Future<Activity>, you'll want a AsyncNotifierProvider<Activity>.

AsyncNotifierProvider is the one you'll want to use the most.

As with functional providers, don't think in terms of "Which provider should I pick". Create whatever state you want to create, and the provider type will follow naturally.

Often, after the type of the provider you may see a "modifier". Modifiers are optional, and are used to tweak the behavior of the provider in a way that is type-safe.

There are currently two modifiers available:

The parameter of "notifier providers" is a function which is expected to instantiate the "notifier". It generally should be a "constructor tear-off".

If NotifierProvider is equivalent to StatefulWidget, then this part is the State class.

This class is responsible for exposing ways to modify the state of the provider. Public methods on this class are accessible to consumers using ref.read(yourProvider.notifier).yourMethod().

Do not put logic in the constructor of your notifier. Notifiers should not have a constructor, as ref and other properties aren't yet available at that point. Instead, put your logic in the build method.

The base class extended by your notifier should match that of the provider + "family", if used. Some examples would be:

All notifiers must override the build method. This method is equivalent to the place where you would normally put your logic in a non-notifier provider.

This method should not be called directly.

All providers must be annotated with @riverpod or @Riverpod(). This annotation can be placed on global functions or classes. Through this annotation, it is possible to configure the provider.

For example, we can disable "auto-dispose" (which we will see later) by writing @Riverpod(keepAlive: true).

When a @riverpod annotation is placed on a class, that class is called a "Notifier". The class must extend _$NotifierName, where NotifierName is the class name.

Notifiers are responsible for exposing ways to modify the state of the provider. Public methods on this class are accessible to consumers using ref.read(yourProvider.notifier).yourMethod().

Do not put logic in the constructor of your notifier. Notifiers should not have a constructor, as ref and other properties aren't yet available at that point. Instead, put your logic in the build method.

All notifiers must override the build method. This method is equivalent to the place where you would normally put your logic in a non-notifier provider.

This method should not be called directly.

You can declare as many providers as you want without limitations. As opposed to when using package:provider, Riverpod allows creating multiple providers exposing a state of the same "type":

The fact that both providers create a String does not cause any problem.

Providers are similar to widgets in that, on their own, they do nothing. Similarly to how widgets are a description of the UI, providers are a description of the state. Surprisingly, a provider is fully stateless, and could be instantiated as const if not for the fact that this would make the syntax a bit more verbose.

To use a provider, you need a separate object: ProviderContainer. See ProviderContainers/ProviderScopes for more information.

Long story short, before you can use a provider, wrap your Flutter applications in a ProviderScope:

Once that is done, you will need to obtain a Ref to interact with your providers. See Refs for information about those.

In short, there are two ways to obtain a Ref:

As example, consider a helloWorldProvider that returns a simple string. You could use it inside widgets like this:

**Examples:**

Example 1 (dart):
```dart
Future<User> fetchUser() async {  final response = await http.get('https://api.example.com/user/123');  return User.fromJson(response.body);}
```

Example 2 (dart):
```dart
// The equivalent of our fetchUser function, but the result is cached.// Using userProvider multiple times will return the same value.final userProvider = FutureProvider<User>((ref) async {  final response = await http.get('https://api.example.com/user/123');  return User.fromJson(response.body);});
```

Example 3 (dart):
```dart
// The equivalent of our fetchUser function, but the result is cached.// This will generate a "userProvider". Using it multiple times will// return the same value.@riverpodFuture<User> user(Ref ref) async {  final response = await http.get('https://api.example.com/user/123');  return User.fromJson(response.body);}
```

Example 4 (dart):
```dart
int synchronous() => 0;Future<int> future() async => 0;Stream<int> stream() => Stream.value(0);
```

---

## FAQ

**URL:** https://riverpod.dev/docs/root/faq

**Contents:**
- FAQ
- What is the difference between ref.refresh and ref.invalidate?​
- Why is there no shared interface between Ref and WidgetRef?​
- Why do we need to extend ConsumerWidget instead of using the raw StatelessWidget?​
- Why doesn't hooks_riverpod export flutter_hooks?​
- Is there a way to reset all providers at once?​
- I have the error "Using "ref" when a widget is about to or has been unmounted is unsafe." after the widget was disposed", what's wrong?​

Here are some commonly asked questions from the community:

You may have noticed that ref has two methods to force a provider to recompute, and wonder how they differ.

It's simpler than you think: ref.refresh is nothing but syntax sugar for invalidate + read:

If you do not care about the new value of a provider after recomputing it, then invalidate is the way to go. If you do, use refresh instead.

This logic is automatically enforced through lint rules. If you tried to use ref.refresh without using the returned value, you would get a warning.

The main difference in behavior is by reading the provider right after invalidating it, the provider immediately recomputes. Whereas if we call invalidate but don't read it right after, then the update will trigger later.

That "later" update is generally at the start of the next frame. Yet, if a provider that is currently not being listened to is invalidated, it will not be updated until it is listened to again.

Riverpod voluntarily dissociates Ref and WidgetRef. This is done on purpose to avoid writing code which conditionally depends on one or the other.

One issue is that Ref and WidgetRef, although similar looking, have subtle differences. Code relying on both would be unreliable in ways that are difficult to spot.

At the same time, relying on WidgetRef is equivalent to relying on BuildContext. It is effectively putting your logic in the UI layer, which is not recommended.

Such code should be refactored to always use Ref.

The solution to this problem is typically to move your logic into a Notifier (see Providers), and then have your logic be a method of that Notifier.

This way, when your widgets want to invoke this logic, they can write something along the lines of:

yourMethod would use the Notifier's Ref to interact with other providers.

This is due to an unfortunate limitation in the API of InheritedWidget.

There are a few problems:

It is not possible to implement an "on change" listener with InheritedWidget. That means that something such as ref.listen cannot be used with BuildContext.

State.didChangeDependencies is the closest thing to it, but it is not reliable. One issue is that the life-cycle can be triggered even if no dependency changed, especially if your widget tree uses GlobalKeys (and some Flutter widgets already do so internally).

Widgets listening to an InheritedWidget never stop listening to it. This is usually fine for pure metadata, such as "theme" or "media query".

For business logic, this is a problem. Say you use a provider to represent a paginated API. When the page offset changes, you wouldn't want your widget to keep listening to the previously visible pages.

InheritedWidget has no way to track when widgets stop listening to them. Riverpod sometimes relies on tracking whether or not a provider is being listened to.

This functionality is crucial for both the auto dispose mechanism and the ability to pass arguments to providers. Those features are what make Riverpod so powerful.

Maybe in a distant future, those issues will be fixed. In that case, Riverpod would migrate to using BuildContext instead of Ref. This would enable using StatelessWidget instead of ConsumerWidget. But that's for another time!

This is to respect good versioning practices.

While you cannot use hooks_riverpod without flutter_hooks, both packages are versioned independently. A breaking change could happen in one but not the other.

No, there is no way to reset all providers at once.

This is on purpose, as it is considered an anti-pattern. Resetting all providers at once will often reset providers that you did not intend to reset.

This is commonly asked by users who want to reset the state of their application when the user logs out. If this is what you are after, you should instead have everything dependent on the user's state to ref.watch the "user" provider.

Then, when the user logs out, all providers depending on it would automatically be reset but everything else would remain untouched.

You might also see "Bad state: No ProviderScope found", which is an older error message of the same issue.

This error happens when you try to use ref in a widget that is no longer mounted. This generally happens after an await:

The solution is to, like with BuildContext, check mounted before using ref:

**Examples:**

Example 1 (dart):
```dart
T refresh<T>(provider) {  invalidate(provider);  return read(provider);}
```

Example 2 (dart):
```dart
ref.read(yourNotifierProvider.notifier).yourMethod();
```

Example 3 (dart):
```dart
ElevatedButton(  onPressed: () async {    await future;    ref.read(...); // May throw "Using "ref" when a widget is about to or has been unmounted is unsafe."  })
```

Example 4 (dart):
```dart
ElevatedButton(  onPressed: () async {    await future;    if (!context.mounted) return;    ref.read(...); // No longer throws  })
```

---

## Motivation

**URL:** https://riverpod.dev/docs/from_provider/motivation

**Contents:**
- Motivation
- Provider's Limitations​
  - Provider can't keep two (or more) providers of the same "type"​
  - Providers reasonably emit only one value at a time​
  - Combining providers is hard and error-prone​
  - Lack of safety​
  - Disposing of state is difficult​
  - Lack of a reliable parametrization mechanism​
  - Testing is tedious​
  - Triggering side effects isn't straightforward​

This in-depth article is meant to show why Riverpod is even a thing.

In particular, this section should answer the following:

By the end of this section you should be convinced that Riverpod is to be preferred over Provider.

Riverpod is indeed a more modern, recommended and reliable approach when compared to Provider.

Riverpod offers better State Management capabilities, better Caching strategies and a simplified Reactivity model. Whereas, Provider is currently lacking in many areas with no way forward.

Provider has fundamental issues due to being restricted by the InheritedWidget API. Inherently, Provider is a "simpler InheritedWidget"; Provider is merely an InheritedWidget wrapper, and thus it's limited by it.

Here's a list of known Provider issues.

Declaring two Provider<Item> will result into unreliable behavior: InheritedWidget's API will obtain only one of the two: the closest Provider<Item> ancestor. While a workaround is explained in Provider's documentation, Riverpod simply doesn't have this problem.

By removing this limitation, we can freely split logic into tiny pieces, like so:

When reading an external RESTful API, it's quite common to show the last read value, while a new call loads the next one. Riverpod allows this behavior via emitting two values at a time (i.e. a previous data value, and an incoming new loading value), via its AsyncValue's APIs:

In the previous snippet, watching evenItemsProvider will produce the following effects:

With Provider, the above features aren't remotely achievable, and even less easy to workaround.

With Provider we may be tempted to use context.watch inside provider's create. This would be unreliable, as didChangeDependencies may be triggered even if no dependency has changed (e.g. such as when there's a GlobalKey involved in the widget tree).

Nonetheless, Provider has an ad-hoc solution named ProxyProvider, but it's considered tedious and error-prone.

Combining state is a core Riverpod mechanism, as we can combine and cache values reactively with zero overhead with simple yet powerful utilities such as ref.watch and ref.listen:

Combining values feels natural with Riverpod: dependencies are readable and the APIs remain the same.

With Provider, it's common to end-up with a ProviderNotFoundException during refactors and / or during large changes. Indeed, this runtime exception was one of the main reasons Riverpod was created in the first place.

Although it brings much more utility than this, Riverpod simply can't throw this exception.

InheritedWidget can't react when a consumer stops listening to them. This prevents the ability for Provider to automatically destroy its providers' state when they're no-longer used. With Provider, we have to rely on scoping providers to dispose the state when it stops being used. But this isn't easy, as it gets tricky when state is shared between pages.

Riverpod solves this with easy-to-understand APIs such as autodispose and [Ref.keepAlive]. These two APIs enable flexible and creative caching strategies (e.g. time-based caching):

Unluckily, there's no way to implement this with a raw InheritedWidget, and thus with Provider.

Riverpod allows its user to declare "parametrized" Providers with the .family modifier. Indeed, .family is one of Riverpod's most powerful feature and it is core to its innovations, e.g. it enables enormous simplification of logic.

If we wanted to implement something similar using Provider, we would have to give up easiness of use and type-safeness on such parameters.

Furthermore, not being able to implement a similar .autoDispose mechanism with Provider inherently prevents any equivalent implementation of .family, as these two features go hand-in-hand.

Finally, as shown before, it turns out that widgets never stop to listen to an InheritedWidget. This implies significant memory leaks if some provider state is "dynamically mounted", i.e. when using parameters to a build a Provider, which is exactly what .family does. Thus, obtaining a .family equivalent for Provider is fundamentally impossible at the moment in time.

To be able to write a test, you have to re-define providers inside each test.

With Riverpod, providers are ready to use inside tests, by default. Furthermore, Riverpod exposes a handy collection of "overriding" utilities that are crucial when mocking Providers.

Testing the combined state snippet above would be as simple as the following:

Since InheritedWidget has no onChange callback, Provider can't have one. This is problematic for navigation, such as for snackbars, modals, etc.

Instead, Riverpod simply offers ref.listen, which integrates well with Flutter.

Conceptually, Riverpod and Provider are fairly similar. Both packages fill a similar role. Both try to:

You can think of Riverpod as what Provider could've been if it continued to mature for a few years.

Originally, a major version of Provider was planned to ship, as a way to solve the aforementioned problems. But it was then decided against it, as this would have been "too breaking" and even controversial, because of the new ConsumerWidget API. Since Provider is still one of the most used Flutter packages, it was instead decided to create a separate package, and thus Riverpod was created.

Creating a separate package enabled:

Indeed, Riverpod is designed to be the spiritual successor of Provider. Hence the name "Riverpod" (which is an anagram of "Provider").

The only true downside of Riverpod is that it requires changing the widget type to work:

But this inconvenience is fairly minor in the grand scheme of things. And this requirement might, one day, disappear.

You're probably asking yourself: "So, as a Provider user, should I use Provider or Riverpod?".

We want to answer to this question very clearly:

Riverpod is overall better designed and could lead to drastic simplifications of your logic.

**Examples:**

Example 1 (dart):
```dart
final itemsProvider = Provider.autoDispose(  (ref) => <Item>[], // ...);final evenItemsProvider = Provider.autoDispose((ref) {  final items = ref.watch(itemsProvider);  return [...items.whereIndexed((index, element) => index.isEven)];});
```

Example 2 (dart):
```dart
@riverpodList<Item> items(Ref ref) {  return []; // ...}@riverpodList<Item> evenItems(Ref ref) {  final items = ref.watch(itemsProvider);  return [...items.whereIndexed((index, element) => index.isEven)];}
```

Example 3 (dart):
```dart
final itemsApiProvider = FutureProvider.autoDispose((ref) async {  final client = Dio();  final result = await client.get<List<dynamic>>('your-favorite-api');  final parsed = [...result.data!.map((e) => Item.fromJson(e as Json))];  return parsed;});final evenItemsProvider = Provider.autoDispose((ref) {  final asyncValue = ref.watch(itemsApiProvider);  if (asyncValue.isLoading) return <Item>[];  if (asyncValue.hasError) return const [Item(id: -1)];  final items = asyncValue.requireValue;  return [...items.whereIndexed((index, element) => index.isEven)];});
```

Example 4 (dart):
```dart
@riverpodFuture<List<Item>> itemsApi(Ref ref) async {  final client = Dio();  final result = await client.get<List<dynamic>>('your-favorite-api');  final parsed = [...result.data!.map((e) => Item.fromJson(e as Json))];  return parsed;}@riverpodList<Item> evenItems(Ref ref) {  final asyncValue = ref.watch(itemsApiProvider);  if (asyncValue.isReloading) return [];  if (asyncValue.hasError) return const [Item(id: -1)];  final items = asyncValue.requireValue;  return [...items.whereIndexed((index, element) => index.isEven)];}
```

---

## Consumers

**URL:** https://riverpod.dev/docs/concepts2/consumers

**Contents:**
- Consumers
  - Which one to use?​
  - Why can't we use StatelessWidget + context.watch?​

A "Consumer" is a type of widget that bridges the gap between the Widget tree and the Provider tree.

The only real difference between a Consumer and typical widgets is that Consumers get access to a Ref. This enables them to read providers and listen to their changes. See Refs for more information.

Consumers come in a few different flavors, mostly for personal preference. You will find:

Alternatively, you will find extra consumers in the hooks_riverpod package. Those combine Riverpod consumers with flutter_hooks. If you don't care about hooks, you can ignore them.

The choice of which consumer to use is mostly a matter of personal preference. You could use Consumer for everything. It is a slightly more verbose option than the others. But this is a reasonable price to pay if you do not like how Riverpod hijacks StatelessWidget and StatefulWidget.

But if you do not have a strong opinion, we recommend using ConsumerWidget (or ConsumerStatefulWidget when you need a State).

In alternative packages like provider, you can use context.watch to listen to providers. This works inside any widget, as long as you have a BuildContext. So why isn't this the case in Riverpod?

The reason is that relying purely on BuildContext instead of a Ref would prevent the implementation of Riverpod's Automatic disposal in a reliable way. There are tricks to make an implementation that "mostly works" with BuildContext. The problem is that there are lots of subtle edge-cases which could silently break the auto-dispose feature.

This would cause memory leaks, but that's not the real issue. Automatic disposal is more importantly about stopping the execution of code that is no longer needed. If auto-dispose fails to dispose a provider, then that provider may continuously perform network requests in the background.

Riverpod preferred to not compromise on reliability for the sake of a little convenience.

To alleviate the downsides of having to use ConsumerWidget/ConsumerStatefulWidget instead of StatelessWidget/StatefulWidget, Riverpod offers various refactors in IDEs like VSCode and Android Studio.

To enable them in your IDE, see Getting started

**Examples:**

Example 1 (dart):
```dart
// We subclass StatelessWidget as usualclass MyWidget extends StatelessWidget {  @override  Widget build(BuildContext context) {    // A FutureBuilder-like widget    return Consumer(      // The "builder" callback gives us a "ref" parameter      builder: (context, ref, _) {        // We can use that "ref" to listen to providers        final value = ref.watch(myProvider);        return Text(value.toString());      },    );  }}
```

Example 2 (dart):
```dart
// We subclass ConsumerWidget instead of StatelessWidgetclass MyWidget extends ConsumerWidget {  // "build" receives an extra parameter  @override  Widget build(BuildContext context, WidgetRef ref) {    // We can use that "ref" to listen to providers    final value = ref.watch(myProvider);    return Text(value.toString());  }}
```

Example 3 (dart):
```dart
// We subclass ConsumerStatefulWidget instead of StatefulWidgetclass MyWidget extends ConsumerStatefulWidget {  @override  ConsumerState<MyWidget> createState() => _MyWidgetState();}// We subclass ConsumerState instead of Stateclass _MyWidgetState extends ConsumerState<MyWidget> {  // A "this.ref" property is available  @override  Widget build(BuildContext context) {    // We can use that "ref" to listen to providers    final value = ref.watch(myProvider);    return Text(value.toString());  }}
```

---

## ProviderObservers

**URL:** https://riverpod.dev/docs/concepts2/observers

**Contents:**
- ProviderObservers
- Usage​
- Example: Logger​

A ProviderObserver is an object used to observe provider lifecycle events in the application. They are generally used for logging, analytics, or debugging purposes.

To use a ProviderObserver, you need to extend the class and override the life-cycles you want to observe. There are many methods available. It is recommended to check its documentation for more details.

The following observer logs all state changes of any provider in the application:

Now, every time the value of our provider is updated, the logger will log it:

To improve debugging, you can optionally give your providers a name:

With this change, the log becomes:

When using code-generation, a name is automatically assigned to providers.

If the state of a provider is mutated, (typically Lists, combined with Ref.notifyListeners), it is likely that didUpdateProvider will receive previousValue and newValue as the same value.

This happens because Dart updates objects by "reference". If you want to change this, you will have to clone your objects before mutating them.

**Examples:**

Example 1 (dart):
```dart
// A basic logger, which logs any state changes.final class Logger extends ProviderObserver {  @override  void didUpdateProvider(    ProviderObserverContext context,    Object? previousValue,    Object? newValue,  ) {    print('''{  "provider": "${context.provider}",  "newValue": "$newValue",  "mutation": "${context.mutation}"}''');  }}void main() {  runApp(    ProviderScope(      // ProviderObservers are used by passing them to ProviderScope/ProviderContainer      observers: [        // Adding ProviderScope enables Riverpod for the entire project        // Adding our Logger to the list of observers        Logger(),      ],      child: const MyApp(),    ),  );}// After this, implement a typical Flutter application
```

Example 2 (dart):
```dart
{  "provider": "Provider<int>",  "newValue": "1"}
```

Example 3 (dart):
```dart
final myProvider = Provider<int>((ref) => 0, name: 'MyProvider');
```

Example 4 (dart):
```dart
{  "provider": "MyProvider",  "newValue": "1"}
```

---

## Testing your providers

**URL:** https://riverpod.dev/docs/how_to/testing

**Contents:**
- Testing your providers
- Setting up a test​
  - Unit tests​
  - Widget tests​
- Mocking providers​
- Spying on changes in a provider​
- Awaiting asynchronous providers​
- Mocking Notifiers​

A core part of the Riverpod API is the ability to test your providers in isolation.

For a proper test suite, there are a few challenges to overcome:

Fortunately, Riverpod makes it easy to achieve all of these goals.

When defining a test with Riverpod, there are two main scenarios:

Unit tests are defined using the test function from package:test.

The main difference with any other test is that we will want to create a ProviderContainer object. This object will enable our test to interact with providers.

A typical test using ProviderContainer will look like:

Now that we have a ProviderContainer, we can use it to read providers using:

Be careful when using container.read when providers are automatically disposed. If your provider is not listened to, chances are that its state will get destroyed in the middle of your test.

In that case, consider using container.listen. Its return value enables reading the current value of provider anyway, but will also ensure that the provider is not disposed in the middle of your test:

Widget tests are defined using the testWidgets function from package:flutter_test.

In this case, the main difference with usual Widget tests is that we must add a ProviderScope widget at the root of tester.pumpWidget:

This is similar to what we do when we enable Riverpod in our Flutter app.

Then, we can use tester to interact with our widget. Alternatively if you want to interact with providers, you can obtain a ProviderContainer. One can be obtained using tester.container(). By using tester, we can therefore write the following:

We can then use it to read providers. Here's a full example:

So far, we've seen how to set up a test and basic interactions with providers. However, in some cases, we may want to mock a provider.

The cool part: All providers can be mocked by default, without any additional setup. This is possible by specifying the overrides parameter on either ProviderScope or ProviderContainer.

Consider the following provider:

We can mock it using:

Since we obtained a ProviderContainer in our tests, it is possible to use it to "listen" to a provider:

You can then combine this with packages such as mockito or mocktail to use their verify API. Or more simply, you can add all changes in a list and assert on it.

In Riverpod, it is very common for providers to return a Future/Stream. In that case, chances are that our tests need to await for that asynchronous operation to be completed.

One way to do so is to read the .future of a provider:

It is generally discouraged to mock Notifiers. This is because Notifiers cannot be instantiated on their own, and only work when used as part of a Provider.

Instead, you should likely introduce a level of abstraction in the logic of your Notifier, such that you can mock that abstraction. For instance, rather than mocking a Notifier, you could mock a "repository" that the Notifier uses to fetch data from.

If you insist on mocking a Notifier, there is a special consideration to create such a mock: Your mock must subclass the original Notifier base class: You cannot "implement" Notifier, as this would break the interface.

As such, when mocking a Notifier, instead of writing the following mockito code:

You should instead write:

If using code-generation, for the above to work, your mock will have to be placed in the same file as the Notifier you are mocking. Otherwise you would not have access to the _$MyNotifier class.

Then, to use your notifier you could do:

**Examples:**

Example 1 (dart):
```dart
void main() {  test('Some description', () {    // Create a ProviderContainer for this test.    // DO NOT share ProviderContainers between tests.    final container = ProviderContainer.test();    // TODO: use the container to test your application.    expect(      container.read(provider),      equals('some value'),    );  });}
```

Example 2 (dart):
```dart
final subscription = container.listen<String>(provider, (_, _) {});expect(  // Equivalent to `container.read(provider)`  // But the provider will not be disposed unless "subscription" is disposed.  subscription.read(),  'Some value',);
```

Example 3 (dart):
```dart
void main() {  testWidgets('Some description', (tester) async {    await tester.pumpWidget(      const ProviderScope(child: YourWidgetYouWantToTest()),    );  });}
```

Example 4 (dart):
```dart
final container = tester.container();
```

---

## Provider vs Riverpod

**URL:** https://riverpod.dev/docs/from_provider/provider_vs_riverpod

**Contents:**
- Provider vs Riverpod
- Defining providers​
- Reading providers: BuildContext​
- Reading providers: Consumer​
  - There is no ConsumerN equivalent in Riverpod​
- Combining providers: ProxyProvider with stateless objects​
- Combining providers: ProxyProvider with stateful objects​
- Scoping Providers vs .family + .autoDispose​

This article recaps the differences and the similarities between Provider and Riverpod.

The primary difference between both packages is how "providers" are defined.

With Provider, providers are widgets and as such placed inside the widget tree, typically inside a MultiProvider:

With Riverpod, providers are not widgets. Instead they are plain Dart objects. Similarly, providers are defined outside of the widget tree, and instead are declared as global final variables.

Also, for Riverpod to work, it is necessary to add a ProviderScope widget above the entire application. As such, the equivalent of the Provider example using Riverpod would be:

Notice how the provider definition simply moved up a few lines.

Since with Riverpod providers are plain Dart objects, it is possible to use Riverpod without Flutter. For example, Riverpod can be used to write command line applications.

With Provider, one way of reading providers is to use a Widget's BuildContext.

For example, if a provider was defined as:

then reading it using Provider is done with:

The equivalent in Riverpod would be:

Riverpod's snippet extends ConsumerWidget instead of StatelessWidget. That different widget type adds one extra parameter to our build function: WidgetRef.

Instead of BuildContext.watch, in Riverpod we do WidgetRef.watch, using the WidgetRef which we obtained from ConsumerWidget.

Riverpod does not rely on generic types. Instead it relies on the variable created using provider definition.

Notice too how similar the wording is. Both Provider and Riverpod use the keyword "watch" to describe "this widget should rebuild when the value changes".

Riverpod uses the same terminology as Provider for reading providers.

The rules for context.watch vs context.read applies to Riverpod too: Inside the build method, use "watch". Inside click handlers and other events, use "read". When in need of filtering out values and rebuilds, use "select".

Provider optionally comes with a widget named Consumer (and variants such as Consumer2) for reading providers.

Consumer is helpful as a performance optimization, by allowing more granular rebuilds of the widget tree - updating only the relevant widgets when the state changes:

As such, if a provider was defined as:

Provider allows reading that provider using Consumer with:

Riverpod has the same principle. Riverpod, too, has a widget named Consumer for the exact same purpose.

If we defined a provider as:

Then using Consumer we could do:

Notice how Consumer gives us a WidgetRef object. This is the same object as we saw in the previous part related to ConsumerWidget.

Notice how pkg:Provider's Consumer2, Consumer3 and such aren't needed nor missed in Riverpod.

With Riverpod, if you want to read values from multiple providers, you can simply write multiple ref.watch statements, like so:

When compared to pkg:Provider's ConsumerN APIs, the above solution feels way less heavy and it should be easier to understand.

When using Provider, the official way of combining providers is using the ProxyProvider widget (or variants such as ProxyProvider2).

For example we may define:

From there we have two options. We may combine UserIdNotifier to create a new "stateless" provider (typically an immutable value that possibly override ==). Such as:

This provider would automatically return a new String whenever UserIdNotifier.userId changes.

We can do something similar in Riverpod, but the syntax is different. First, in Riverpod, the definition of our UserIdNotifier would be:

From there, to generate our String based on the userId, we could do:

Notice the line doing ref.watch(userIdNotifierProvider).

This line of code tells Riverpod to obtain the content of the userIdNotifierProvider and that whenever that value changes, labelProvider will be recomputed too. As such, the String emitted by our labelProvider will automatically update whenever the userId changes.

This ref.watch line should feel similar. This pattern was covered previously when explaining how to read providers inside widgets. Indeed, providers are now able to listen to other providers in the same way that widgets do.

When combining providers, another alternative use-case is to expose stateful objects, such as a ChangeNotifier instance.

For that, we could use ChangeNotifierProxyProvider (or variants such as ChangeNotifierProxyProvider2). For example we may define:

Then, we can define a new ChangeNotifier that is based on UserIdNotifier.userId. For example we could do:

This new provider creates a single instance of UserNotifier (which is never re-constructed) and prints a string whenever the user ID changes.

Doing the same thing in provider is achieved differently. First, in Riverpod, the definition of our UserIdNotifier would be:

From there, the equivalent to the previous ChangeNotifierProxyProvider would be:

The core of this snippet is the ref.listen line. This ref.listen function is a utility that allows listening to a provider, and whenever the provider changes, executes a function.

The previous and next parameters of that function correspond to the last value before the provider changed and the new value after it changed.

In pkg:Provider, scoping was used for two things:

Using scoping just to destroy state isn't ideal. The problem is that scoping doesn't work well over large applications. For example, state often is created in one page, but destroyed later in a different page after navigation. This doesn't allow for multiple caches to be active over different pages.

Similarly, the "custom state per page" approach quickly becomes difficult to handle if that state needs to be shared with another part of the widget tree, like you'd need with modals or a with a multi-step form.

Riverpod takes a different approach: first, scoping providers is kind-of discouraged; second, .family and .autoDispose are a complete replacement solution for this.

Within Riverpod, Providers marked as .autoDispose automatically destroy their state when they aren't used anymore. When the last widget removing a provider is unmounted, Riverpod will detect this and destroy the provider. Try using these two lifecycle methods in a provider to test this behavior:

This inherently solves the "destroying state" problem.

Also it is possible to mark a Provider as .family (and, at the same time, as .autoDispose). This enables passing parameters to providers, which make multiple providers to be spawned and tracked internally. In other words, when passing parameters, a unique state is created per unique parameter.

This solves the "custom state per page" problem. Actually, there's another advantage: such state is no-longer bound to one specific page. Instead, if a different page tries to access the same state, such page will be able to do so by just reusing the parameters.

In many ways, passing parameters to providers is equivalent to a Map key. If the key is the same, the value obtained is the same. If it's a different key, a different state will be obtained.

**Examples:**

Example 1 (dart):
```dart
class Counter extends ChangeNotifier { ...}void main() {  runApp(    MultiProvider(      providers: [        ChangeNotifierProvider<Counter>(create: (context) => Counter()),      ],      child: MyApp(),    )  );}
```

Example 2 (dart):
```dart
// Providers are now top-level variablesfinal counterProvider = ChangeNotifierProvider<Counter>((ref) => Counter());void main() {  runApp(    // This widget enables Riverpod for the entire project    ProviderScope(      child: MyApp(),    ),  );}
```

Example 3 (dart):
```dart
Provider<Model>(...);
```

Example 4 (dart):
```dart
class Example extends StatelessWidget {  @override  Widget build(BuildContext context) {    Model model = context.watch<Model>();  }}
```

---

## Provider overrides

**URL:** https://riverpod.dev/docs/concepts2/overrides

**Contents:**
- Provider overrides

In Riverpod, all providers can be overridden to change their behavior. This is useful for testing, debugging, or providing different implementations in different environments, or even Scoping providers.

Overriding a provider is done on ProviderContainers/ProviderScopes, using the overrides parameter. In it, you can specify a list of instructions on how to override a specific provider.

Such "instruction" is created using your provider, combined with value methods named overrideWithSomething. There are a bunch of these methods available, but all of them have their name starting with overrideWith. This includes:

A typical override looks like this:

**Examples:**

Example 1 (dart):
```dart
void main() {  runApp(    ProviderScope(      overrides: [        // Your overrides are defined here.        // The following shows how to override a "counter provider"        // to use a different initial value.        counterProvider.overrideWith((ref) => 42),      ]    )  );}
```

Example 2 (dart):
```dart
final container = ProviderContainer(  overrides: [    // Your overrides are defined here.    // The following shows how to override a "counter provider"    // to use a different initial value.    counterProvider.overrideWith((ref) => 42),  ]);
```

---

## How to reduce provider/widget rebuilds

**URL:** https://riverpod.dev/docs/how_to/select

**Contents:**
- How to reduce provider/widget rebuilds
- Filtering widget/provider rebuild using "select".​
  - Selecting asynchronous properties​

With everything we've seen so far, we can already build a fully functional application. However, you may have questions regarding performance.

In this page, we will cover a few tips and tricks to possibly optimize your code.

Before doing any optimization, make sure to benchmark your application. The added complexity of the optimizations may not be worth minor gains.

You may have noticed that, by default, using ref.watch causes consumers/providers to rebuild whenever any of the properties of an object changes. For instance, watching a User and only using its "name" will still cause the consumer to rebuild if the "age" changes.

But in case you have a consumer using only a subset of the properties, you want to avoid rebuilding the widget when the other properties change.

This can be achieved by using the select functionality of providers. When doing so, ref.watch will no-longer return the full object, but rather the selected properties. And your consumers/providers will now rebuild only if those selected properties change.

It is possible to call select as many times as you wish. You are free to call it once per property you desire.

The selected properties are expected to be immutable. Returning a List and then mutating that list will not trigger a rebuild.

Using select slightly slows down individual read operations and increases the complexity of your code by a tiny bit. It may not be worth using it if those "other properties" rarely change.

In case you are trying to optimize a provider listening to another provider, chances are that other provider is asynchronous.

Normally, you would ref.watch(anotherProvider.future) to get the value. The issue is, select will apply on an AsyncValue – which is not something you can await.

For this purpose, you can instead use selectAsync. It is unique to asynchronous code, and enables performing a select operation on the data emitted by a provider. Its usage is similar to that of select, but returns a Future instead:

**Examples:**

Example 1 (dart):
```dart
class User {  late String firstName, lastName;}final provider = Provider(  (ref) => User()    ..firstName = 'John'    ..lastName = 'Doe',);class ConsumerExample extends ConsumerWidget {  @override  Widget build(BuildContext context, WidgetRef ref) {    // Instead of writing:    // String name = ref.watch(provider).firstName!;    // We can write:    String name = ref.watch(provider.select((it) => it.firstName));    // This will cause the widget to only listen to changes on "firstName".    return Text('Hello $name');  }}
```

Example 2 (dart):
```dart
class User {  late String firstName, lastName;}@riverpodUser example(Ref ref) => User()  ..firstName = 'John'  ..lastName = 'Doe';class ConsumerExample extends ConsumerWidget {  @override  Widget build(BuildContext context, WidgetRef ref) {    // Instead of writing:    // String name = ref.watch(provider).firstName!;    // We can write:    String name = ref.watch(exampleProvider.select((it) => it.firstName));    // This will cause the widget to only listen to changes on "firstName".    return Text('Hello $name');  }}
```

Example 3 (dart):
```dart
final provider = FutureProvider((ref) async {  // Wait for a user to be available, and listen to only the "firstName" property  final firstName = await ref.watch(    userProvider.selectAsync((it) => it.firstName),  );  // TODO use "firstName" to fetch something else});
```

Example 4 (dart):
```dart
@riverpodObject? example(Ref ref) async {  // Wait for a user to be available, and listen to only the "firstName" property  final firstName = await ref.watch(    userProvider.selectAsync((it) => it.firstName),  );  // TODO use "firstName" to fetch something else}
```

---

## Family

**URL:** https://riverpod.dev/docs/concepts2/family

**Contents:**
- Family
- Creating a Family​
- Using a Family​
- Overriding families​

One of Riverpod's most powerful feature is called "Families". In short, it allows a provider to be associated with multiple independent states, based on a unique parameter combination.

A typical use-case is to fetch data from a remote API, where the response depends on some parameters (such as a user ID or a search query or a page number). It enables defining a single provider that can be used to fetch and cache any possible parameter combination.

If normal providers can be assimilated to a variable, then "family" providers can be assimilated to a Map.

Defining a family is done by slightly modifying the provider definition to receive a parameter.

For functional providers, the syntax is as follows:

And for notifier providers, the syntax is:

Although not strictly required, it is highly advised to enable Automatic disposal when using families.

This avoids memory leaks in case the parameter changes and the previous state is no longer needed.

Providers that receive parameters see their usage slightly modified too.

Long story short, you need to pass the parameters that your provider expects, as follows:

Parameters passed need to have a consistent ==/hashCode.

View "family" as a Map, where the parameters are the key and the provider's state is the value. As such, if the ==/hashCode of a parameter changes, the value obtained will be different.

Therefore code such as the following is incorrect:

To help spot this mistake, it is recommended to use the riverpod_lint and enable the provider_parameters lint rule. Then, the previous snippet would show a warning. See Getting started for installation steps.

You can read as many "family" providers as you want, and they will all be independent. As such, it is legal to do:

When trying to mock a provider in tests, you may want to override a family provider.

In that scenario, you have two options:

**Examples:**

Example 1 (dart):
```dart
// When not using code-generation, providers can use ".family".// This adds one generic parameter corresponding to the type of the parameter.// The initialization function then receives the parameter.final userProvider = FutureProvider.autoDispose.family<User, String>((ref, id) async {  final dio = Dio();  final response = await dio.get('https://api.example.com/users/$id');  return User.fromJson(response.data);});
```

Example 2 (dart):
```dart
@riverpodFuture<User> user(  Ref ref,  // When using code-generation, providers can receive any number of parameters.  // They can be both positional/named and required/optional.  String id,) async {  final dio = Dio();  final response = await dio.get('https://api.example.com/users/$id');  return User.fromJson(response.data);}
```

Example 3 (dart):
```dart
// With notifiers providers, we also use ".family" and receive and extra// generic argument.// The main difference is that the associated Notifier needs to define// a constructor+field to accept the argument.final userProvider = AsyncNotifierProvider.autoDispose.family<UserNotifier, User, String>(  UserNotifier.new,);class UserNotifier extends AsyncNotifier<User> {  // We store the argument in a field, so that we can use it  UserNotifier(this.id);  final String id;  @override  Future<User> build() async {    final dio = Dio();    final response = await dio.get('https://api.example.com/users/$id');    return User.fromJson(response.data);  }}
```

Example 4 (dart):
```dart
@riverpodclass UserNotifier extends _$UserNotifier {  @override  Future<User> build(    // When using code-generation, Notifiers can define parameters on their    // "build" method. Any number of parameter can be defined.    String id,  ) async {    final dio = Dio();    final response = await dio.get('https://api.example.com/users/$id');    // The generated class will naturally have access to the parameters    // passed to the "build" method in "this":    print(this.id);    return User.fromJson(response.data);  }}
```

---

## Scoping providers

**URL:** https://riverpod.dev/docs/concepts2/scoping

**Contents:**
- Scoping providers
- Defining a scoped provider​
- Listening to a scoped provider​
- Setting the value of a scoped provider​

Scoping is the act of changing the behavior of a provider for only a small part of your application.

Scoping is achieved using Provider overrides, by overriding a provider in ProviderContainers/ProviderScopes that are not the root of your application.

The scoping feature is highly complex and will likely be reworked in the future to be more ergonomic.

By default, Riverpod will not allow you to scope a provider. You need to opt-in to this feature by specifying dependencies on the provider.

The first scoped provider in your app will typically specify dependencies: []. The following snippet defines a scoped provider that exposes the current item ID that is being displayed:

To listen to a scoped provider, use the provider as usual by obtaining Refs, (such as with Consumers).

If a provider is listening to a scoped provider, that scoped provider needs to be included in the dependencies of the provider that is listening to it:

Inside dependencies, you only need to list scoped providers. You do not need to list providers that are not scoped.

To set a scoped provider, you can use Provider overrides. A typical example is to specify overrides on ProviderScope like so:

**Examples:**

Example 1 (dart):
```dart
final currentItemIdProvider = Provider<String?>(  dependencies: const [],  (ref) => null,);
```

Example 2 (dart):
```dart
@Riverpod(dependencies: [])String? currentItemId(Ref ref) => null;
```

Example 3 (dart):
```dart
final currentItemId = ref.watch(currentItemIdProvider);
```

Example 4 (dart):
```dart
final currentItemProvider = FutureProvider<Item?>(  dependencies: [currentItemIdProvider],  (ref) async {    final currentItemId = ref.watch(currentItemIdProvider);    if (currentItemId == null) return null;    // Fetch the item from a database or API    return fetchItem(id: currentItemId);  },);
```

---

## ProviderContainers/ProviderScopes

**URL:** https://riverpod.dev/docs/concepts2/containers

**Contents:**
- ProviderContainers/ProviderScopes
  - Using a ProviderContainer for pure Dart applications​
  - Using a ProviderScope for Flutter applications​
- Why store the state of providers inside a container?​

ProviderContainer is the central piece of Riverpod's architecture. In Riverpod, Providers hold no state themselves. Instead, the state of a given provider is stored inside this container object.

ProviderScope is a widget that creates a ProviderContainer and exposes it to the widget tree. Hence why, when you use Riverpod, you will always see a scope at the root of apps. Without it, Riverpod would be unable to store the state of providers!

ProviderContainer is a useful object when you want to use Riverpod in pure Dart codebases, such as command-line applications or server-side applications.

You can create a ProviderContainer inside your main, and use it to read and modify providers:

Inside tests, do not use ProviderContainer directly. Use ProviderContainer.test instead. This will automatically dispose the container when the test ends.

In Flutter applications, you shouldn't use ProviderContainer directly. Instead, you should use ProviderScope, which is a widget equivalent of ProviderContainer.

The end-result is the same: Create a ProviderScope in your main. After that, you can use Consumers to read and modify providers in your widgets.

One might wonder why providers don't store their state themselves. If we got rid of that requirement, we could imagine a world where we could write:

instead of having to write ref.watch(helloWorldProvider).

Riverpod does this for a few reasons, which come down the the same logic: "No global state".

Better separation of concerns. If Riverpod were to allow providers to store their own state, it would imply that anything could read/write to that state. This means that it would be difficult to control how/when a state is modified.

Using Riverpod's architecture, state updates are centralized: All the logic for modifying a provider is done in the provider itself. And generally, the UI will only invoke one method on the provider's Notifier.

Better testing. By storing the state of providers inside a container, we do not have to worry about resetting the application state between tests. We can simply create a new container for each test, and a fresh state will be created for each provider:

Here, we can see that both tests rely on the same provider. Yet state changes inside one test do not affect the other test.

Of course, the same applies when using ProviderScope and widget tests.

A centralized place for configuring your application. Through ProviderContainer and ProviderScope, we can configure various app-wide aspects of Riverpod. For example:

Support for Scoping providers. By storing the state of a provider inside a container, we can have the same provider resolve to a different state depending on where in the widget tree it is used. This feature is quite advanced and generally discouraged, but useful for performance optimizations.

**Examples:**

Example 1 (dart):
```dart
import 'package:riverpod/riverpod.dart';void main() {  final container = ProviderContainer();  try {    final sub = container.listen(counterProvider, (previous, next) {      print('Counter changed from $previous to $next');    });    print('Counter starts at ${sub.read()}');  } finally {    // Dispose the container when done    container.dispose();  }}
```

Example 2 (dart):
```dart
test('Counter starts at 0 and can be incremented', () {  // No need to dispose the container when the test ends  final container = ProviderContainer.test();  // Use the container to test your providers});
```

Example 3 (dart):
```dart
import 'package:flutter/material.dart';import 'package:hooks_riverpod/hooks_riverpod.dart';void main() {  runApp(    ProviderScope(      child: Consumer(        builder: (context, ref, _) {          final counter = ref.watch(counterProvider);          // TODO use "counter"        },      ),    ),  );}
```

Example 4 (dart):
```dart
print(helloWorldProvider.value); // Prints "Hello world!"
```

---

## Quickstart

**URL:** https://riverpod.dev/docs/from_provider/quickstart

**Contents:**
- Quickstart
- Start with ChangeNotifierProvider​
- Start with leaves​
- Riverpod and Provider can coexist​
- Migrate one Provider at a time​
- Migrating ProxyProviders​
- Eager initialization​
- Code Generation​

This section is designed for people familiar with the Provider package who wants to learn about Riverpod.

Before anything, read the short getting started article and try out the small sandbox example to test Riverpod's features out. If you like what you see there, you should then definitively consider a migration.

Indeed, migrating from Provider to Riverpod can be very straightforward.

Migrating basically consists in a few steps that can be done in an incremental way.

It's fine to keep using ChangeNotifier while transitioning towards Riverpod, and not use its latest fancy features ASAP.

Indeed, the following is perfectly fine to start with:

As you can see Riverpod exposes a ChangeNotifierProvider class, which is there precisely to support migrations from pkg:Provider.

Keep in mind that this provider is not recommended when writing new code, and it is not the best way to use Riverpod, but it's a gentle and very easy way to start your migration.

There is no rush to immediately try to change your ChangeNotifiers into the more modern Notifiers. Some require a bit of a paradigm shift, so it may be difficult to do initially.

Take your time, as it is important to get yourself familiar with Riverpod first; you'll quickly find out that almost all Providers from pkg:provider have a strict equivalent in pkg:riverpod.

Start with Providers that do not depend on anything else, i.e. start with the leaves in your dependency tree. Once you have migrated all of the leaves, you can then move on to the providers that depend on leaves.

In other words, avoid migrating ProxyProviders at first; tackle them once all of their dependencies have been migrated.

This should boost and simplify the migration process, while also minimizing / tracking down any errors.

Keep in mind that it is entirely possible to use both Provider and Riverpod at the same time.

Indeed, using import aliases, it is possible to use the two APIs altogether. This is also great for readability and it removes any ambiguous API usage.

If you plan on doing this, consider using import aliases for each Provider import in your codebase.

A full guide onto how to effectively implement import aliases is incoming soon.

If you have an existing app, don't try to migrate all your providers at once!

While you should strive toward moving all your application to Riverpod in the long-run, don't burn yourself out. Do it one provider at a time.

Take the above example. Fully migrating that myNotifierProvider to Riverpod means writing the following:

.. and it's also needed to change how that provider is consumed, i.e. writing ref.watch in the place of each context.watch for this provider.

This operation might take some time and might lead to some errors, so don't rush doing this all at once.

Within pkg:Provider, ProxyProvider is used to combine values from other Providers; its build depends on the value of other providers, reactively.

With Riverpod, instead, Providers are composable by default; therefore, when migrating a ProxyProvider you'll simply need to write ref.watch if you want to declare a direct dependency from a Provider to another.

If anything, combining values with Riverpod should feel simpler and straightforward; thus, the migration should greatly simplify your code.

Furthermore, there are no shenanigans about combining more than two providers together: just add another ref.watch and you'll be good to go.

Since Notifiers are final global variables, they are lazy by default.

If you need to initialize some warm-up data or a useful service on startup, the best way to do it is to first read your provider in the place where you used to put MultiProvider.

In other words, since Riverpod can't be forced to be eager initialized, they can be read and cached in your startup phase, so that they're warm and ready when needed inside the rest of your application.

A full guide about eager initialization of pkg:Notifiers is available here.

Code generation is recommended to use Riverpod the future-proof way. As a side note, chances are that when metaprogramming will be a thing, codegen will be default for Riverpod.

Unluckily, @riverpod can't generate code for ChangeNotifierProvider. To overcome this, you can use the following utility extension method:

And then, you can expose your ChangeNotifier with the following codegen syntax:

Once the "base" migration is done, you can change your ChangeNotifier to Notifier, thus eliminating the need for temporary extensions. Taking up the previous examples, a "fully migrated" Notifier becomes:

Once this is done, and you're positive that there are no more ChangeNotifierProviders in your codebase, you can get rid of the temporary extension definitively.

Keep in mind that, while being recommended, codegen is not mandatory. It's good to reason about migrations incrementally: if you feel like that implementing this migration while transitioning to the code generation syntax in one single take might be too much, that's fine.

Following this guide, you can migrate towards codegen as a further step forward, later on.

**Examples:**

Example 1 (dart):
```dart
// If you have this...class MyNotifier extends ChangeNotifier {  int state = 0;  void increment() {    state++;    notifyListeners();  }}// ... just add this!final myNotifierProvider = ChangeNotifierProvider<MyNotifier>((ref) {  return MyNotifier();});
```

Example 2 (dart):
```dart
class MyNotifier extends Notifier<int> {  @override  int build() => 0;  void increment() => state++;}final myNotifierProvider = NotifierProvider<MyNotifier, int>(MyNotifier.new);
```

Example 3 (dart):
```dart
extension ChangeNotifierWithCodeGenExtension on Ref {  T listenAndDisposeChangeNotifier<T extends ChangeNotifier>(T notifier) {    notifier.addListener(notifyListeners);    onDispose(() => notifier.removeListener(notifyListeners));    onDispose(notifier.dispose);    return notifier;  }}
```

Example 4 (dart):
```dart
// ignore_for_file: unsupported_provider_value@riverpodMyNotifier example(Ref ref) {  return ref.listenAndDisposeChangeNotifier(MyNotifier());}
```

---
