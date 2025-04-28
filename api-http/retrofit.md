# Flutter Retrofit

This document explains how to use **Retrofit** in Flutter for structured and type-safe HTTP networking.

---

## 1. What is Retrofit?

- Retrofit is a type-safe HTTP client inspired by the Retrofit library from Android.
- It simplifies HTTP API integration in Flutter applications.
- Works with **Dio** HTTP client for network requests.

---

## 2. Required Packages

Add these dependencies to your `pubspec.yaml`:

```yaml
dependencies:
  dio: ^5.0.0
  retrofit: ^4.0.0
  json_annotation: ^4.8.0

dev_dependencies:
  retrofit_generator: ^5.0.0
  build_runner: ^2.4.0
  json_serializable: ^6.7.0
```

---

## 3. Basic Setup

### 3.1. Create a Model

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

Then run:

```bash
flutter pub run build_runner build
```

### 3.2. Define API Service

```dart
import 'package:retrofit/retrofit.dart';
import 'package:dio/dio.dart';
import 'user.dart';

part 'rest_client.g.dart';

@RestApi(baseUrl: "https://jsonplaceholder.typicode.com")
abstract class RestClient {
  factory RestClient(Dio dio, {String baseUrl}) = _RestClient;

  @GET("/users/{id}")
  Future<User> getUser(@Path("id") int id);

  @GET("/users")
  Future<List<User>> getUsers();
}
```

Then generate code:

```bash
flutter pub run build_runner build
```

### 3.3. Use the API

```dart
final dio = Dio();
final client = RestClient(dio);

void fetchUser() async {
  User user = await client.getUser(1);
  print(user.name);
}
```

---

## 4. Handling Headers and Queries

### Add Custom Headers

```dart
@GET("/users")
Future<List<User>> getUsers(@Header("Authorization") String token);
```

### Query Parameters

```dart
@GET("/users")
Future<List<User>> searchUsers(@Query("search") String query);
```

---

## 5. Uploading Files

```dart
@POST("/upload")
@MultiPart()
Future<void> uploadFile(@Part(name: "file") File file);
```

---

## 6. Error Handling

Use Dio's built-in interceptors and try-catch blocks to manage network errors:

```dart
try {
  final user = await client.getUser(1);
} catch (e) {
  if (e is DioException) {
    print('Network error: ${e.message}');
  }
}
```

---

## 7. Best Practices

- Separate API classes and models clearly.
- Always handle timeouts and server errors.
- Use interceptors for authentication and logging.
- Regenerate `.g.dart` files when modifying models or endpoints.

---

## 8. Useful Links

- [Retrofit Flutter Pub](https://pub.dev/packages/retrofit)
- [Dio Flutter Pub](https://pub.dev/packages/dio)
- [Json Serializable Pub](https://pub.dev/packages/json_serializable)

---

*End of Flutter Retrofit*

