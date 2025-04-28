# Flutter Curved Animations

This document covers how to create smooth, visually appealing animations in Flutter using the `CurvedAnimation` class, built-in curve presets, and custom curves.

---

## 1. Introduction to Curved Animations

- A **curved animation** applies a non-linear curve to a linear animation (0.0 to 1.0) for more natural motion.
- Flutter provides a variety of predefined curves in the `Curves` class (e.g., `easeIn`, `bounceOut`).
- Use `CurvedAnimation` in combination with an `AnimationController` and `Tween` to drive animated values.

---

## 2. Key Classes

| Class                  | Purpose                                                          |
|------------------------|------------------------------------------------------------------|
| `AnimationController`  | Manages the animation duration and state (forward, reverse, etc.)|
| `CurvedAnimation`      | Applies a non-linear curve to a parent `Animation<double>`.      |
| `Tween<T>`             | Defines how to interpolate between beginning and end values.     |
| `AnimatedBuilder`      | Rebuilds widgets whenever the animation changes.                 |

---

## 3. Simple Example

```dart
import 'package:flutter/material.dart';

class CurvedAnimationDemo extends StatefulWidget {
  const CurvedAnimationDemo({Key? key}) : super(key: key);

  @override
  _CurvedAnimationDemoState createState() => _CurvedAnimationDemoState();
}

class _CurvedAnimationDemoState extends State<CurvedAnimationDemo> with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      vsync: this,
      duration: const Duration(seconds: 2),
    );

    _animation = CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    );

    _controller.repeat(reverse: true);
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        return Transform.scale(
          scale: _animation.value * 0.5 + 0.5,
          child: child,
        );
      },
      child: const FlutterLogo(size: 100),
    );
  }
}
```

- The `AnimationController` drives a value from 0.0 → 1.0 over 2 seconds.
- `CurvedAnimation` applies `easeInOut` for smooth start and end.
- `repeat(reverse: true)` continuously animates back and forth.

---

## 4. Common Built-in Curves

| Curve Name               | Description                                    |
|--------------------------|------------------------------------------------|
| `Curves.linear`          | No easing; constant speed.                     |
| `Curves.easeIn`          | Starts slowly, accelerates.                    |
| `Curves.easeOut`         | Starts quickly, decelerates.                   |
| `Curves.easeInOut`       | Slow start and end with faster middle.         |
| `Curves.bounceOut`       | Bounces at the end.                            |
| `Curves.elasticInOut`    | Elastic effect at start and end.               |

Use any curve by passing to `CurvedAnimation(curve: Curves.<name>)`.

---

## 5. Custom Curves

Implement your own by extending `Curve`:

```dart\ nclass SineCurve extends Curve {
  @override
  double transform(double t) {
    return math.sin(t * math.pi);
  }
}

// Usage:
_animation = CurvedAnimation(
  parent: _controller,
  curve: SineCurve(),
);
```

---

## 6. Tweening Values

Combine `Tween<T>` with a curved animation:

```dart
_animation = Tween<double>(begin: 0, end: 300).animate(
  CurvedAnimation(
    parent: _controller,
    curve: Curves.easeIn,
  ),
);

// Then use AnimatedBuilder to animate widget position:
AnimatedBuilder(
  animation: _animation,
  builder: (context, child) {
    return Positioned(
      top: _animation.value,
      child: child!,
    );
  },
  child: const Icon(Icons.arrow_upward),
)
```

---

## 7. Animation Widgets

- **`AnimatedContainer`**: implicitly animates changes to decoration, padding, etc.
- **`AnimatedOpacity`**: animates opacity.
- **`AnimatedPositioned`**: animates position within a `Stack`.

```dart
AnimatedContainer(
  duration: const Duration(seconds: 1),
  curve: Curves.easeInOut,
  width: _expanded ? 200 : 100,
  height: _expanded ? 200 : 100,
  color: Colors.blue,
)
```

---

## 8. Best Practices

- **Dispose controllers**: always call `dispose()`.
- **Reuse animations**: use `AnimatedBuilder` instead of `setState`.
- **Use implicit animations** when possible for simplicity.
- **Limit rebuild scope**: separate animated widgets from static children.
- **Test performance**: use the Flutter performance overlay and DevTools.

---

## 9. Further Reading

- [Flutter Animations Tutorial](https://flutter.dev/docs/development/ui/animations)
- [Animation Library API](https://api.flutter.dev/flutter/animation/animation-library.html)

---

*End of Flutter Curved Animations*

