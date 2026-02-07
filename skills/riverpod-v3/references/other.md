# Riverpod-V3 - Other

**Pages:** 1

---

## How to debounce/cancel network requests

**URL:** https://riverpod.dev/docs/how_to/cancel

**Contents:**
- How to debounce/cancel network requests
- The application​
- Cancelling requests​
- Debouncing requests​
- Going further: Doing both at once​

As applications grow in complexity, it's common to have multiple network requests in flight at the same time. For example, a user might be typing in a search box and triggering a new request for each keystroke. If the user types quickly, the application might have many requests in flight at the same time.

Alternatively, a user might trigger a request, then navigate to a different page before the request completes. In this case, the application might have a request in flight that is no longer needed.

To optimize performance in those situations, there are a few techniques you can use:

In Riverpod, both of these techniques can be implemented in a similar way. The key is to use ref.onDispose combined with "automatic disposal" or ref.watch to achieve the desired behavior.

To showcase this, we will make a simple application with two pages:

We will then implement the following behaviors:

First, let's create the application, without any debouncing or cancelling. We won't use anything fancy here, and stick to a plain FloatingActionButton with a Navigator.push to open the detail page.

First, let's start with defining our home screen. As usual, let's not forget to specify a ProviderScope at the root of our application.

Then, let's define our detail page. To fetch the activity and implement pull-to-refresh, refer to the Implementing pull-to-refresh case study.

Now that we have a working application, let's implement the cancellation logic.

To do so, we will use ref.onDispose to cancel the request when the user navigates away from the page. For this to work, it is important that the automatic disposal of providers is enabled.

The exact code needed to cancel the request will depend on the HTTP client. In this example, we will use package:http, but the same principle applies to other clients.

The key here is that ref.onDispose will be called when the user navigates away. That is because our provider is no-longer used, and therefore disposed thanks to automatic disposal. We can therefore use this callback to cancel the request. When using package:http, this can be done by closing our HTTP client.

Now that we have implemented cancellation, let's implement debouncing. At the moment, if the user refreshes the activity multiple times in a row, we will send a request for each refresh.

Technically speaking, now that we have implemented cancellation, this is not a problem. If the user refreshes the activity multiple times in a row, the previous request will be cancelled, when a new request is made.

However, this is not ideal. We are still sending multiple requests, and wasting bandwidth and server resources. What we could instead do is delay our requests until the user stops refreshing the activity for a fixed amount of time.

The logic here is very similar to the cancellation logic. We will again use ref.onDispose. However, the idea here is that instead of closing an HTTP client, we will rely on onDispose to abort the request before it starts. We will then arbitrarily wait for 500ms before sending the request. Then, if the user refreshes the activity again before the 500ms have elapsed, onDispose will be invoked, aborting the request.

To abort requests, a common practice is to voluntarily throw. It is safe to throw inside providers after the provider has been disposed. The exception will naturally be caught by Riverpod and be ignored.

We now know how to debounce and cancel requests. But currently, if we want to do another request, we need to copy-paste the same logic in multiple places. This is not ideal.

However, we can go further and implement a reusable utility to do both at once.

The idea here is to implement an extension method on Ref that will handle both cancellation and debouncing in a single method.

We can then use this extension method in our providers as followed:

**Examples:**

Example 1 (dart):
```dart
void main() => runApp(const ProviderScope(child: MyApp()));class MyApp extends StatelessWidget {  const MyApp({super.key});  @override  Widget build(BuildContext context) {    return MaterialApp(      routes: {        '/detail-page': (_) => const DetailPageView(),      },      home: const ActivityView(),    );  }}class ActivityView extends ConsumerWidget {  const ActivityView({super.key});  @override  Widget build(BuildContext context, WidgetRef ref) {    return Scaffold(      appBar: AppBar(title: const Text('Home screen')),      body: const Center(        child: Text('Click the button to open the detail page'),      ),      floatingActionButton: FloatingActionButton(        onPressed: () => Navigator.of(context).pushNamed('/detail-page'),        child: const Icon(Icons.add),      ),    );  }}
```

Example 2 (dart):
```dart
class Activity {  Activity({    required this.activity,    required this.type,    required this.participants,    required this.price,  });  factory Activity.fromJson(Map<Object?, Object?> json) {    return Activity(      activity: json['activity']! as String,      type: json['type']! as String,      participants: json['participants']! as int,      price: json['price']! as double,    );  }  final String activity;  final String type;  final int participants;  final double price;}final activityProvider = FutureProvider.autoDispose<Activity>((ref) async {  final response = await http.get(    Uri.https('www.boredapi.com', '/api/activity'),  );  final json = jsonDecode(response.body) as Map;  return Activity.fromJson(json);});class DetailPageView extends ConsumerWidget {  const DetailPageView({super.key});  @override  Widget build(BuildContext context, WidgetRef ref) {    final activity = ref.watch(activityProvider);    return Scaffold(      appBar: AppBar(        title: const Text('Detail page'),      ),      body: RefreshIndicator(        onRefresh: () => ref.refresh(activityProvider.future),        child: ListView(          children: [            switch (activity) {              AsyncValue(:final value?) => Text(value.activity),              AsyncValue(:final error?) => Text('Error: $error'),              _ => const Center(child: CircularProgressIndicator()),            },          ],        ),      ),    );  }}
```

Example 3 (dart):
```dart
@freezedsealed class Activity with _$Activity {  factory Activity({    required String activity,    required String type,    required int participants,    required double price,  }) = _Activity;  factory Activity.fromJson(Map<String, dynamic> json) =>      _$ActivityFromJson(json);}@riverpodFuture<Activity> activity(Ref ref) async {  final response = await http.get(    Uri.https('www.boredapi.com', '/api/activity'),  );  final json = jsonDecode(response.body) as Map;  return Activity.fromJson(Map.from(json));}class DetailPageView extends ConsumerWidget {  const DetailPageView({super.key});  @override  Widget build(BuildContext context, WidgetRef ref) {    final activity = ref.watch(activityProvider);    return Scaffold(      appBar: AppBar(        title: const Text('Detail page'),      ),      body: RefreshIndicator(        onRefresh: () => ref.refresh(activityProvider.future),        child: ListView(          children: [            switch (activity) {              AsyncValue(:final value?) => Text(value.activity),              AsyncValue(:final error?) => Text('Error: $error'),              _ => const Center(child: CircularProgressIndicator()),            },          ],        ),      ),    );  }}
```

Example 4 (dart):
```dart
final activityProvider = FutureProvider.autoDispose<Activity>((ref) async {  // We create an HTTP client using package:http  final client = http.Client();  // On dispose, we close the client.  // This will cancel any pending request that the client might have.  ref.onDispose(client.close);  // We now use the client to make the request instead of the "get" function.  final response = await client.get(    Uri.https('www.boredapi.com', '/api/activity'),  );  // The rest of the code is the same as before  final json = jsonDecode(response.body) as Map;  return Activity.fromJson(Map.from(json));});
```

---
