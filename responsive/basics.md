# Flutter Responsive Design

Learn how to make your Flutter applications look great on devices of all sizes!

---

## 1. What is Responsive Design?

Responsive design ensures your app UI adapts and looks good across a wide range of screen sizes, from small phones to large tablets and desktops.

---

## 2. MediaQuery

**MediaQuery** provides information about the size and orientation of the current device.

### Example

```dart
import 'package:flutter/material.dart';

class ResponsiveExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final size = MediaQuery.of(context).size;

    return Scaffold(
      body: Center(
        child: Text('Screen width: ${size.width}'),
      ),
    );
  }
}
```

---

## 3. LayoutBuilder

**LayoutBuilder** gives the dimensions of the parent widget and allows you to adapt based on available space.

### Example

```dart
class ResponsiveBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(
      builder: (context, constraints) {
        if (constraints.maxWidth > 600) {
          return Text('Large screen');
        } else {
          return Text('Small screen');
        }
      },
    );
  }
}
```

---

## 4. OrientationBuilder

**OrientationBuilder** lets you adjust the UI based on the device's orientation (portrait or landscape).

### Example

```dart
class ResponsiveOrientation extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return OrientationBuilder(
      builder: (context, orientation) {
        return GridView.count(
          crossAxisCount: orientation == Orientation.portrait ? 2 : 4,
          children: List.generate(8, (index) => Card(color: Colors.blue)),
        );
      },
    );
  }
}
```

---

## 5. Responsive Packages

To simplify responsive design, you can use external packages:

### Popular Packages

- **responsive_builder**
- **flutter_screenutil**
- **sizer**

### Example with flutter_screenutil

First, add it to your `pubspec.yaml`:

```yaml
dependencies:
  flutter_screenutil: ^5.8.4
```

Initialize it in your app:

```dart
import 'package:flutter_screenutil/flutter_screenutil.dart';

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ScreenUtilInit(
      designSize: Size(360, 690),
      builder: (context, child) {
        return MaterialApp(
          home: HomeScreen(),
        );
      },
    );
  }
}
```

Use it like this:

```dart
Text(
  'Hello',
  style: TextStyle(fontSize: 20.sp), // scalable text
)
```

---

## 6. Best Practices

- Always test your app on different screen sizes.
- Use flexible layouts like `Expanded`, `Flexible`, `Wrap`, `GridView`.
- Avoid hardcoded sizes and margins.
- Prefer relative units (e.g., percentages, MediaQuery, ScreenUtil).
- Handle both portrait and landscape modes.

---

## 7. Example: Fully Responsive Page

```dart
class ResponsivePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final size = MediaQuery.of(context).size;

    return Scaffold(
      appBar: AppBar(title: Text('Responsive Example')),
      body: Center(
        child: Container(
          width: size.width * 0.8,
          height: size.height * 0.3,
          color: Colors.amber,
          child: Center(child: Text('Responsive Box')),
        ),
      ),
    );
  }
}
```

---

## 8. Useful Links

- [Flutter Responsive UI Guide](https://docs.flutter.dev/development/ui/layout/responsive)
- [flutter_screenutil package](https://pub.dev/packages/flutter_screenutil)

---

*End of Flutter Responsive Design*

