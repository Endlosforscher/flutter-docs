# Flutter Theming

This document explains how to implement and customize theming in Flutter, covering Material themes, dark/light modes, theme extensions, dynamic theming, and best practices.

---

## 1. Introduction to Theming

- Theming allows consistent styling across your app by centralizing colors, typography, and component styles.
- Flutter provides `ThemeData` for Material-based theming and `CupertinoThemeData` for Cupertino styling.
- Use `Theme` widgets and `Theme.of(context)` to access theme properties anywhere in the widget tree.

---

## 2. MaterialApp and ThemeData

Wrap your app in a `MaterialApp` and provide a `ThemeData`:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Theming Demo',
      theme: ThemeData(
        primarySwatch: Colors.indigo,
        colorScheme: ColorScheme.fromSwatch(
          primarySwatch: Colors.indigo,
          accentColor: Colors.amber,
        ),
        textTheme: const TextTheme(
          headline6: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
          bodyText2: TextStyle(fontSize: 16),
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(12),
            ),
          ),
        ),
      ),
      home: const HomePage(),
    );
  }
}
```

- **`primarySwatch`**: generates primary, primaryVariant, etc.
- **`colorScheme`**: modern way to define all colors.
- **`textTheme`**: centralizes text styles.
- **Component themes**: override defaults for widgets (buttons, cards, inputs).

---

## 3. Light and Dark Themes

Provide both `theme` and `darkTheme`, and optionally `themeMode`:

```dart
MaterialApp(
  theme: ThemeData.light().copyWith(
    primaryColor: Colors.blue,
  ),
  darkTheme: ThemeData.dark().copyWith(
    primaryColor: Colors.tealAccent,
  ),
  themeMode: ThemeMode.system, // can be light, dark, or system
  home: const HomePage(),
)
```

Detect current brightness in widgets:

```dart
bool isDark = Theme.of(context).brightness == Brightness.dark;
```

---

## 4. Accessing Theme Properties

Use `Theme.of(context)` or `context` extensions from packages:

```dart
// Colors
Color primary = Theme.of(context).colorScheme.primary;

// Text styles
TextStyle heading = Theme.of(context).textTheme.headline6!;
```

Example usage:

```dart
Text(
  'Hello, themed world!',
  style: Theme.of(context).textTheme.headline6,
)

ElevatedButton(
  onPressed: () {},
  child: const Text('Press me'),
)
```

---

## 5. Custom Theme Extensions

Extend `ThemeData` with custom values:

```dart
class MyColors extends ThemeExtension<MyColors> {
  final Color? success;
  final Color? warning;

  const MyColors({this.success, this.warning});

  @override
  MyColors copyWith({Color? success, Color? warning}) {
    return MyColors(
      success: success ?? this.success,
      warning: warning ?? this.warning,
    );
  }

  @override
  MyColors lerp(ThemeExtension<MyColors>? other, double t) {
    if (other is! MyColors) return this;
    return MyColors(
      success: Color.lerp(success, other.success, t),
      warning: Color.lerp(warning, other.warning, t),
    );
  }
}

// In ThemeData
theme: ThemeData(...).copyWith(
  extensions: [
    const MyColors(success: Colors.green, warning: Colors.orange),
  ],
),

// Access in widget
final myColors = Theme.of(context).extension<MyColors>()!;
Container(color: myColors.success);
```

---

## 6. Dynamic Theming

Use `Provider`, `Riverpod`, or `Bloc` to switch themes at runtime:

```dart
class ThemeNotifier extends ChangeNotifier {
  ThemeMode _mode = ThemeMode.system;
  ThemeMode get mode => _mode;
  void toggle() {
    _mode = _mode == ThemeMode.light ? ThemeMode.dark : ThemeMode.light;
    notifyListeners();
  }
}

// In main
return ChangeNotifierProvider(
  create: (_) => ThemeNotifier(),
  child: Consumer<ThemeNotifier>(
    builder: (_, notifier, __) => MaterialApp(
      theme: lightTheme,
      darkTheme: darkTheme,
      themeMode: notifier.mode,
      home: const HomePage(),
    ),
  ),
);
```

---

## 7. Theming Widgets Individually

Override theme for a subtree:

```dart
Theme(
  data: Theme.of(context).copyWith(
    colorScheme: Theme.of(context).colorScheme.copyWith(primary: Colors.red),
  ),
  child: ElevatedButton(
    onPressed: () {},
    child: const Text('Themed Locally'),
  ),
)
```

Use widget-level themes like `CardTheme`, `InputDecorationTheme`:

```dart
ThemeData(
  cardTheme: CardTheme(
    color: Colors.grey[100],
    shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(8)),
  ),
  inputDecorationTheme: const InputDecorationTheme(
    border: OutlineInputBorder(),
  ),
)
```

---

## 8. Best Practices

- Centralize all theme definitions in one file or folder.
- Use `ColorScheme` over deprecated color properties.
- Leverage `const` constructors for themes when possible.
- Provide accessible contrast for text and UI elements.
- Test dark and light themes thoroughly.
- Document custom theme extensions.

---

## 9. Further Reading

- [Theming Tutorial](https://flutter.dev/docs/cookbook/design/themes)
- [ThemeData API](https://api.flutter.dev/flutter/material/ThemeData-class.html)
- [Adaptive Themes](https://flutter.dev/docs/development/platform-adaptations)

---

*End of Flutter Theming*

