# Riverpod-V3 - Best Practices

**Pages:** 1

---

## DO/DON'T

**URL:** https://riverpod.dev/docs/root/do_dont

**Contents:**
- DO/DON'T
- AVOID initializing providers in a widget​
- AVOID using providers for Ephemeral state.​
- DON'T perform side effects during the initialization of a provider​
- PREFER ref.watch/read/listen (and similar APIs) with statically known providers​
- AVOID dynamically creating providers​

To ensure good maintainability of your code, here is a list of good practices you should follow when using Riverpod.

This list is not exhaustive, and is subject to change. If you have any suggestions, feel free to open an issue.

Items in this list are not in any particular order.

A good portion of these recommendations can be enforced with riverpod_lint. See Getting started for installation instructions.

Providers should initialize themselves. They should not be initialized by an external element such as a widget.

Failing to do so could cause possible race conditions and unexpected behaviors.

There is no "one-size fits all" solution to this problem. If your initialization logic depends on factors external to the provider, often the correct place to put such logic is in the onPressed method of a button triggering navigation:

Providers are designed to be for shared business state. They are not meant to be used for Ephemeral state, such as for:

If you are looking for a way to handle local widget state, consider using flutter_hooks instead.

One reason why this is discouraged is that such state is often scoped to a route. Failing to do so could break your app's back button, due to a new page overriding the state of a previous page.

For instance say we were to store the currently selected book in a provider:

One challenge we could face is, the navigation history could look like:

In this scenario, when pressing the back button, we should expect to go back to /books/42. But if we were to use selectedBookProvider to store the selected book, the selected ID would not reset to its previous value, and we would keep seeing /books/21.

Providers should generally be used to represent a "read" operation. You should not use them for "write" operations, such as submitting a form.

Using providers for such operations could have unexpected behaviors, such as skipping a side-effect if a previous one was performed.

If you are looking at a way to handle loading/error states of a side-effect, see Mutations (experimental).

Riverpod strongly recommends enabling lint rules (via riverpod_lint). But for lints to be effective, your code should be written in a way that is statically analysable.

Failing to do so could make it harder to spot bugs or cause false positives with lints.

Providers should exclusively be top-level final variables.

Creating providers as static final variables is allowed, but not supported by the code-generator.

**Examples:**

Example 1 (dart):
```dart
class WidgetState extends State<MyWidget> {  @override  void initState() {    super.initState();    // Bad: the provider should initialize itself    ref.read(provider).init();  }}
```

Example 2 (dart):
```dart
ElevatedButton(  onPressed: () {    ref.read(provider).init();    Navigator.of(context).push(...);  },  child: Text('Navigate'),)
```

Example 3 (dart):
```dart
final selectedBookProvider = StateProvider<String?>((ref) => null);
```

Example 4 (dart):
```dart
/books/books/42/books/21
```

---
