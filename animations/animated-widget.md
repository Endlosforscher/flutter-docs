# Flutter AnimatedWidget

This document explains how to use Flutter's **AnimatedWidget** class to simplify animation logic by creating widgets that listen to animation changes directly.

---

## 1. What is AnimatedWidget?

- `AnimatedWidget` is a base class for widgets that perform animations.
- It automatically listens to an `Animation` and rebuilds when the animation value changes.
- Reduces boilerplate code by avoiding the need for manual `addListener` and `setState` calls.

---

## 2. Constructor

```dart
AnimatedWidget({
  Key? key,
  required Listenable listenable,
})
```

- **listenable**: usually an `Animation<double>` or an `AnimationController`.
- The widget automatically rebuilds when the animation changes.

---

## 3. Basic Example

```dart
class RotationTransitionWidget extends AnimatedWidget {
  const RotationTransitionWidget({Key? key, required Animation<double> animation})
      : super(key: key, listenable: animation);

  @override
  Widget build(BuildContext context) {
    final animation = listenable as Animation<double>;

    return Transform.rotate(
      angle: animation.value * 2 * math.pi,
      child: const FlutterLogo(size: 100),
    );
  }
}
```

- The widget extends `AnimatedWidget`.
- `listenable` is cast to the correct animation type inside `build`.

---

## 4. Usage Example with Controller

```dart
class AnimatedWidgetDemo extends StatefulWidget {
  @override
  _AnimatedWidgetDemoState createState() => _AnimatedWidgetDemoState();
}

class _AnimatedWidgetDemoState extends State<AnimatedWidgetDemo>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    )..repeat(reverse: true);
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Center(
      child: RotationTransitionWidget(animation: _controller),
    );
  }
}
```

---

## 5. When to Use AnimatedWidget

- When you want to create a reusable, animated widget.
- When you want to simplify animation logic without manually managing listeners.
- For clean, declarative animation-driven UI components.

---

## 6. Benefits

- **Less boilerplate**: no manual `addListener`/`removeListener` or `setState()`.
- **Cleaner code**: widget reacts to animation changes naturally.
- **Reusable widgets**: easy to encapsulate animation behaviors.

---

## 7. Drawbacks

- Less flexibility compared to `AnimatedBuilder` if you have multiple animations or complex dependencies.
- For very dynamic builds based on several values, `AnimatedBuilder` might be preferred.

---

## 8. Alternatives

- **AnimatedBuilder**: when you need more control and optimize rebuilds with a `child` parameter.
- **Implicit animations**: (`AnimatedContainer`, `AnimatedOpacity`) for simpler UI changes without needing custom animations.

---

## 9. Further Reading

- [AnimatedWidget API](https://api.flutter.dev/flutter/widgets/AnimatedWidget-class.html)
- [Flutter Animations Overview](https://docs.flutter.dev/development/ui/animations)

---

*End of Flutter AnimatedWidget*
