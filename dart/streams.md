# Dart Streams

This document provides an in-depth look at Dart Streams: asynchronous sequences of data. It covers types of streams, listening, creating, transforming, error handling, and practical use cases.

---

## 1. What is a Stream?

- A **Stream** delivers a sequence of asynchronous events: data, errors, and a done signal.
- Analogy: an `Iterable` is a synchronous list, while a `Stream` is its asynchronous equivalent.

---

## 2. Types of Streams

| Stream Type             | Characteristics                                  |
|-------------------------|--------------------------------------------------|
| Single-subscription     | Only one listener; suited for data sources like HTTP responses. |
| Broadcast              | Multiple listeners; events are shared; suitable for UI events. |

```dart
// Single-subscription (default)
Stream<int> single = Stream.fromIterable([1, 2, 3]);

// Broadcast
Stream<int> broadcast = Stream.fromIterable([1, 2, 3]).asBroadcastStream();
```

---

## 3. Listening to a Stream

```dart
var controller = StreamController<int>();

// Listener
controller.stream.listen(
  (data) => print('Data: \$data'),
  onError: (err) => print('Error: \$err'),
  onDone: () => print('Done'),
  cancelOnError: false,
);

// Emitting events
controller.add(1);
controller.addError(Exception('Oops'));
controller.close();
```

- **onData**: handles data events
- **onError**: handles errors
- **onDone**: called when stream is closed
- **cancelOnError**: cancel subscription on first error if true

---

## 4. StreamControllers

Create custom streams and control event emission.

```dart
import 'dart:async';

var controller = StreamController<String>();

void produce() {
  controller.sink.add('Hello');
  controller.sink.add('World');
  controller.close();
}

produce();
```

- **sink.add(value)**: emit data
- **sink.addError(error)**: emit error
- **close()**: signal done

Use `StreamController.broadcast()` for broadcast streams.

---

## 5. Transforming Streams

Use `StreamTransformer` or high-level methods:

```dart
// Using map, where, take
var numbers = Stream.fromIterable([1, 2, 3, 4, 5]);

numbers
  .where((n) => n.isEven)
  .map((n) => 'Even: \$n')
  .listen(print);

// Custom transformer
class MultiplyTransformer extends StreamTransformerBase<int, int> {
  final int multiplier;
  MultiplyTransformer(this.multiplier);

  @override
  Stream<int> bind(Stream<int> stream) {
    return stream.map((n) => n * multiplier);
  }
}

numbers.transform(MultiplyTransformer(10)).listen(print);
```

---

## 6. Combining Streams

- **`Stream.zip`** (via rxdart or custom)
- **`StreamGroup.merge`** from `async` package

```dart
import 'package:async/async.dart';

var a = Stream.fromIterable([1, 3, 5]);
var b = Stream.fromIterable([2, 4, 6]);

var merged = StreamGroup.merge([a, b]);
merged.listen(print); // Order depends on events
```

---

## 7. Pause, Resume, and Cancel

```dart
var subscription = numbers.listen(print);

// Pause
subscription.pause(Duration(seconds: 1));

// Resume
subscription.resume();

// Cancel
subscription.cancel();
```

---

## 8. Error Handling

```dart
numbers.listen(
  (data) => print(data),
  onError: (e) => print('Error: \$e'),
  onDone: () => print('Done'),
);
```

Wrap code in `try`/`catch` within `async` listeners or use `handleError`:

```dart
numbers.handleError((e) {
  print('Handled: \$e');
}).listen(print);
```

---

## 9. Practical Use Cases

- **HTTP responses** (`http.Response` as `Stream<List<int>>`)
- **File IO**: `File.openRead()` returns a `Stream<List<int>>`
- **WebSockets**: bidirectional `Stream` communication
- **Sensor/event data** in Flutter: accelerometer, location updates

```dart
import 'dart:io';

File('data.txt')
  .openRead()
  .transform(utf8.decoder)
  .transform(LineSplitter())
  .listen(print);
```

---

## 10. Best Practices

- Choose single-subscription or broadcast based on listener count.
- Close controllers to free resources.
- Handle errors and `onDone` callbacks.
- Use built-in transformers (`map`, `where`, `take`) before writing custom ones.
- Leverage packages like `rxdart` or `async` for advanced operations.

---

*End of Dart Streams*

