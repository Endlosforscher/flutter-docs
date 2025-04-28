# Flutter JSON Serialization/Deserialization

Learn how to handle **JSON serialization and deserialization** in Flutter to convert JSON data to Dart objects and vice versa.

---

## 1. What is JSON Serialization/Deserialization?

- **Serialization**: Converting Dart objects into JSON format.
- **Deserialization**: Converting JSON data into Dart objects.

---

## 2. Manual Serialization

### Example Dart Model

```dart
class User {
  final int id;
  final String name;

  User({required this.id, required this.name});

  // Deserialize
  factory User.fromJson(Map<String, dynamic> json) => User(
        id: json['id'],
        name: json['name'],
      );

  // Serialize
  Map<String, dynamic> toJson() => {
        'id': id,
        'name': name,
      };
}
```

### Usage

```dart
// From JSON to User
final user = User.fromJson({'id': 1, 'name': 'John'});

// From User to JSON
final userJson = user.toJson();
print(userJson); // Output: {id: 1, name: John}
```

---

## 3. Automatic Code Generation

For large projects, manually writing serialization code can be tedious. Flutter supports automatic code generation using **json_serializable**.

### Installation

Add these dependencies to `pubspec.yaml`:

```yaml
dependencies:
  json_annotation: ^4.8.0

dev_dependencies:
  build_runner: ^2.3.3
  json_serializable: ^6.7.0
```

### Create a Model with Annotations

```dart
import 'package:json_annotation/json_annotation.dart';

part 'user.g.dart';

@JsonSerializable()
class User {
  final int id;
  final String name;

  User({required this.id, required this.name});

  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```

### Generate Code

Run this command:

```bash
flutter pub run build_runner build
```

This will generate a `user.g.dart` file automatically with `_$UserFromJson` and `_$UserToJson` methods.

### Usage

```dart
final user = User.fromJson({'id': 1, 'name': 'John'});
final jsonData = user.toJson();
```

---

## 4. Working with Lists of Objects

### Deserialize a list

```dart
final jsonList = [
  {'id': 1, 'name': 'John'},
  {'id': 2, 'name': 'Jane'},
];

final users = jsonList.map((json) => User.fromJson(json)).toList();
```

### Serialize a list

```dart
final jsonList = users.map((user) => user.toJson()).toList();
```

---

## 5. Custom JSON Key Names

If your JSON fields don't match your Dart field names, you can specify custom key names:

```dart
@JsonSerializable()
class User {
  @JsonKey(name: 'user_id')
  final int id;

  @JsonKey(name: 'user_name')
  final String name;

  User({required this.id, required this.name});

  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```

---

## 6. Error Handling

Always check if JSON fields exist and are of the correct type.

```dart
factory User.fromJson(Map<String, dynamic> json) {
  return User(
    id: json['id'] ?? 0,
    name: json['name'] ?? 'Unknown',
  );
}
```

---

## 7. Best Practices

- Prefer **automatic serialization** for large projects.
- Avoid exposing raw JSON data directly in your UI code.
- Always validate incoming JSON.
- Use strong typing to reduce runtime errors.

---

## 8. Useful Links

- [json_serializable on pub.dev](https://pub.dev/packages/json_serializable)
- [json_annotation on pub.dev](https://pub.dev/packages/json_annotation)

---

*End of Flutter JSON Serialization/Deserialization*

