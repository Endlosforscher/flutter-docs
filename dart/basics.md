# Dart Basics

This document introduces the fundamental concepts of the Dart programming language. It covers syntax, data types, control flow, functions, classes, null safety, asynchronous programming, and essential tools.

---

## 1. Introduction to Dart

- Dart is a client-optimized, object-oriented language for building fast apps on multiple platforms (mobile, web, server, desktop).
- Developed by Google and used as the language for Flutter.
- Features:
  - Strong static typing with type inference
  - Null safety
  - Concurrency with isolates and asynchronous features
  - Rich standard libraries

---

## 2. Variables and Data Types

Dart supports the following built-in types:

| Type       | Description                          |
|------------|--------------------------------------|
| `int`      | Integer numbers                      |
| `double`   | Floating-point numbers               |
| `String`   | Sequence of characters               |
| `bool`     | Boolean values (`true`/`false`)      |
| `List<T>`  | Ordered collection of items of type `T` |
| `Set<T>`   | Unordered collection of unique items |
| `Map<K, V>`| Key–value pairs                      |
| `dynamic`  | Opt out of static checking (avoid when possible) |

**Declaring variables:**

```dart
int count = 10;
double price = 19.99;
String name = 'Dart';
bool isValid = true;
var message = 'Hello, Dart!';  // inferred as String
```

---

## 3. Control Flow

### 3.1 Conditional Statements

```dart
if (count > 0) {
  print('Count is positive');
} else if (count == 0) {
  print('Count is zero');
} else {
  print('Count is negative');
}
```

### 3.2 Switch Statement

```dart
switch (command) {
  case 'open':
    executeOpen();
    break;
  case 'close':
    executeClose();
    break;
  default:
    print('Unknown command');
}
```

### 3.3 Loops

```dart
// For loop\ for (var i = 0; i < 5; i++) {
  print(i);
}

// For-in loop
for (var item in items) {
  print(item);
}

// While loop
while (count > 0) {
  count--;
}

// Do-while loop
do {
  performTask();
} while (condition);
```

---

## 4. Functions

```dart
// Named function
double add(double a, double b) {
  return a + b;
}

// Arrow syntax
double multiply(double a, double b) => a * b;

// Optional parameters
double power(double base, [double exponent = 2]) {
  return base == 0 ? 0 : base;
}

// Named parameters with null safety
void greet({required String name, String? message}) {
  print('Hello, \$name! \${message ?? ''}');
}
```

---

## 5. Collections

```dart
// List
List<String> fruits = ['apple', 'banana', 'orange'];

// Set
Set<int> numbers = {1, 2, 3};

// Map
Map<String, int> scores = {'Alice': 90, 'Bob': 85};

// Iterating collections
for (var fruit in fruits) {
  print(fruit);
}
```

---

## 6. Null Safety

- Dart enforces non-nullable types by default.
- Append `?` to allow null values.
- Use `late` for lazy initialization of non-nullable fields.

```dart
String? nullableName;
late String nonNullable;

void setup() {
  nonNullable = 'Initialized';
}
```

---

## 7. Classes and Objects

```dart
class Person {
  String name;
  int age;

  // Constructor
  Person(this.name, this.age);

  // Named constructor
  Person.guest() : name = 'Guest', age = 0;

  // Method
  void introduce() {
    print('Hi, I\'m \$name and I\'m \$age years old.');
  }
}

// Using the class
var alice = Person('Alice', 30);
alice.introduce();
```

---

## 8. Exception Handling

```dart
try {
  var result = riskyOperation();
} on FormatException catch (e) {
  print('Format error: \$e');
} catch (e) {
  print('Unknown error: \$e');
} finally {
  cleanup();
}
```

---

## 9. Asynchronous Programming

### 9.1 Futures

```dart
Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 2));
  return 'Data loaded';
}

void main() async {
  print('Start');
  var data = await fetchData();
  print(data);
}
```

### 9.2 Streams

```dart
Stream<int> counter(int to) async* {
  for (int i = 1; i <= to; i++) {
    yield i;
    await Future.delayed(Duration(milliseconds: 500));
  }
}

void listenCounter() {
  counter(3).listen((value) => print(value));
}
```

---

## 10. Dart Tools

- **Analyze**: `dart analyze`
- **Format**: `dart format .`
- **Test**: `dart test`
- **Package manager**: `dart pub get`, `dart pub upgrade`
- **Compile to native** (experimental): `dart compile exe bin/main.dart -o main`

---

*End of Dart Basics*

