# Riverpod-V3 - Migration

**Pages:** 6

---

## ^0.14.0 to ^1.0.0

**URL:** https://riverpod.dev/docs/migration/0.14.0_to_1.0.0

**Contents:**
- ^0.14.0 to ^1.0.0
- Using the migration tool to automatically upgrade your project to the new syntax​
  - Installing the command line tool​
  - Usage​
- The changes​
  - Syntax unification​
  - StateProvider update​

After a long wait, the first stable version of Riverpod is finally released 👏

To see the full list of changes, consult the Changelog. In this page, we will focus on how to migrate an existing Riverpod application from version 0.14.x to version 1.0.0.

Before explaining the various changes, it is worth noting that Riverpod comes with a command-line tool to automatically migrate your project for you.

To install the migration tool, run:

You should now be able to run:

Now that the command line is installed, we can start using it.

First, open the project you want to migrate in your terminal.

Do not upgrade Riverpod. The migration tool will upgrade the version of Riverpod for you.

Not upgrading Riverpod is important. The tool will not execute properly if you have already installed version 1.0.0. As such, make sure that you are properly using an older version before starting the tool.

Make sure that your project does not contain errors.

The tool will then analyze your project and suggest changes. For example you may see:

To accept the change, simply press y. Otherwise to reject it, press n.

Now that we've seen how to use the CLI to automatically upgrade your project, let's see in detail the changes necessary.

Version 1.0.0 of Riverpod focused on the unification of the syntax for interacting with providers. Before, Riverpod had many similar yet different syntaxes for reading a provider, such as ref.watch(provider) vs useProvider(provider) vs watch(provider). With version 1.0.0, only one syntax remains: ref.watch(provider). The others were removed.

useProvider is removed in favor of HookConsumerWidget.

The prototype of ConsumerWidget's build and Consumer's builder changed.

context.read is removed in favor of ref.read.

StateProvider was updated to match StateNotifierProvider.

Before, doing ref.watch(StateProvider) returned a StateController instance. Now it only returns the state of the StateController.

To migrate you have a few solutions. If your code only obtained the state without modifying it, you can change from:

Alternatively you can use the new StateProvider.state to keep the old behavior.

**Examples:**

Example 1 (unknown):
```unknown
dart pub global activate riverpod_cli
```

Example 2 (unknown):
```unknown
riverpod --help
```

Example 3 (unknown):
```unknown
riverpod migrate
```

Example 4 (unknown):
```unknown
-Widget build(BuildContext context, ScopedReader watch) {+Widget build(BuildContext context, Widget ref) {-  MyModel state = watch(provider);+  MyModel state = ref.watch(provider);}Accept change (y = yes, n = no [default], A = yes to all, q = quit)?
```

---

## ^0.13.0 to ^0.14.0

**URL:** https://riverpod.dev/docs/migration/0.13.0_to_0.14.0

**Contents:**
- ^0.13.0 to ^0.14.0
- The changes​
- Using the migration tool to automatically upgrade your projects to the new syntax​
  - Installing the command line​
  - Usage​

With the release of version 0.14.0 of Riverpod, the syntax for using StateNotifierProvider changed (see https://github.com/rrousselGit/riverpod/issues/341 for the explanation).

For the entire article, consider the following StateNotifier:

StateNotifierProvider takes an extra generic parameter, which should be the type of the state of your StateNotifier.

to obtain the StateNotifier, you should now read myProvider.notifier instead of just myProvider:

to listen to the state of the StateNotifier, you should now read myProvider instead of myProvider.state:

With version 0.14.0 came the release of a command line tool for Riverpod, which can help you migrate your projects.

To install the migration tool, run:

You should now be able to run:

Now that the command line is installed, we can start using it.

The tool will then analyze your project and suggest changes. For example you may see:

To accept the change, simply press y. Otherwise to reject it, press n.

**Examples:**

Example 1 (dart):
```dart
class MyModel {}class MyStateNotifier extends StateNotifier<MyModel> {  MyStateNotifier(): super(MyModel());}
```

Example 2 (dart):
```dart
final provider = StateNotifierProvider<MyStateNotifier>((ref) {  return MyStateNotifier();});
```

Example 3 (dart):
```dart
final provider = StateNotifierProvider<MyStateNotifier, MyModel>((ref) {  return MyStateNotifier();});
```

Example 4 (dart):
```dart
Widget build(BuildContext context, ScopedReader watch) {  MyStateNotifier notifier = watch(provider);}
```

---

## From `ChangeNotifier`

**URL:** https://riverpod.dev/docs/migration/from_change_notifier

**Contents:**
- From `ChangeNotifier`
- Start your migration​
  - Initialization​
  - Mutations and Side Effects​
- Migration Process Summary​

Within Riverpod, ChangeNotifierProvider is meant to be used to offer a smooth transition from pkg:provider.

If you've just started a migration to pkg:riverpod, make sure you read the dedicated guide (see Quickstart). This article is meant for folks that already transitioned to riverpod, but want to move away from ChangeNotifier.

All in all, migrating from ChangeNotifier to AsyncNotifier requires a paradigm shift, but it brings great simplification with the resulting migrated code.

Take this (faulty) example:

This implementation shows several weak design choices such as:

Note how this example has been crafted to show how ChangeNotifier can lead to faulty design choices for newbie developers; also, another takeaway is that mutable state might be way harder than it initially promises.

Notifier/AsyncNotifier, in combination with immutable state, can lead to better design choices and less errors.

Let's see how to migrate the above snippet, one step at a time, towards the newest APIs.

First, we should declare the new provider / notifier: this requires some thought process which depends on your unique business logic.

Let's summarize the above requirements:

The above thought process boils down to answering the following questions:

If you're using codegen, the above thought process is enough. There's no need to think about the right class names and their specific APIs. @riverpod only asks you to write a class with its return type, and you're good to go.

Technically, the best fit here is to define a AsyncNotifier<List<Todo>>, which meets all the above requirements. Let's write some pseudocode first.

Remember: use snippets in your IDE to get some guidance, or just to speed up your code writing. See Getting started.

With respect with ChangeNotifier's implementation, we don't need to declare todos anymore; such variable is state, which is implicitly loaded with build.

Indeed, riverpod's notifiers can expose one entity at a time.

Riverpod's API is meant to be granular; nonetheless, when migrating, you can still define a custom entity to hold multiple values. Consider using Dart 3's records to smooth out the migration at first.

Initializing a notifier is easy: just write initialization logic inside build. We can now get rid of the old _init function.

With respect of the old _init, the new build isn't missing anything: there is no need to initialize variables such as isLoading or hasError anymore.

Riverpod will automatically translate any asynchronous provider, via exposing an AsyncValue<List<Todo>> and handles the intricacies of asynchronous state way better than what two simple boolean flags can do.

Indeed, any AsyncNotifier effectively makes writing additional try/catch/finally an anti-pattern for handling asynchronous state.

Just like initialization, when performing side effects there's no need to manipulate boolean flags such as hasError, or to write additional try/catch/finally blocks.

Below, we've cut down all the boilerplate and successfully fully migrated the above example:

Syntax and design choices may vary, but in the end we just need to write our request and update state afterwards. See Providers.

Let's review the whole migration process applied above, from a operational point of view.

**Examples:**

Example 1 (dart):
```dart
class MyChangeNotifier extends ChangeNotifier {  MyChangeNotifier() {    _init();  }  List<Todo> todos = [];  bool isLoading = true;  bool hasError = false;  Future<void> _init() async {    try {      final json = await http.get('api/todos');      todos = [...json.map(Todo.fromJson)];    } on Exception {      hasError = true;    } finally {      isLoading = false;      notifyListeners();    }  }  Future<void> addTodo(int id) async {    isLoading = true;    notifyListeners();    try {      final json = await http.post('api/todos');      todos = [...json.map(Todo.fromJson)];      hasError = false;    } on Exception {      hasError = true;    } finally {      isLoading = false;      notifyListeners();    }  }}final myChangeProvider = ChangeNotifierProvider<MyChangeNotifier>((ref) {  return MyChangeNotifier();});
```

Example 2 (dart):
```dart
class MyNotifier extends AsyncNotifier<List<Todo>> {  @override  FutureOr<List<Todo>> build() {    // TODO ...    return [];  }  Future<void> addTodo(Todo todo) async {    // TODO  }}final myNotifierProvider =    AsyncNotifierProvider.autoDispose<MyNotifier, List<Todo>>(MyNotifier.new);
```

Example 3 (dart):
```dart
@riverpodclass MyNotifier extends _$MyNotifier {  @override  FutureOr<List<Todo>> build() {    // TODO ...    return [];  }  Future<void> addTodo(Todo todo) async {    // TODO  }}
```

Example 4 (dart):
```dart
class MyNotifier extends AsyncNotifier<List<Todo>> {  @override  FutureOr<List<Todo>> build() async {    final json = await http.get('api/todos');    return [...json.map(Todo.fromJson)];  }}final myNotifierProvider =    AsyncNotifierProvider.autoDispose<MyNotifier, List<Todo>>(MyNotifier.new);
```

---

## Migrating from 2.0 to 3.0

**URL:** https://riverpod.dev/docs/3.0_migration

**Contents:**
- Migrating from 2.0 to 3.0
- Automatic retry​
- Out of view providers are paused​
- StateProvider, StateNotifierProvider, and ChangeNotifierProvider are moved to a new import​
- Providers now all use == to filter updates​
- ProviderObserver has its interface slightly changed​
- Simplified Ref and removed Ref subclasses​
- AutoDispose interfaces are removed.​
- The family variant of Notifiers is removed​
- Provider failures are now rethrown as ProviderExceptions​

For the list of changes, please refer to the What's new in Riverpod 3.0 page.

Riverpod 3.0 introduces a number of breaking changes that may require you to update your code. They should in general be relatively minor, but we recommend you to read this page carefully.

This migration is supposed to be smooth. If there is anything that is unclear, or if you encountered a scenario that is difficult to migrate, please open an issue.

It is important to us that the migration is as smooth as possible, so we will do our best to help you, improve the migration guide, or even include helpers to make the migration easier.

Riverpod 3.0 now automatically retries failing providers by default. This means that if a provider fails to compute its value, it will automatically retry until it succeeds.

In general, this is a good thing as it makes your app more resilient to transient errors. However, you may want to disable/customize this behavior in some cases.

To disable automatic retry globally, you can do so on ProviderContainer/ProviderScope:

Alternatively, you can disable automatic retry on a per-provider basis by using the retry parameter of the provider:

In Riverpod 3.0, out of view providers are paused by default.

There is currently no way to disable this behavior globally, but you can control the pause behavior at the consumer level by using the TickerMode widget.

In Riverpod 3.0, StateProvider, StateNotifierProvider, and ChangeNotifierProvider are considered "legacy". They are not removed, but are no longer part of the main API. This is to discourage their use in favor of the new Notifier API.

To keep using them, you need to change your imports to one of the following:

Before, Riverpod was inconsistent in how it filtered updates to providers. Some providers used == to filter updates, while others used identical. In Riverpod 3.0, all providers now use == to filter updates.

The most likely way for you to be impacted by this change is when using StreamProvider/StreamNotifier, as now stream values will be filtered by ==. If you need to, you can override Notifier.updateShouldNotify to customize the behavior.

In the scenario where you didn't use a Notifier, you can refactor your provider in its notifier equivalent (Such as converting StreamProvider to StreamNotifierProvider).

For the sake of mutations, the ProviderObserver interface has changed slightly.

Instead of two separate parameters for ProviderContainer and ProviderBase, a single ProviderObserverContext object is passed. This object contains both the container, provider, and extra information (such as the associated mutation).

To migrate, you need to update all methods of your observers like so:

For the sake of simplification, Ref has lost its type parameter, and all properties/methods that were using the type parameter have been moved to Notifiers.

Specifically, ProviderRef.state, Ref.listenSelf and FutureProviderRef.future should be replaced by Notifier.state, Notifier.listenSelf and AsyncNotifier.future respectively.

Similarly, all Ref subclasses are removed (such as but not limited to ProviderRef, FutureProviderRef, etc).

This primarily affects code-generation. Instead of MyProviderRef, you can now use Ref directly:

The auto-dispose feature is simplified. Instead of relying on a clone of all interfaces, interfaces are unified. In short, instead of AutoDisposeProvider, AutoDisposeNotifier, etc, you now have Provider, Notifier, etc. The behavior is the same, but the API is simplified.

To easily migrate, you can do a case-sensitive replace of AutoDispose to (empty string).

In the same vein as the previous point, the family variant of Notifiers has been removed. Now, we only use Notifier/AsyncNotifier/StreamNotifier, and FamilyNotifier/... have been removed.

To migrate, you will need to replace:

Then, you will need to:

An example of this migration is as follows:

In Riverpod 3.0, all provider failures are rethrown as ProviderExceptions. This means that if a provider fails to compute its value, reading it will throw a ProviderException instead of the original error.

This can impact you if you were relying on the original error type to handle specific errors. To migrate, you can catch the ProviderException and extract the original error from it:

This is only necessary if you were explicitly relying on try/catch to handle such error.

If you are using [AsyncValue] to check for errors, you don't need to change anything:

**Examples:**

Example 1 (dart):
```dart
void main() {  runApp(    ProviderScope(      // Never retry any provider      retry: (retryCount, error) => null,      child: MyApp(),    ),  );}
```

Example 2 (dart):
```dart
void main() {  final container = ProviderContainer(    // Never retry any provider    retry: (retryCount, error) => null,  );}
```

Example 3 (dart):
```dart
final todoListProvider = NotifierProvider<TodoList, List<Todo>>(  TodoList.new,  // Never retry this specific provider  retry: (retryCount, error) => null,);
```

Example 4 (dart):
```dart
// Never retry this specific providerDuration? retry(int retryCount, Object error) => null;@Riverpod(retry: retry)class TodoList extends _$TodoList {  @override  List<Todo> build() => [];}
```

---

## From `StateNotifier`

**URL:** https://riverpod.dev/docs/migration/from_state_notifier

**Contents:**
- From `StateNotifier`
- New syntax comparison​
- Migrating asynchronous StateNotifiers​
  - Advantages​
- Explicit .family and .autoDispose modifications​
- Lifecycles have a different behavior​
  - Old dispose vs ref.onDispose​
  - No more mounted​
- Mutations APIs are the same as before​
- Other migrations​

Along with Riverpod 2.0, new classes were introduced: Notifier / AsyncNotifier. StateNotifier is now discouraged in favor of those new APIs.

This page shows how to migrate from the deprecated StateNotifier to the new APIs.

The main benefit introduced by AsyncNotifier is a better async support; indeed, AsyncNotifier can be thought as a FutureProvider which can expose ways to be modified from the UI.

Furthermore, the new (Async)Notifiers:

Let's see how to define a Notifier, how it compares with StateNotifier and how to migrate the new AsyncNotifier for asynchronous state.

Be sure to know how to define a Notifier before diving into this comparison. See Providers.

Let's write an example, using the old StateNotifier syntax:

Here's the same example, built with the new Notifier APIs, which roughly translates to:

Comparing Notifier with StateNotifier, one can observe these main differences:

Similar conclusions can be made with AsyncNotifier, Notifier's asynchronous equivalent.

The main appeal of the new API syntax is an improved DX on asynchronous data. Take the following example:

Here's the above example, rewritten with the new AsyncNotifier APIs:

AsyncNotifier, just like Notifier, brings a simpler and more uniform API. Here, it's easy to see AsyncNotifier as a FutureProvider with methods.

AsyncNotifier comes with a set of utilities and getters that StateNotifier doesn't have, such as e.g. future and update. This enables us to write much simpler logic when handling asynchronous mutations and side-effects. See also Providers.

Migrating from StateNotifier<AsyncValue<T>> to a AsyncNotifier<T> boils down to:

After these few examples, let's now highlight the main advantages of Notifier and AsyncNotifier:

Let's go further down and highlight more differences and similarities.

Another important difference is how families and auto dispose is handled with the new APIs.

Modifications are explicitly stated inside the class; any parameters are directly injected in the build method, so that they're available to the initialization logic. This should bring better readability, more conciseness and overall less mistakes.

Take the following example, in which a StateNotifierProvider.family is being defined.

BugsEncounteredNotifier feels... heavy / hard to read. Let's take a look at its migrated AsyncNotifier counterpart:

Its migrated counterpart should feel like a light read.

(Async)Notifier's .family parameters are available via this.arg (or this.paramName when using codegen)

Lifecycles between Notifier/AsyncNotifier and StateNotifier differ substantially.

This example showcases - again - how the old API have sparse logic:

Here, if durationProvider updates, MyNotifier disposes: its instance is then re-instantiated and its internal state is then re-initialized. Furthermore, unlike every other provider, the dispose callback is to be defined in the class, separately. Finally, it is still possible to write ref.onDispose in its provider, showing once again how sparse the logic can be with this API; potentially, the developer might have to look into eight (8!) different places to understand this Notifier behavior!

These ambiguities are solved with Riverpod 2.0.

StateNotifier's dispose method refers to the dispose event of the notifier itself, aka it's a callback that gets called before disposing of itself.

(Async)Notifiers don't have this property, since they don't get disposed of on rebuild; only their internal state is. In the new notifiers, dispose lifecycles are taken care of in only one place, via ref.onDispose (and others), just like any other provider. This simplifies the API, and hopefully the DX, so that there is only one place to look at to understand lifecycle side-effects: its build method.

Shortly: to register a callback that fires before its internal state rebuilds, we can use ref.onDispose like every other provider.

You can migrate the above snippet like so:

In this last snippet there sure is some simplification, but there's still an open problem: we are now unable to understand whether or not our notifiers are still alive while performing update. This might arise an unwanted StateErrors.

This happens because (Async)Notifiers lacks a mounted property, which was available on StateNotifier. Considering their difference in lifecycle, this makes perfect sense; while possible, a mounted property would be misleading on the new notifiers: mounted would almost always be true.

While it would be possible to craft a custom workaround, it's recommended to work around this by canceling the asynchronous operation.

Canceling an operation can be done with a custom Completer, or any custom derivative.

For example, if you're using Dio to perform network requests, consider using a cancel token (see also Automatic disposal).

Therefore, the above example migrates to the following:

Up until now we've shown the differences between StateNotifier and the new APIs. Instead, one thing Notifier, AsyncNotifier and StateNotifier share is how their states can be consumed and mutated.

Consumers can obtain data from these three providers with the same syntax, which is great in case you're migrating away from StateNotifier; this applies for notifiers methods, too.

Let's explore the less-impactful differences between StateNotifier and Notifier (or AsyncNotifier)

StateNotifier's .addListener and .stream can be used to listen for state changes. These two APIs are now to be considered outdated.

This is intentional due to the desire to reach full API uniformity with Notifier, AsyncNotifier and other providers. Indeed, using a Notifier or an AsyncNotifier shouldn't be any different from any other provider.

In a nutshell: if you want to listen to a Notifier/AsyncNotifier, just use ref.listen. See Refs.

StateNotifier exposes .debugState: this property is used for pkg:state_notifier users to enable state access from outside the class when in development mode, for testing purposes.

If you're using .debugState to access state in tests, chances are that you need to drop this approach.

Notifier / AsyncNotifier don't have a .debugState; instead, they directly expose .state, which is @visibleForTesting.

AVOID accessing .state from tests; if you have to, do it if and only if you had already have a Notifier / AsyncNotifier properly instantiated; then, you could access .state inside tests freely.

Indeed, Notifier / AsyncNotifier should not be instantiated by hand; instead, they should be interacted with by using its provider: failing to do so will break the notifier, due to ref and family args not being initialized.

Don't have a Notifier instance? No problem, you can obtain one with ref.read, just like you would read its exposed state:

Learn more about testing in its dedicated guide. See Testing your providers.

StateProvider was exposed by Riverpod since its release, and it was made to save a few LoC for simplified versions of StateNotifierProvider. Since StateNotifierProvider is deprecated, StateProvider is to be avoided, too. Furthermore, as of now, there is no StateProvider equivalent for the new APIs.

Nonetheless, migrating from StateProvider to Notifier is simple.

Even though it costs us a few more LoC, migrating away from StateProvider enables us to archive StateNotifier.

**Examples:**

Example 1 (dart):
```dart
class CounterNotifier extends StateNotifier<int> {  CounterNotifier() : super(0);  void increment() => state++;  void decrement() => state--;}final counterNotifierProvider =    StateNotifierProvider<CounterNotifier, int>((ref) {  return CounterNotifier();});
```

Example 2 (dart):
```dart
class CounterNotifier extends Notifier<int> {  @override  int build() => 0;  void increment() => state++;  void decrement() => state++;}final counterNotifierProvider = NotifierProvider<CounterNotifier, int>(CounterNotifier.new);
```

Example 3 (dart):
```dart
@riverpodclass CounterNotifier extends _$CounterNotifier {  @override  int build() => 0;  void increment() => state++;  void decrement() => state--;}
```

Example 4 (dart):
```dart
class AsyncTodosNotifier extends StateNotifier<AsyncValue<List<Todo>>> {  AsyncTodosNotifier() : super(const AsyncLoading()) {    _postInit();  }  Future<void> _postInit() async {    state = await AsyncValue.guard(() async {      final json = await http.get('api/todos');      return [...json.map(Todo.fromJson)];    });  }  // ...}
```

---

## What's new in Riverpod 3.0

**URL:** https://riverpod.dev/docs/whats_new

**Contents:**
- What's new in Riverpod 3.0
- Offline persistence (experimental)​
- Mutations (experimental)​
- Automatic retry​
- Ref.mounted​
- Generic support (code-generation)​
- Pause/Resume support​
- Unification of the Public APIs​
  - [StateProvider]/[StateNotifierProvider] and [ChangeNotifierProvider] are discouraged and moved to a different import​
  - AutoDispose interfaces are removed​

Welcome to Riverpod 3.0! This update includes many long-due features, bug fixes, and simplifications of the API.

This version is a transition period toward a simpler, unified Riverpod.

This version contains a few life-cycle changes. Those could break your app in subtle ways. Upgrade carefully. For the migration guide, please refer to the migration page.

Some of the key highlights include:

This feature is experimental and not yet stable. It is usable, but the API may change in breaking ways without a major version bump.

Offline persistence is a new feature that enables caching a provider locally on the device. Then, when the application is closed and reopened, the provider can be restored from the cache. Offline persistence is opt-in, and supported by all "Notifier" providers, and regardless of if you use code generation or not.

Riverpod only includes interfaces to interact with a database. It does not include a database itself. You can use any database you want, as long as it implements the interfaces. An official package for SQLite is maintained: riverpod_sqflite.

As a short demo, here's how you can use offline persistence:

This feature is experimental and not yet stable. It is usable, but the API may change in breaking ways without a major version bump.

A new feature called "mutations" is introduced in Riverpod 3.0. This feature solves two problems:

The TL;DR is, a new Mutation object is added. It is declared as a top-level final variable, like providers:

After that, your UI can use ref.listen/ref.watch to listen to the state of mutations:

Last but not least, inside our onPressed callback, we can trigger our side-effect as followed:

Note how we called tsx.get here instead of Ref.read. This is a feature unique to mutations. That tsx.get obtains the state of a provider, but keep it alive until the mutation is completed.

Starting 3.0, providers that fail during initialization will automatically retry. The retry is done with an exponential backoff, and the provider will be retried until it succeeds or is disposed. This helps when an operation fails due to a temporary issue, such as a lack of network connection.

The default behavior retries any error, and starts with a 200ms delay that doubles after each retry up to 6.4 seconds. This can be customized for all providers on ProviderContainer/ProviderScope by passing a retry parameter:

Alternatively, this can be configured on a per-provider basis by passing a retry parameter to the provider constructor:

The long-awaited Ref.mounted is finally here! It is similar to BuildContext.mounted, but for Ref.

You can use it to check if a provider is still mounted after an async operation:

For this to work, quite a few life-cycle changes were necessary. Make sure to read the life-cycle changes section.

When using code generation, you can now define type parameters for your generated providers. Type parameters work like any other provider parameter, and need to be passed when watching the provider.

In 2.0, Riverpod already had some form of pause/resume support, but it was fairly limited. With 3.0, all ref.listen listeners can be manually paused/resumed on demand:

At the same time, Riverpod now pauses providers in various situations:

See the life-cycle changes section for more details.

One goal of Riverpod 3.0 is to simplify the API. This includes:

For this sake, a few changes were made:

Those providers are not removed, but simply moved to a different import. Instead of:

This is to highlight that those providers are not recommended anymore. At the same time, those are preserved for backward compatibility.

No, the "auto-dispose" feature isn't removed. This only concerns the interfaces. In 2.0, all providers, Refs and Notifiers were duplicated for the sake of auto-dispose ( Ref vs AutoDisposeRef, Notifier vs AutoDisposeNotifier, etc). This was done for the sake of having a compilation error in some edge-cases, but came at the cost of a worse API.

In 3.0, the interfaces are unified, and the previous compilation error is now implemented as a lint rule (using riverpod_lint). What this means concretely is that you can replace all references to AutoDisposeNotifier with Notifier. The behavior of your code should not change.

Similarly to the previous point, the FamilyNotifier and Notifier interfaces are now fused.

Long story short, instead of:

This means that instead of Notifier+FamilyNotifier+AutoDisposeNotifier+AutoDisposeFamilyNotifier, we always use the Notifier class.

This change has no impact on code-generation.

In Riverpod 2.0, each provider came with its own Ref subclass (FutureProviderRef, StreamProviderRef, etc). Some Ref had state property, some a future, or a notifier, etc. Although useful, this was a lot of complexity for not much gain. One of the reasons for that is because Notifiers already have the extra properties it had, so the interfaces were redundant.

In 3.0, Ref is unified. No more generic parameter such as Ref<T>, no more FutureProviderRef. We only have one thing: Ref. What this means in practice is, the syntax for generated providers is simplified:

This does not concern WidgetRef, which is intact. Ref and WidgetRef are two different things.

updateShouldNotify is a method that is used to determine if a provider should notify its listeners when a state change occurs. But in 2.0, the implementation of this method varied quite a bit between providers. Some providers used ==, some identical, and some more complex logic.

Starting 3.0, all providers use == to filter notifications.

This can impact you in a few ways:

The most common case where you will be impacted is when using StreamProvider/StreamNotifier, as events of the stream are now filtered using ==.

If you are impacted by those changes, you can override updateShouldNotify to use a custom implementation:

In 2.0, in some edge-cases you could still interact with things like Ref or Notifier after they were disposed. This was not intended and caused various severe bugs.

In 3.0, Riverpod will throw an error if you try to interact with a disposed Ref/Notifier.

You can use Ref.mounted to check if a Ref/Notifier is still usable.

Before, if a provider threw an error, Riverpod would sometimes rethrow that error directly:

In 3.0, this is changed. Instead, the error will be encapsulated in a ProviderException that contains both the original error and its stack trace.

AsyncValue.error, ref.listen(..., onError: ...) and ProviderObservers are unaffected by this change, and will still receive the unaltered error.

This has multiple benefits:

For example, a ProviderObserver can use this to avoid logging the same error twice:

This is used internally by Riverpod in its automatic retry mechanism. The default automatic retry ignores ProviderExceptions:

Now that Riverpod has a way to pause listeners, Riverpod uses that to natively pauses listeners when the widget is not visible. In practice what this means is: Providers that are not used by the visible widget tree are paused.

As a concrete example, consider an application with two routes:

In typical applications, a user first opens the home page and then opens the settings page. This means that while the settings page is open, the homepage is also open, but not visible.

In 2.0, the homepage would actively keep listening to the websocket. In 3.0, the websocket provider will instead be paused, possibly saving resources.

How it works: Riverpod relies on TickerMode to determine if a widget is visible or not. And when false, all listeners of a Consumer are paused.

It also means that you can rely on TickerMode yourself to manually control the pause behavior of your consumers. You can voluntarily set the value to true/false to forcibly resume/pause listeners:

Riverpod 2.0 already had some form of pause/resume support. But it was limited and failed to cover some edge-cases. Consider:

In 2.0, if you were to call ref.read once on this provider, the state of the provider would be maintained, but 'paused' will be printed. This is because calling ref.read does not "listen" to the provider. And since the provider is not "listened" to, it is paused.

This is useful to pause providers that are currently not used! The problem is that in many cases, this optimization does not work. For example, your provider could be used indirectly through another provider.

In this scenario, if we click on the button once, then anotherProvider will start listening to our exampleProvider. But anotherProvider is no-longer used and will be paused. Yet exampleProvider will not be paused, because it thinks that it is still being used. As such, clicking on the button will not print 'paused' anymore.

In 3.0, this is fixed. If a provider is only used by paused providers, it is paused too.

In 2.0, there was a known inconvenience when using asynchronous providers combined with 'auto-dispose'.

Specifically, when an asynchronous provider watches an auto-dispose provider after an await, the "auto dispose" could be triggered unexpectedly.

In you run this on Dartpad, you will see that its prints:

As you can see, this consistently prints 0 every second, because the autoDispose provider repeatedly gets disposed during the async gap. A workaround was to move the ref.watch call before the await statement. But this is error prone, not very intuitive, and not always possible.

In 3.0, this is fixed by delaying the disposal of listeners. When a provider rebuilds, instead of immediately removing all of its listeners, it pauses them.

The exact same code will now instead print:

For the sake of differentiating between "a provider failed" from "a provider is depending on a failed provider", Riverpod 3.0 now wraps exceptions in a ProviderException that contains the original.

This means that if you catch errors in your providers, you will need to update your try/catch to inspect the content of ProviderException:

In 2.0, typical testing code would rely on a custom-made utility called createContainer. In 3.0, this utility is now part of Riverpod, and is called ProviderContainer.test. It creates a new container, and automatically disposes it after the test ends.

You can safely do a global search-and-replace for createContainer to ProviderContainer.test.

It is now possible to mock only the Notifier.build method, without mocking the whole notifier. This is useful when you want to initialize your notifier with a specific state, but still want to use the original implementation of the notifier.

A while back, FutureProvider.overrideWithValue and StreamProvider.overrideWithValue were removed "temporarily" from Riverpod. They are finally back!

A simple way to access the ProviderContainer in your widget tree.

See the WidgetTester.container extension for more information.

It is now possible to create custom ProviderListenables in Riverpod 3.0. This is doable using SyncProviderTransformerMixin.

The following example implements a variable of provider.select, where the callback returns a boolean instead of the selected value.

Used as ref.watch(provider.where((previous, value) => value > 0)).

Through riverpod_lint, Riverpod now includes a way to detect when scoping is used incorrectly. This lints detects when an override is missing, to avoid runtime errors.

To use this provider, you have two options. If neither of the following options are used, the provider will throw an error at runtime.

AsyncValue received various changes.

It is now possible to "unsubscribe" to the various life-cycles listeners:

When using Ref.listen, you can optionally specify weak: true:

Specifying this flag will tell Riverpod that it can still dispose the listened provider if it stops being used.

This flag is an advanced feature to help with some niche use-cases regarding combining multiple "sources of truth" in a single provider.

**Examples:**

Example 1 (dart):
```dart
// A example showcasing JsonSqFliteStorage without code generation.final storageProvider = FutureProvider<JsonSqFliteStorage>((ref) async {  // Initialize SQFlite. We should share the Storage instance between providers.  return JsonSqFliteStorage.open(    join(await getDatabasesPath(), 'riverpod.db'),  );});/// A serializable Todo class.class Todo {  const Todo({    required this.id,    required this.description,    required this.completed,  });  Todo.fromJson(Map<String, dynamic> json)      : id = json['id'] as int,        description = json['description'] as String,        completed = json['completed'] as bool;  final int id;  final String description;  final bool completed;  Map<String, dynamic> toJson() {    return {      'id': id,      'description': description,      'completed': completed,    };  }}final todosProvider =    AsyncNotifierProvider<TodosNotifier, List<Todo>>(TodosNotifier.new);class TodosNotifier extends AsyncNotifier<List<Todo>>{  @override  FutureOr<List<Todo>> build() async {    // We call persist at the start of our 'build' method.    // This will:    // - Read the DB and update the state with the persisted value the first    //   time this method executes.    // - Listen to changes on this provider and write those changes to the DB.    persist(      // We pass our JsonSqFliteStorage instance. No need to "await" the Future.      // Riverpod will take care of that.      ref.watch(storageProvider.future),      // A unique key for this state.      // No other provider should use the same key.      key: 'todos',      // By default, state is cached offline only for 2 days.      // We can optionally uncomment the following line to change cache duration.      // options: const StorageOptions(cacheTime: StorageCacheTime.unsafe_forever),      encode: jsonEncode,      decode: (json) {        final decoded = jsonDecode(json) as List;        return decoded            .map((e) => Todo.fromJson(e as Map<String, Object?>))            .toList();      },    );      // We asynchronously fetch todos from the server.      // During the await, the persisted todo list will be available.      // After the network request completes, the server state will take precedence      // over the persisted state.      final todos = await fetchTodos();      return todos;  }  Future<void> add(Todo todo) async {    // When modifying the state, no need for any extra logic to persist the change.    // Riverpod will automatically cache the new state and write it to the DB.    state = AsyncData([...await future, todo]);  }}
```

Example 2 (dart):
```dart
@riverpodFuture<JsonSqFliteStorage> storage(Ref ref) async {  // Initialize SQFlite. We should share the Storage instance between providers.  return JsonSqFliteStorage.open(    join(await getDatabasesPath(), 'riverpod.db'),  );}/// A serializable Todo class. We're using Freezed for simple serialization.@freezedabstract class Todo with _$Todo {  const factory Todo({    required int id,    required String description,    required bool completed,  }) = _Todo;  factory Todo.fromJson(Map<String, dynamic> json) => _$TodoFromJson(json);}@riverpod@JsonPersist()class TodosNotifier extends _$TodosNotifier {  @override  FutureOr<List<Todo>> build() async {    // We call persist at the start of our 'build' method.    // This will:    // - Read the DB and update the state with the persisted value the first    //   time this method executes.    // - Listen to changes on this provider and write those changes to the DB.    persist(      // We pass our JsonSqFliteStorage instance. No need to "await" the Future.      // Riverpod will take care of that.      ref.watch(storageProvider.future),      // By default, state is cached offline only for 2 days.      // We can optionally uncomment the following line to change cache duration.      // options: const StorageOptions(cacheTime: StorageCacheTime.unsafe_forever),    );    // We asynchronously fetch todos from the server.    // During the await, the persisted todo list will be available.    // After the network request completes, the server state will take precedence    // over the persisted state.    final todos = await fetchTodos();    return todos;  }  Future<void> add(Todo todo) async {    // When modifying the state, no need for any extra logic to persist the change.    // Riverpod will automatically cache the new state and write it to the DB.    state = AsyncData([...await future, todo]);  }}
```

Example 3 (dart):
```dart
final addTodoMutation = Mutation<void>();
```

Example 4 (dart):
```dart
class AddTodoButton extends ConsumerWidget {  @override  Widget build(BuildContext context, WidgetRef ref) {    // Listen to the status of the "addTodo" side-effect    final addTodo = ref.watch(addTodoMutation);    return switch (addTodo) {      // No side-effect is in progress      // Let's show a submit button      MutationIdle() => ElevatedButton(        // Trigger the side-effect on click        onPressed: () {          // TODO see explanation after the code snippet        },        child: const Text('Submit'),      ),      // The side-effect is in progress. We show a spinner      MutationPending() => const CircularProgressIndicator(),      // The side-effect failed. We show a retry button      MutationError() => ElevatedButton(        onPressed: () {          // TODO see explanation after the code snippet        },        child: const Text('Retry'),      ),      // The side-effect was successful. We show a success message      MutationSuccess() => const Text('Todo added!'),    };  }}
```

---
