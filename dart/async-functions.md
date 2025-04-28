# Dart Async Functions

This document explains how to write, use, and manage asynchronous functions in Dart, including `async`/`await`, `Future` APIs, error handling, generator-based streams, and concurrency patterns.

---

## 1. Introduction to Futures

A **Future** represents a computation that will complete at some point in the future, yielding a value or an error:

```dart
Future<String> fetchUsername() {
  return Future.delayed(Duration(seconds: 2), () => 'Alice');
}
```

Callers can use the Future’s API:

```dart
fetchUsername().then((name) {
  print('User: \$name');
}).catchError((error) {
  print('Error: \$error');
});
```

---

## 2. `async` and `await`

Mark functions as `async` to use `await` for clearer, linear code:

```dart
Future<void> greetUser() async {
  try {
    var name = await fetchUsername();
    print('Hello, \$name!');
  } catch (e) {
    print('Failed to fetch username: \$e');
  }
}
```

- **`async`** makes the function return a `Future`.
- **`await`** pauses execution until the awaited Future completes.

---

## 3. Return Types for Async Functions

- `Future<T>` when returning a value of type `T`.
- `Future<void>` or `Future` for no return value.
- `FutureOr<T>` allows returning either `T` immediately or a `Future<T>`.

```dart
FutureOr<int> maybeCompute(bool async) {
  if (async) {
    return Future.delayed(Duration(seconds: 1), () => 42);
  } else {
    return 42;
  }
}
```

---

## 4. Error Handling

### 4.1 `try`/`catch`

```dart
Future<void> loadData() async {
  try {
    var data = await fetchData();
    print(data);
  } on FormatException catch (e) {
    print('Format error: \$e');
  } catch (e) {
    print('Unknown error: \$e');
  }
}
```

### 4.2 Future API

```dart
fetchData()
  .then((data) => print(data))
  .catchError((e) => print('Error: \$e'))
  .whenComplete(() => print('Done'));
```

---

## 5. Parallel and Sequential Execution

### 5.1 Sequential

```dart
var a = await taskA();
var b = await taskB();
```

### 5.2 Parallel with `Future.wait`

```dart
var results = await Future.wait([taskA(), taskB()]);
print(results); // [resultA, resultB]
```

### 5.3 Racing with `Future.any` and `Future.race`

```dart
var first = await Future.any([taskA(), taskB()]);
```

---

## 6. Timeouts and Cancellation

- **Timeout**:

  ```dart
  try {
    var result = await longTask().timeout(Duration(seconds: 5));
  } on TimeoutException {
    print('Operation timed out');
  }
  ```

- **Cancellation**: Dart Futures cannot be cancelled directly; use flags or `Isolate` control for cancellable work.

---

## 7. Generator Functions and Streams

### 7.1 `async*` and `yield`

Produce a sequence of values as a `Stream`:

```dart
Stream<int> countStream(int to) async* {
  for (var i = 1; i <= to; i++) {
    await Future.delayed(Duration(milliseconds: 500));
    yield i;
  }
}

void listen() {
  countStream(3).listen((value) => print(value));
}
```

### 7.2 Combining Streams

```dart
Stream.zip([streamA, streamB], (a, b) => a + b);
```

---

## 8. Best Practices

- **Prefer `async`/`await`** for readability over nested `.then()` chains.
- **Limit parallel tasks** to avoid resource exhaustion.
- **Handle errors** at the appropriate level; use `whenComplete` for cleanup.
- **Use `compute`** for CPU-bound tasks off the main isolate in Flutter.

---

## 9. Further Reading

- [Dart Asynchronous Programming](https://dart.dev/codelabs/async-await)
- [Streams in Dart](https://dart.dev/tutorials/language/streams)

---

*End of Dart Async Functions*

