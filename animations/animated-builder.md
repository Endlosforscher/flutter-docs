# Flutter AnimatedBuilder

This document explains how to use Flutter’s **AnimatedBuilder** widget to create efficient, composable animations by separating the animation logic from widget rebuilding.

---

## 1. What is AnimatedBuilder?

- `AnimatedBuilder` is a widget that rebuilds its child whenever an animation changes, without requiring manual `setState` calls.
- It helps optimize performance by only rebuilding the parts of the widget tree that depend on the animation value.
- Ideal for complex animations where only a subset of child widgets need updating.

---

## 2. Constructor

```dart
AnimatedBuilder({
  Key? key,
  required Listenable animation,
  required Widget Function(BuildContext, Widget?) builder,
  Widget? child,
})
```

- **animation**: a `Listenable`, typically an `Animation<double>` or `AnimationController`.
- **builder**: function called every tick, receives context and optional static `child`.
- **child**: widget subtree that remains static and is passed back to `builder` for efficiency.

---

## 3. Basic Usage Example

```dart
class AnimatedBuilderDemo extends StatefulWidget {
  @override
  _AnimatedBuilderDemoState createState() => _AnimatedBuilderDemoState();
}

class _AnimatedBuilderDemoState extends State<AnimatedBuilderDemo>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      vsync: this,
      duration: const Duration(seconds: 2),
    )..repeat(reverse: true);

    _animation = CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    );
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Center(
      child: AnimatedBuilder(
        animation: _animation,
        child: const FlutterLogo(size: 100),
        builder: (context, child) {
          return Transform.rotate(
            angle: _animation.value * 2 * math.pi,
            child: child,
          );
        },
      ),
    );
  }
}
```

- **child**: `FlutterLogo` is created once and not rebuilt, improving performance.
- **builder**: applies rotation transform based on animation value.

---

## 4. Complex Example: Fading and Scaling

```dart
class FadeScaleWidget extends StatefulWidget {
  @override
  _FadeScaleWidgetState createState() => _FadeScaleWidgetState();
}

class _FadeScaleWidgetState extends State<FadeScaleWidget>
    with SingleTickerProviderStateMixin {
  late AnimationController _ctrl;
  late Animation<double> _fade;
  late Animation<double> _scale;

  @override
  void initState() {
    super.initState();
    _ctrl = AnimationController(
      vsync: this,
      duration: const Duration(seconds: 3),
    )..forward();

    _fade = Tween(begin: 0.0, end: 1.0).animate(
      CurvedAnimation(parent: _ctrl, curve: Curves.easeIn),
    );
    _scale = Tween(begin: 0.5, end: 1.5).animate(
      CurvedAnimation(parent: _ctrl, curve: Curves.elasticOut),
    );
  }

  @override
  void dispose() {
    _ctrl.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _ctrl,
      builder: (context, child) {
        return Opacity(
          opacity: _fade.value,
          child: Transform.scale(
            scale: _scale.value,
            child: child,
          ),
        );
      },
      child: const Card(
        child: Padding(
          padding: EdgeInsets.all(16),
          child: Text('Hello AnimatedBuilder'),
        ),
      ),
    );
  }
}
```

---

## 5. Benefits of Using AnimatedBuilder

- **Performance**: isolates static children to avoid unnecessary rebuilds.
- **Flexibility**: supports multiple animations by combining `animation` values.
- **Clean code**: separates animation logic in the builder callback.

---

## 6. Tips and Best Practices

- Pass static parts of the widget tree as `child` to `AnimatedBuilder`.
- Avoid heavy work inside the `builder`; keep it lean for 60fps.
- Dispose of controllers in `dispose()` to prevent memory leaks.
- Use `Listenable.merge([anim1, anim2])` to listen to multiple animations.

---

## 7. Alternatives

- **Implicit animations** (`AnimatedContainer`, `AnimatedOpacity`, etc.) for simple UI changes.
- **AnimatedWidget**: subclass that holds animation and calls `build` on value change.
- **ValueListenableBuilder**: for non-animation listenables like `ValueNotifier`.

---

## 8. Further Reading

- [AnimatedBuilder API](https://api.flutter.dev/flutter/widgets/AnimatedBuilder-class.html)
- [Flutter Animations Guide](https://flutter.dev/docs/development/ui/animations)

---

*End of Flutter AnimatedBuilder*

