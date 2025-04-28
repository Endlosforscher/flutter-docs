# Flutter Assets Management

This document covers how to manage and use assets in Flutter projects, including images, fonts, JSON files, and other resources. It explains configuration in `pubspec.yaml`, loading assets at runtime, best practices, and practical examples.

---

## 1. Defining Assets in `pubspec.yaml`

Add an `assets` section under the `flutter` key:

```yaml
flutter:
  assets:
    - assets/images/logo.png
    - assets/images/
    - assets/data/sample.json
  fonts:
    - family: Roboto
      fonts:
        - asset: assets/fonts/Roboto-Regular.ttf
        - asset: assets/fonts/Roboto-Bold.ttf
          weight: 700
```

- **Single file**: specify the full path.
- **Directory**: include all files under the directory.
- **Fonts**: declare font families and weights.

After editing, run:
```bash
flutter pub get
```

---

## 2. Loading Image Assets

### 2.1 Using `Image.asset`

```dart
Image.asset(
  'assets/images/logo.png',
  width: 120,
  height: 120,
  fit: BoxFit.cover,
)
```

### 2.2 As Decoration

```dart
Container(
  decoration: BoxDecoration(
    image: DecorationImage(
      image: AssetImage('assets/images/background.jpg'),
      fit: BoxFit.fill,
    ),
  ),
)
```

### 2.3 Precaching

```dart
@override
void initState() {
  super.initState();
  precacheImage(AssetImage('assets/images/logo.png'), context);
}
```

---

## 3. Loading Other Assets

### 3.1 Text and JSON Files

Use `rootBundle` from `package:flutter/services.dart`:

```dart
import 'package:flutter/services.dart' show rootBundle;

Future<String> loadJson() async {
  return await rootBundle.loadString('assets/data/sample.json');
}
```

Parsing JSON:

```dart
import 'dart:convert';

Future<MyData> fetchData() async {
  final jsonString = await loadJson();
  final Map<String, dynamic> jsonMap = json.decode(jsonString);
  return MyData.fromJson(jsonMap);
}
```

### 3.2 Binary Assets

Load bytes:

```dart
Future<Uint8List> loadBytes() async {
  final data = await rootBundle.load('assets/data/sample.bin');
  return data.buffer.asUint8List();
}
```

---

## 4. Fonts and Icon Packs

### 4.1 Custom Fonts

Declare in `pubspec.yaml` (see Section 1), then use:

```dart
Text(
  'Custom Font',
  style: TextStyle(fontFamily: 'Roboto', fontWeight: FontWeight.w700),
)
```

### 4.2 Icon Fonts

Use packages like `flutter_icon_font` or include a `.ttf` icons file:

```yaml
fonts:
  - family: CustomIcons
    fonts:
      - asset: assets/fonts/custom_icons.ttf
```

Map code points:

```dart
class CustomIcons {
  CustomIcons._();
  static const _kFontFam = 'CustomIcons';

  static const IconData thumbsUp = IconData(0xe800, fontFamily: _kFontFam);
}
```

Use in code:

```dart
Icon(CustomIcons.thumbsUp)
```

---

## 5. AssetBundle and Alternative Bundles

- **Default**: `rootBundle`.
- **Custom**: load assets from network or packaged bundles.

```dart
var bundle = DefaultAssetBundle.of(context);
String text = await bundle.loadString('assets/data/config.txt');
```

---

## 6. Best Practices

- **Organize** assets in meaningful directories (`images/`, `fonts/`, `data/`).
- **Optimize** images for size (use WebP or compressed PNGs).
- **Precache** large assets to improve UX.
- **Use appropriate formats**: SVG for icons (via `flutter_svg`), PNG/WebP for photos.
- **Minimize asset bundle size**: remove unused assets.
- **Leverage asset variants**: provide different resolutions (`2.0x/`, `3.0x/`) for high-DPI screens.

```yaml
flutter:
  assets:
    - assets/images/logo.png
    - assets/images/2.0x/logo.png
    - assets/images/3.0x/logo.png
```

---

## 7. Handling Asset Changes

- After adding/removing assets, always run:
  ```bash
  flutter pub get
  flutter clean
  flutter run
  ```
- Hot reload may not pick up new assets; perform full restart.

---

## 8. Further Reading

- [Flutter Asset Management](https://flutter.dev/docs/development/ui/assets-and-images)
- [Using Packages & Plugins](https://flutter.dev/docs/development/packages-and-plugins/using-packages)
- [Font & Icon Guide](https://flutter.dev/docs/cookbook/design/fonts)

---

*End of Flutter Assets Management*

