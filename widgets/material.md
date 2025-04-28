# Flutter Material Widgets

This document provides an overview of the key Material Design widgets in Flutter. It covers structure, theming, common components, layout helpers, and usage examples.

---

## 1. Introduction to Material Design in Flutter

- Flutter’s Material library implements Google’s Material Design guidelines.
- Provides a consistent look-and-feel across Android, web, and desktop platforms.
- Use `MaterialApp` as the root widget to access Material theming and components.

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
      title: 'Material Widgets Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const HomePage(),
    );
  }
}
```

---

## 2. App Structure

### 2.1 Scaffold

Provides the basic visual layout structure: app bar, body, floating action button, drawer, bottom navigation.

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Home')),
      body: const Center(child: Text('Hello, Material!')),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

### 2.2 AppBar

Top bar with title, navigation, and actions.

```dart
AppBar(
  title: const Text('Title'),
  leading: const Icon(Icons.menu),
  actions: [
    IconButton(
      icon: const Icon(Icons.search),
      onPressed: () {},
    ),
  ],
)
```

---

## 3. Common Material Widgets

### 3.1 Text

Styled using `TextStyle` and theme.

```dart
const Text(
  'Material Text',
  style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
)
```

### 3.2 Icons

```dart
const Icon(
  Icons.favorite,
  color: Colors.red,
  size: 32,
)
```

### 3.3 Buttons

- **ElevatedButton** (raised button)
- **TextButton** (flat)
- **OutlinedButton** (bordered)

```dart
ElevatedButton(
  onPressed: () {},
  child: const Text('Elevated'),
)

TextButton(
  onPressed: () {},
  child: const Text('Text'),
)

OutlinedButton(
  onPressed: () {},
  child: const Text('Outlined'),
)
```

### 3.4 FloatingActionButton

Circular button for primary actions.

```dart
FloatingActionButton(
  onPressed: () {},
  child: const Icon(Icons.navigation),
)
```

### 3.5 Cards

Container with rounded corners and shadow.

```dart
Card(
  elevation: 4,
  shape: RoundedRectangleBorder(
    borderRadius: BorderRadius.circular(8),
  ),
  child: Padding(
    padding: const EdgeInsets.all(16),
    child: const Text('This is a card'),
  ),
)
```

### 3.6 ListTile

Standard list item with leading/trailing widgets.

```dart
ListTile(
  leading: const Icon(Icons.album),
  title: const Text('Song Title'),
  subtitle: const Text('Artist'),
  trailing: const Icon(Icons.more_vert),
  onTap: () {},
)
```

---

## 4. Layout Helpers

### 4.1 Padding & Margin

Use `Padding` and `SizedBox` for spacing.

```dart
Padding(
  padding: const EdgeInsets.all(8),
  child: const Text('Padded text'),
)

SizedBox(height: 16),
```

### 4.2 Rows & Columns

Horizontal and vertical layouts.

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  children: [Text('Left'), Text('Right')],
)

Column(
  crossAxisAlignment: CrossAxisAlignment.start,
  children: [Text('Top'), Text('Bottom')],
)
```

### 4.3 Expanded & Flexible

Control how children fill available space.

```dart
Row(
  children: [
    Expanded(child: Container(color: Colors.red)),
    Flexible(child: Container(color: Colors.blue, width: 100)),
  ],
)
```

### 4.4 ListView & GridView

Scrollable lists and grids.

```dart
ListView.builder(
  itemCount: 20,
  itemBuilder: (context, index) => ListTile(title: Text('Item \$index')),
)

GridView.count(
  crossAxisCount: 3,
  children: List.generate(9, (i) => Card(child: Center(child: Text('Tile \$i')))),
)
```

---

## 5. Input Widgets

### 5.1 TextField

```dart
TextField(
  decoration: const InputDecoration(
    border: OutlineInputBorder(),
    labelText: 'Enter text',
  ),
  onChanged: (value) {},
)
```

### 5.2 Checkbox, Switch, Radio

```dart
Checkbox(
  value: isChecked,
  onChanged: (val) {},
)

Switch(
  value: isOn,
  onChanged: (val) {},
)

Radio<int>(
  value: optionValue,
  groupValue: selectedValue,
  onChanged: (val) {},
)
```

---

## 6. Theming & Customization

- Use `ThemeData` in `MaterialApp` to define colors, typography, and component themes.

```dart
MaterialApp(
  theme: ThemeData(
    primaryColor: Colors.teal,
    textTheme: const TextTheme(bodyText2: TextStyle(fontSize: 16)),
    elevatedButtonTheme: ElevatedButtonThemeData(
      style: ElevatedButton.styleFrom(
        shape: const StadiumBorder(),
      ),
    ),
  ),
)
```

- Access theme in widgets:

```dart
Text(
  'Themed text',
  style: Theme.of(context).textTheme.bodyText2,
)
```

---

## 7. Further Reading

- [Material Components Widgets](https://flutter.dev/docs/development/ui/widgets/material)
- [Material Theming Guide](https://flutter.dev/docs/catalog/samples/material-components)

---

*End of Flutter Material Widgets*

