# Flutter StatefulWidget

This document covers the **StatefulWidget**, a core concept in Flutter for building UI components that maintain mutable state.

---

## 1. What is a StatefulWidget?

- A `StatefulWidget` is a widget that has mutable state. When the state changes, the widget rebuilds to reflect the updated state.
- Consists of two classes:
  1. **`StatefulWidget`** subclass: immutable configuration.
  2. **`State<T>`** subclass: holds mutable state and implements the `build` method.

---

## 2. Basic Structure

```dart
import 'package:flutter/material.dart';

class Counter extends StatefulWidget {
  const Counter({Key? key}) : super(key: key);

  @override
  _CounterState createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _count = 0;

  void _increment() {
    setState(() {
      _count++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      children: [
        Text('Count: \$_count', style: TextStyle(fontSize: 24)),
        ElevatedButton(
          onPressed: _increment,
          child: const Text('Increment'),
        ),
      ],
    );
  }
}
```

- **`createState()`**: returns the mutable `State` object.
- **`setState()`**: schedules a rebuild when state changes.
- **Private `_CounterState`**: underscore prefix marks it as library-private.

---

## 3. Lifecycle Methods

| Method                | Description                                                  |
|-----------------------|--------------------------------------------------------------|
| `initState()`         | Called once when the `State` object is inserted into the tree. Ideal for one-time initialization. |
| `didChangeDependencies()` | Called when inherited widgets change.                        |
| `build()`             | Describes the UI; called after `initState`, `setState`, and dependency changes. |
| `didUpdateWidget()`   | Called when the widget configuration changes (new `Widget` instance). |
| `setState()`          | Triggers a rebuild of the widget.                            |
| `deactivate()`        | Called when the `State` is removed from the tree temporarily. |
| `dispose()`           | Called when the `State` object is permanently removed. Use to clean up controllers and listeners. |

```dart
@override
void initState() {
  super.initState();
  // Initialize resources
}

@override
void dispose() {
  // Dispose resources
  super.dispose();
}
```

---

## 4. Example: Todo Item

```dart
class TodoItem extends StatefulWidget {
  final String title;
  const TodoItem({Key? key, required this.title}) : super(key: key);

  @override
  _TodoItemState createState() => _TodoItemState();
}

class _TodoItemState extends State<TodoItem> {
  bool _completed = false;

  void _toggleComplete() {
    setState(() {
      _completed = !_completed;
    });
  }

  @override
  Widget build(BuildContext context) {
    return ListTile(
      title: Text(
        widget.title,
        style: TextStyle(
          decoration: _completed ? TextDecoration.lineThrough : null,
        ),
      ),
      leading: Checkbox(
        value: _completed,
        onChanged: (_) => _toggleComplete(),
      ),
    );
  }
}
```

- Access widget properties via `widget` inside `State`.
- `setState` toggles the `_completed` boolean and rebuilds.

---

## 5. When to Use StatefulWidget

| Scenario                                     | Use StatefulWidget When...                               |
|----------------------------------------------|----------------------------------------------------------|
| Interactive UI with user input               | fields, checkboxes, toggles, forms                       |
| Animations and transitions                   | requires `AnimationController` and state updates         |
| Asynchronous data loading                    | fetching data and updating UI on completion              |
| Dynamic layouts                              | elements that show/hide based on state                   |

---

## 6. Comparison with StatelessWidget

| Feature                   | StatelessWidget                 | StatefulWidget                         |
|---------------------------|---------------------------------|----------------------------------------|
| Mutability                | Immutable                       | Maintains mutable state                |
| Lifecycle                 | Only `build`                    | Full lifecycle (`initState`, `dispose`, etc.) |
| Use Case                  | Static content                  | Interactive and dynamic content        |

---

## 7. State Management Tips

- Keep `setState` calls minimal and localized.
- Extract sub-widgets to reduce rebuild scope.
- Use `StatefulWidget` sparingly; prefer external state management for complex apps.
- Consider using `Provider`, `Riverpod`, `Bloc`, or other packages for scalable state management.

---

## 8. Best Practices

- **Dispose resources**: cancel timers, controllers in `dispose()`.
- **Avoid heavy work in `build`**: move long-running tasks to `initState` or separate async functions.
- **Use `const`** for widgets without state or in subtrees unaffected by state changes.
- **Name state classes** consistently: use `_<WidgetName>State`.

---

## 9. Further Reading

- [StatefulWidget API Documentation](https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html)
- [Flutter Lifecycle](https://flutter.dev/docs/perf/app-lifecycle)
- [Managing State in Flutter](https://flutter.dev/docs/development/data-and-backend/state-mgmt)

---

*End of Flutter StatefulWidget*

