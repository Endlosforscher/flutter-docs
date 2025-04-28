# Dart Futures

This document provides a thorough overview of Futures in Dart—objects representing asynchronous computations—and how to work with them effectively. It covers creation, handling results and errors, chaining, timeouts, cancellation patterns, and practical examples.

---

## 1. What is a Future?

- A **Future<T>** represents a computation that may complete later with a value of type `T` or an error.
- Futures are single-assignment: once a value or error is set, it cannot change.

```dart
Future<String> fetchUsername() {
  // Simulate network delay
  return Future.delayed(Duration(seconds: 2), () => 'Alice');
}
```

---

## 2. Creating Futures

### 2.1 Using `Future` constructors

```dart
// Delayed future\ Future<int> delayedValue = Future.delayed(
  Duration(seconds: 1),
  () => 42,
);

// Immediate future with value
Future<String> immediate = Future.value('Hello');

// Immediate future with error
Future<void> errorFuture = Future.error('Something went wrong');
```

### 2.2 Asynchronous functions (`async`)

```dart
Future<double> computeSum(double a, double b) async {
  return a + b; // implicit Future
}
```

---

## 3. Handling Results

### 3.1 `.then()` and `.catchError()`

```dart
fetchUsername()
  .then((name) {
    print('User: \$name');
  })
  .catchError((error) {
    print('Error: \$error');
  })
  .whenComplete(() {
    print('Done fetching username');
  });
```

- **`.then`**: callback on success
- **`.catchError`**: callback on error
- **`.whenComplete`**: always runs when done

### 3.2 `async`/`await`

```dart
Future<void> showUser() async {
  try {
    var user = await fetchUsername();
    print('Hello, \$user');
  } catch (e) {
    print('Failed to fetch: \$e');
  } finally {
    print('Operation complete');
  }
}
```

---

## 4. Chaining Futures

Combine sequential async operations:

```dart
Future<void> chained() {
  return fetchUsername()
    .then((name) => loadUserProfile(name))
    .then((profile) => print('Profile loaded: \$profile'))
    .catchError((e) => print('Error in chain: \$e'));
}
```

With `async`:

```dart
Future<void> chainedAsync() async {
  try {
    var name = await fetchUsername();
    var profile = await loadUserProfile(name);
    print('Profile loaded: \$profile');
  } catch (e) {
    print('Error in chain: \$e');
  }
}
```

---

## 5. Parallel Execution

### 5.1 `Future.wait`

Wait for multiple futures:

```dart
var futures = [fetchA(), fetchB(), fetchC()];
var results = await Future.wait(futures);
print(results); // [resultA, resultB, resultC]
```

### 5.2 `Future.any`

Get first completed:

```dart
var first = await Future.any([fetchA(), fetchB()]);
print('First result: \$first');
```

---

## 6. Timeouts and Retries

### 6.1 Timeout

```dart
try {
  var value = await longOperation().timeout(Duration(seconds: 5));
} on TimeoutException {
  print('Operation timed out');
}
```

### 6.2 Retry Pattern

```dart
Future<T> retry<T>(Future<T> Function() action, int retries) async {
  for (var i = 0; i < retries; i++) {
    try {
      return await action();
    } catch (_) {
      if (i == retries - 1) rethrow;
    }
  }
  throw Exception('Unreachable');
}
```

---

## 7. Cancellation Patterns

Futures cannot be cancelled directly. Common approaches:

1. **Use `bool` flag** to ignore results:
   ```dart
   bool _cancelled = false;

   Future<void> task() async {
     var result = await longTask();
     if (!_cancelled) handleResult(result);
   }

   void cancel() {
     _cancelled = true;
   }
   ```

2. **Use `Isolate`** for heavy work and terminate isolate.

---

## 8. Practical Examples

### 8.1 HTTP Request

```dart
import 'package:http/http.dart' as http;

Future<String> fetchData(String url) async {
  var response = await http.get(Uri.parse(url));
  if (response.statusCode == 200) {
    return response.body;
  } else {
    throw Exception('Failed to load data');
  }
}
```

### 8.2 File Read

```dart
import 'dart:io';

Future<String> readFile(String path) async {
  var file = File(path);
  return file.readAsString();
}
```

---

## 9. Best Practices

- Prefer `async`/`await` for clarity.
- Always handle errors with `try`/`catch`.
- Use `Future.wait` judiciously to avoid flooding resources.
- Avoid unhandled futures: await or attach handlers.
- Encapsulate retry logic.

---

## 10. Further Reading

- [Dart Asynchronous Programming](https://dart.dev/codelabs/async-await)
- [Effective Dart: Asynchrony](https://dart.dev/guides/language/effective-dart/usage#asynchrony)

---

*End of Dart Futures*

