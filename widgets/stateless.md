# Flutter StatelessWidget

This document covers the **StatelessWidget**, one of the foundational building blocks in Flutter for creating immutable UI components.

---

## 1. What is a StatelessWidget?

- A `StatelessWidget` is a widget that describes part of the user interface by building a constellation of other widgets that describe the UI more concretely.
- It is **immutable**: once created, its properties cannot change over its lifetime.
- Ideal for UI elements that do not need to maintain any internal state (e.g., icons, labels, static layouts).

---

## 2. Basic Structure

```dart
import 'package:flutter/material.dart';

class MyLabel extends StatelessWidget {
  final String text;
  final Color color;

  const MyLabel({
    Key? key,
    required this.text,
    this.color = Colors.black,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Text(
      text,
      style: TextStyle(
        color: color,
        fontSize: 18,
      ),
    );
  }
}
```

- **Constructor**: typically `const` to enable compile-time optimizations.
- **Fields**: `final` (immutable properties).
- **build()**: returns a `Widget` tree.

---

## 3. Usage Example

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('StatelessWidget Example')),
        body: Center(
          child: MyLabel(
            text: 'Hello, StatelessWidget!',
            color: Colors.blue,
          ),
        ),
      ),
    );
  }
}
```

- Place your custom `StatelessWidget` anywhere in the widget tree just like built-in widgets.

---

## 4. When to Use StatelessWidget

| Scenario                                      | Use StatelessWidget When...                       |
|-----------------------------------------------|---------------------------------------------------|
| Static text or images                         | content never changes after creation              |
| Layout-only widgets (e.g., Rows, Columns)     | no state management needed                        |
| Performance-critical, immutable components    | const constructors allow compile-time reuse       |

---

## 5. Comparison with StatefulWidget

| Feature                   | StatelessWidget                 | StatefulWidget                          |
|---------------------------|---------------------------------|-----------------------------------------|
| Mutability                | Immutable                       | Mutable state via `State` object        |
| Lifecycle                 | Only `build`                    | `initState`, `build`, `setState`, etc.  |
| Use Case                  | Static UI                       | Interactive or data-driven UI           |


---

## 6. Best Practices

- **Use `const` constructors** whenever possible to improve performance and reduce rebuilds.
- **Keep widgets small and focused**: each widget should do one thing well.
- **Pass data via constructor parameters**, not by modifying fields after creation.
- **Compose widgets**: build complex UIs by combining simple `StatelessWidget`s.

---

## 7. Further Reading

- [StatelessWidget API Documentation](https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html)
- [Flutter Widget Catalog](https://flutter.dev/docs/development/ui/widgets)

---

*End of Flutter StatelessWidget*

