# Riverpod-V3 - Advanced

**Pages:** 3

---

## Mutations (experimental)

**URL:** https://riverpod.dev/docs/concepts2/mutations

**Contents:**
- Mutations (experimental)
- Defining a mutation​
- Listening to a mutation​
  - Scoping a mutation​
  - Triggering a mutation​
  - The different mutation states and their meaning​
  - After a mutation has been started once, how to reset it to its idle state?​

Mutations are experimental, and the API may change in a breaking way without a major version bump.

Mutations, in Riverpod, are objects which enable the user interface to react to state changes. A common use-case is displaying a loading indicator while a form is being submitted

In short, mutations are to achieve effects such as this: !

Without mutations, you would have to store the progress of the form submission directly inside the state of a provider. This is not ideal as it pollutes the state of your provider with UI concerns ; and it involves a lot of boilerplate code to handle the loading state, error state, and success state.

Mutations are designed to handle these concerns in a more elegant way.

Mutations are instances of the Mutation object, stored in a final variable somewhere.

Typically, this variable will either be global or as a static final variable on a Notifier.

Once we've defined a mutation, we can start using it inside Consumers or Providers. For this, we will need a Refs and pick a listening method of our choice (typically Ref.watch).

A typical example would be:

Sometimes, you may want to have multiple instances of the same mutation.

This can include things like an id, or any other parameter that makes the mutation unique.

This is useful if you want to have multiple instances of the same mutation, such as deleting a specific item in a list

Simply call the mutation with the unique key:

Sometimes, these mutations have a generic return type, such as if an api response may have different response types based on the input parameters, such as with deserialization.

So far, we've listened to the state of a mutation, but nothing actually happens yet.

To trigger a mutation, we can use Mutation.run, pass our mutation, and provide an asynchronous callback that updates whatever state we want. Lastly, we'll need to return a value matching the generic type of the mutation.

Mutations can be in one of the following states:

You can switch over the different states using a switch statement:

Mutations naturally reset themselves to MutationIdle if:

This is similar to how Automatic disposal works, but for mutations.

Alternatively, you can manually reset a mutation to its idle state by calling the Mutation.reset method:

**Examples:**

Example 1 (dart):
```dart
// A mutation to track the "add todo" operation.// The generic type is optional and can be specified to enable the UI to interact// with the result of the mutation.final addTodo = Mutation<Todo>();
```

Example 2 (dart):
```dart
class Example extends ConsumerWidget {  const Example({super.key});  @override  Widget build(BuildContext context, WidgetRef ref) {    // We listen to the current state of the "addTodo" mutation.    // Listening to this will not perform any side effects by itself.    final addTodoState = ref.watch(addTodo);    return Row(      children: [        ElevatedButton(          style: ButtonStyle(            // If there is an error, we show the button in red            backgroundColor: switch (addTodoState) {              MutationError() => const WidgetStatePropertyAll(Colors.red),              _ => null,            },          ),          onPressed: () {            addTodo.run(ref, (tsx) async {              // todo            });          },          child: const Text('Add todo'),        ),        // The operation is pending, let's show a progress indicator        if (addTodoState is MutationPending) ...[          const SizedBox(width: 8),          const CircularProgressIndicator(),        ],      ],    );  }}
```

Example 3 (dart):
```dart
final removeTodo = Mutation<void>();final removeTodoWithId = removeTodo(todo.id);
```

Example 4 (dart):
```dart
final create = Mutation<ApiResponse>();final createTodo = create<CreatedResponse<Todo>>('create_todo');Future<void> executeCreateTodo(MutationTarget ref) async {  await createTodo.run(ref, (tsx) async {    final client = tsx.get(apiProvider);    final response = client.post('/todos', data: {'title': 'Eat a cookie'});    return CreatedResponse<Todo>.fromJson(response.data, Todo.fromJson);  });}
```

---

## Automatic retry

**URL:** https://riverpod.dev/docs/concepts2/retry

**Contents:**
- Automatic retry
- Customizing retry logic​
  - Disabling retry​
- About the default retry logic​
- Awaiting for retries to complete​

In Riverpod, Providers are automatically retried when they fail.

A retry is attempted when an exception is thrown during the provider's computation. The retry logic can be customized either on a per-provider basis or globally for all providers.

By default, a provider can be retried up to 10 times, with an exponential backoff going from 200ms to 6.4 seconds. For the full details about the default retry logic, see retry.

A custom retry logic can be provided either for the full application or for a specific provider.

The implementation is the same for both cases: Custom retry logic is a function that is expected to return a Duration? value ; which indicates the delay before the next retry (or null to stop retrying).

The following implements a custom retry function, which will retry up to 5 times, with an exponential backoff starting at 200ms, and ignores ProviderExceptions

This function can then be used either inside providers to update the retry logic for that specific provider:

Or globally by passing it to ProviderContainers/ProviderScopes:

Disabling retry is as simple as always retuning null in the retry function. If you wish to disable retry for all your application, do:

The default retry logic is designed to be a more more clever than a naive "if fail, retry". In particular, it will not retry Errors and ProviderExceptions.

Errors are not retried, because they are not recoverable. They indicate a bug in the code, and retrying would not help. Retrying in those cases would just pollute the logs with useless retry attempts.

As for ProviderExceptions, those are not retried because they indicate that a provider did not fail, but instead rethrow an exception from a failed provider. Consider:

In this example, although myProvider fails, it is not responsible for the failure. Retrying it would not help. Instead, it is failedProvider that should be retried.

This implies that if you disable retry for failedProvider, then myProvider will also not be retried.

You may be aware that you can await for asynchronous providers to complete, by using FutureProvider.future:

But you may wonder how automatic retry interacts with this.

In short, when an asynchronous provider fails and is retried, the associated future will keep waiting until either:

This ensures that await ref.watch(myProvider.future) skips the intermediate failures.

**Examples:**

Example 1 (dart):
```dart
Duration? myRetry(int retryCount, Object error) {  // Stop retrying on ProviderException  if (retryCount >= 5) return null;  // Ignore ProviderException  if (error is ProviderException) return null;  return Duration(milliseconds: 200 * (1 << retryCount)); // Exponential backoff}
```

Example 2 (dart):
```dart
final myProvider = Provider<int>(    retry: myRetry,    (ref) => 0,  );
```

Example 3 (dart):
```dart
@Riverpod(retry: myRetry)int myProvider(MyProviderRef ref) {  return 0;}
```

Example 4 (dart):
```dart
// For pure Dart codefinal container = ProviderContainer(  retry: myRetry,);...// For Flutter coderunApp(  ProviderScope(    retry: myRetry,    child: MyApp(),  ),);
```

---

## Offline persistence (experimental)

**URL:** https://riverpod.dev/docs/concepts2/offline

**Contents:**
- Offline persistence (experimental)
- Creating a Storage​
- Persisting the state of a provider​
  - Using simplified JSON serialization (code-generation)​
- Understanding persist keys​
- Changing the cache duration​
- Using "destroy key" for simple data-migration​
- Waiting for persistence decoding​
- Testing persistence​

Offline persistence is the ability to store the state of Providers on the user's device, so that it can be accessed even when the user is offline or when the app is restarted.

Riverpod is independent from the underlying database or the protocol used to store the data. But by default, Riverpod provides riverpod_sqflite alongside basic JSON serialization.

Riverpod's offline persistence is designed to be a simple wrapper around databases. It is not designed to fully replace code for interacting with a database.

You may still need to manually interact with a database for:

Offline persistence works using two parts:

Before we start persisting notifiers, we need to instantiate an object that implements the Storage interface. This object will be responsible for connecting Riverpod with your database.

You need have to either:

If using SQFlite, you can use riverpod_sqflite:

Then, you can create a Storage by instantiating JsonSqFliteStorage:

Once we've created a Storage, we can start persisting the state of providers. Currently, only "Notifiers" can be persisted. See Providers for more information about them.

To persist the state of a notifier, you will typically need to call AnyNotifier.persist inside the build method of your notifier.

If you are using riverpod_sqflite and code-generation, you can simplify the persist call by using the JsonPersist annotation:

In some of the previous snippets, we've passed a key parameter to AnyNotifier.persist. That key is there to enable your database to know where to store the state of a provider in the Database. Depending on the database, this key may be a unique row ID.

When specifying key, it is critical to ensure that:

By default, state is only cached for 2 days. This default ensures that no leak happens and deleted providers stay in the database indefinitely

This is generally safe, as Riverpod is designed to be used primarily as a cache for IO operations (network requests, database queries, etc). But such default will not be suitable for all use-cases, such as if you want to store user preferences.

To change this default, specify options like so:

If you set the cache duration to infinite, make sure to manually delete the persisted state from the database if you ever delete the provider.

For this, refer to your database's documentation.

A common challenge when persisting data is handling when the data structure changes. If you change how an object is serialized, you may need to migrate the data stored in the database.

While Riverpod does not provide a way to do proper data migration, it does provide a way to easily replace the old persisted state with a brand new one: Destroy keys.

Destroy keys help doing simple data migrations by enabling Riverpod to know when the old persisted state should be discarded. When a new version of the application is released with a different destroyKey, the old persisted state will be discarded, and the provider will be initialized as if it was never persisted.

Until now, we've never waited for AnyNotifier.persist to complete. This is voluntary, as this allows the provider to start its network requests as soon as possible. However, it means that the provider cannot easily access the persisted state right after calling persist.

In some cases, instead of initializing the provider with a network request, you may want to initialize it with the persisted state.

In that case, you can await the result of persist as follows:

This enables accessing the persisted state within build using this.state:

When testing your application, it may be inconvenient to use a real database. In particular, unit and widget tests will not have access to a device, and thus cannot use a database.

For this reason, Riverpod provides a way to use an in-memory database using Storage.inMemory. To have your test use this in-memory database, you can use Provider overrides:

**Examples:**

Example 1 (unknown):
```unknown
dart pub add riverpod_sqflite sqflite
```

Example 2 (dart):
```dart
import 'package:flutter_riverpod/experimental/persist.dart';import 'package:flutter_riverpod/flutter_riverpod.dart';import 'package:path/path.dart';import 'package:riverpod_sqflite/riverpod_sqflite.dart';import 'package:sqflite/sqflite.dart';final storageProvider = FutureProvider<Storage<String, String>>((ref) async {  // Initialize SQFlite. We should share the Storage instance between providers.  return JsonSqFliteStorage.open(    join(await getDatabasesPath(), 'riverpod.db'),  );});
```

Example 3 (dart):
```dart
import 'package:flutter_riverpod/experimental/persist.dart';import 'package:path/path.dart';import 'package:riverpod_annotation/riverpod_annotation.dart';import 'package:riverpod_sqflite/riverpod_sqflite.dart';import 'package:sqflite/sqflite.dart';part 'codegen.g.dart';@riverpodFuture<Storage<String, String>> storage(Ref ref) async {  // Initialize SQFlite. We should share the Storage instance between providers.  return JsonSqFliteStorage.open(    join(await getDatabasesPath(), 'riverpod.db'),  );}
```

Example 4 (dart):
```dart
class Todo {  Todo({required this.task});  final String task;}final todoListProvider = AsyncNotifierProvider<TodoList, List<Todo>>(  TodoList.new,);class TodoList extends AsyncNotifier<List<Todo>> {  @override  Future<List<Todo>> build() async {    persist(      // We pass in the previously created Storage.      // Do not "await" this. Riverpod will handle it for you.      ref.watch(storageProvider.future),      // A unique identifier for this state.      // If your provider receives parameters, make sure to encode those      // in the key as well.      key: 'todo_list',      // Encode/decode the state. Here, we're using a basic JSON encoding.      // You can use any encoding you want, as long as your Storage supports it.      encode: (todos) => todos.map((todo) => {'task': todo.task}).toList(),      decode:          (json) =>              (json as List)                  .map((todo) => Todo(task: todo['task'] as String))                  .toList(),    );    // Regardless of whether some state was restored or not, we fetch the list of    // todos from the server.    return fetchTodosFromServer();  }}
```

---
