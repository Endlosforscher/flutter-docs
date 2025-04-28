# Flutter AnimationController

This document provides a comprehensive reference for the **AnimationController**, the core class for driving animations in Flutter.

---

## 1. Introduction

An **AnimationController** manages the timing of an animation. It produces values between a lower and upper bound over a given duration, and exposes methods to start, stop, reverse, and repeat the animation.

Key points:
- Drives values from `lowerBound` to `upperBound` (default 0.0 → 1.0).
- Requires a `TickerProvider` (usually a `State` with `SingleTickerProviderStateMixin`).
- Notifies listeners on each animation tick and on status changes.

---

## 2. Constructor

```dart
AnimationController({
  required TickerProvider vsync,
  Duration duration = const Duration(milliseconds: 300),
  double? lowerBound,
  double? upperBound,
  String? debugLabel,
})
```

- **vsync**: the ticker provider that drives the animation; prevents offscreen animations from consuming resources.
- **duration**: time taken to run forward from `lowerBound` to `upperBound`.
- **lowerBound** and **upperBound**: define the range of output values (default 0.0 and 1.0).
- **debugLabel**: optional identifier for debugging.

---

## 3. Core Methods

| Method                                     | Description                                                      |
|--------------------------------------------|------------------------------------------------------------------|
| `forward({ double? from })`                | Starts animation forward. Optionally begins at `from` value.     |
| `reverse({ double? from })`                | Runs animation in reverse. Optionally begins at `from` value.    |
| `repeat({ bool reverse = false, Duration? period })` | Continuously repeats animation; reverses if `reverse` is true.    |
| `stop({ bool canceled = true })`           | Stops the animation; if `canceled` is false, completes current cycle. |
| `reset()`                                  | Resets the controller’s value to `lowerBound`.                   |

---

## 4. Listeners and Status

- **addListener(VoidCallback listener)**
  - Invoked on every frame with updated `value`.
- **removeListener(...)**
  - Remove a previously added listener.
- **addStatusListener(AnimationStatusListener listener)**
  - Invoked when the animation status changes: `dismissed`, `forward`, `reverse`, `completed`.
- **removeStatusListener(...)**
  - Remove a status listener.

```dart
_controller.addListener(() {
  print('Value: \${_controller.value}');
});

_controller.addStatusListener((status) {
  if (status == AnimationStatus.completed) {
    print('Animation completed');
  }
});
```

---

## 5. Example Usage

```dart
class MyAnimatedWidget extends StatefulWidget {
  @override
  _MyAnimatedWidgetState createState() => _MyAnimatedWidgetState();
}

class _MyAnimatedWidgetState extends State<MyAnimatedWidget>
    with SingleTickerProviderStateMixin {
  late final AnimationController _controller;
  late final Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      vsync: this,
      duration: const Duration(seconds: 2),
    );

    // Example: animate opacity
    _animation = _controller.drive(
      Tween<double>(begin: 0.0, end: 1.0),
    );

    _controller.forward();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return FadeTransition(
      opacity: _animation,
      child: const FlutterLogo(size: 100),
    );
  }
}
```

---

## 6. Best Practices

- **Dispose** controllers in `dispose()` to avoid memory leaks.
- **Use `TickerProviderStateMixin`** (or `SingleTickerProviderStateMixin`) for the `State` class.
- **Limit controller lifespan** to the widget’s lifecycle.
- **Use `drive` or `animateWith`** to combine with `Tween` for type safety.

---

## 7. Further Reading

- [AnimationController API](https://api.flutter.dev/flutter/animation/AnimationController-class.html)
- [Flutter Animations Guide](https://flutter.dev/docs/development/ui/animations)

---

*End of Flutter AnimationController*

