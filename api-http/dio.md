# Flutter Dio

This document explains how to use **Dio**, a powerful HTTP client for Flutter applications.

---

## 1. What is Dio?

- Dio is a rich HTTP client for Dart and Flutter.
- Supports interceptors, global configuration, FormData, request cancellation, file downloading, and timeout management.

---

## 2. Installation

Add Dio to your `pubspec.yaml`:

```yaml
dependencies:
  dio: ^5.0.0
```

---

## 3. Basic Usage

### Create a Dio Instance

```dart
import 'package:dio/dio.dart';

final dio = Dio();
```

### Making a GET Request

```dart
void fetchUser() async {
  try {
    final response = await dio.get('https://jsonplaceholder.typicode.com/users/1');
    print(response.data);
  } catch (e) {
    print('Error: $e');
  }
}
```

### Making a POST Request

```dart
void createUser() async {
  try {
    final response = await dio.post(
      'https://jsonplaceholder.typicode.com/users',
      data: {'name': 'John Doe', 'email': 'john@example.com'},
    );
    print(response.data);
  } catch (e) {
    print('Error: $e');
  }
}
```

---

## 4. Setting Options

```dart
dio.options.baseUrl = 'https://jsonplaceholder.typicode.com';
dio.options.connectTimeout = Duration(seconds: 5);
dio.options.receiveTimeout = Duration(seconds: 3);
```

---

## 5. Sending Headers

```dart
void fetchWithHeaders() async {
  try {
    final response = await dio.get(
      '/users/1',
      options: Options(headers: {'Authorization': 'Bearer your_token_here'}),
    );
    print(response.data);
  } catch (e) {
    print('Error: $e');
  }
}
```

---

## 6. Interceptors

Dio supports interceptors to log requests/responses or add headers globally.

```dart
dio.interceptors.add(InterceptorsWrapper(
  onRequest: (options, handler) {
    print('Request: ${options.method} ${options.path}');
    return handler.next(options);
  },
  onResponse: (response, handler) {
    print('Response: ${response.statusCode}');
    return handler.next(response);
  },
  onError: (DioException e, handler) {
    print('Error: ${e.message}');
    return handler.next(e);
  },
));
```

---

## 7. Downloading Files

```dart
void downloadFile() async {
  await dio.download(
    'https://example.com/file.zip',
    '/path/to/save/file.zip',
  );
}
```

---

## 8. Uploading Files

```dart
void uploadFile() async {
  FormData formData = FormData.fromMap({
    "file": await MultipartFile.fromFile("/path/to/file.jpg", filename: "file.jpg"),
  });

  final response = await dio.post('/upload', data: formData);
  print(response.data);
}
```

---

## 9. Canceling Requests

```dart
CancelToken cancelToken = CancelToken();

dio.get('/users', cancelToken: cancelToken);

// Cancel the request
cancelToken.cancel("Request canceled");
```

---

## 10. Error Handling

```dart
try {
  final response = await dio.get('/users/1');
} catch (e) {
  if (e is DioException) {
    print('Dio error: ${e.message}');
  }
}
```

---

## 11. Best Practices

- Configure global settings for `Dio` early.
- Use interceptors for authentication tokens.
- Always handle timeouts and errors properly.
- Avoid leaking `CancelToken` references.

---

## 12. Useful Links

- [Dio Pub.dev](https://pub.dev/packages/dio)
- [Dio GitHub Repository](https://github.com/cfug/dio)

---

*End of Flutter Dio*

