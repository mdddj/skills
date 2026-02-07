# Riverpod-V3 - Getting Started

**Pages:** 1

---

## Getting started

**URL:** https://riverpod.dev/docs/introduction/getting_started

**Contents:**
- Getting started
- Try Riverpod online​
- Installing the package​
- Enabling riverpod_lint​
- Usage example: Hello world​
- Going further: Installing code snippets​

To get a feel of Riverpod, try it online on Dartpad or on Zapp:

Riverpod comes as a main "riverpod" package that’s self-sufficient, complemented by optional packages for using code generation (About code generation) and hooks (About hooks).

Once you know what package(s) you want to install, proceed to add the dependency to your app in a single line like this:

Alternatively, you can manually add the dependency to your app from within your pubspec.yaml:

Then, install packages with flutter pub get.

Then, install packages with dart pub get.

If using code-generation, you can now run the code-generator with:

That's it. You've added Riverpod to your app!

Riverpod comes with an optional riverpod_lint package that provides lint rules to help you write better code, and provide custom refactoring options.

Riverpod_lint is implemented using analysis_server_plugin. As such, it is installed through analysis_options.yaml

Long story short, create an analysis_options.yaml next to your pubspec.yaml and add:

You should now see warnings in your IDE if you made mistakes when using Riverpod in your codebase.

To see the full list of warnings and refactorings, head to the riverpod_lint page.

Now that we have installed Riverpod, we can start using it.

The following snippets showcase how to use our new dependency to make a "Hello world":

Then, start the application with flutter run. This will render "Hello world" on your device.

Then, start the application with dart lib/main.dart. This will print "Hello world" in the console.

If you are using Flutter and VS Code , consider using Flutter Riverpod Snippets

If you are using Flutter and Android Studio or IntelliJ, consider using Flutter Riverpod Snippets

**Examples:**

Example 1 (dart):
```dart
flutter pub add flutter_riverpod
```

Example 2 (dart):
```dart
flutter pub add hooks_riverpodflutter pub add flutter_hooks
```

Example 3 (dart):
```dart
flutter pub add flutter_riverpodflutter pub add riverpod_annotationflutter pub add dev:riverpod_generatorflutter pub add dev:build_runner
```

Example 4 (dart):
```dart
flutter pub add hooks_riverpodflutter pub add flutter_hooksflutter pub add riverpod_annotationflutter pub add dev:riverpod_generatorflutter pub add dev:build_runner
```

---
