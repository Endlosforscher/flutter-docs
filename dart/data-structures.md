# Dart Data Structures

This document covers core and advanced data structures available in Dart, both in the core language and through the standard libraries. It includes examples to help you choose and use the right structure for your needs.

---

## 1. Overview

- Dart provides built-in collection types (`List`, `Set`, `Map`) that are generic and type-safe.
- Additional structures (`Queue`, `LinkedList`, `SplayTreeMap`, etc.) are available in `dart:collection`.
- All structures support null safety and generics.

---

## 2. Lists

An ordered, indexable collection.

```dart
// Fixed-length list
var fixed = List<int>.filled(3, 0);
// Growable list
var fruits = <String>['apple', 'banana'];
fruits.add('orange');

// Access
print(fruits[0]); // apple

// Iterate
for (var f in fruits) print(f);

// Common methods
fruits.insert(1, 'mango');
fruits.remove('banana');
fruits.sort();
```

---

## 3. Sets

An unordered collection of unique items.

```dart
var animals = <String>{'cat', 'dog'};
animals.add('bird');
animals.add('dog'); // ignored

print(animals.contains('cat')); // true

// Set operations
var a = {1, 2, 3};
var b = {2, 3, 4};
print(a.union(b));      // {1,2,3,4}
print(a.intersection(b)); // {2,3}
print(a.difference(b));   // {1}
```

---

## 4. Maps

Key–value pairs, with unique keys.

```dart
var scores = <String, int>{
  'Alice': 90,
  'Bob': 85,
};

scores['Charlie'] = 92;

print(scores['Alice']);

// Iterate entries
scores.forEach((name, score) {
  print('$name: $score');
});

// Methods
scores.remove('Bob');
var keys = scores.keys;
var values = scores.values;
```

---

## 5. Queues

FIFO collections, available in `dart:collection`.

```dart
import 'dart:collection';

var queue = Queue<int>();
queue.addAll([1, 2, 3]);

print(queue.removeFirst()); // 1
print(queue.removeLast());  // 3
```

---

## 6. LinkedList

Doubly-linked list of elements that implement `LinkedListEntry`.

```dart
import 'dart:collection';

class Node extends LinkedListEntry<Node> {
  int value;
  Node(this.value);
}

var list = LinkedList<Node>();
list.add(Node(10));
list.add(Node(20));

for (var node in list) {
  print(node.value);
}
```

---

## 7. SplayTree Structures

Self-balancing binary search trees (`SplayTreeSet` and `SplayTreeMap`).

```dart
import 'dart:collection';

var treeSet = SplayTreeSet<int>();
treeSet.addAll([3, 1, 2]);
print(treeSet.toList()); // [1,2,3]

var treeMap = SplayTreeMap<String, int>();
treeMap['b'] = 2;
treeMap['a'] = 1;
print(treeMap.keys); // (a, b)
```

---

## 8. Typed Data (Uint8List, etc.)

Efficient fixed-size lists for binary data, in `dart:typed_data`.

```dart
import 'dart:typed_data';

var bytes = Uint8List(4);
bytes[0] = 0xFF;
print(bytes); // [255,0,0,0]
```

---

## 9. Unmodifiable Views

Create read-only views on collections.

```dart
import 'dart:collection';

var original = [1, 2, 3];
var view = UnmodifiableListView(original);
// view.add(4); // throws

original.add(4);
print(view); // [1,2,3,4]
```

---

## 10. Choosing the Right Structure

| Use Case               | Structure             |
|------------------------|-----------------------|
| Ordered sequence       | `List<T>`             |
| Unique unordered items | `Set<T>`              |
| Key–value mapping      | `Map<K, V>`           |
| FIFO processing        | `Queue<T>`            |
| Frequent insert/delete | `LinkedList<T>`       |
| Sorted data            | `SplayTreeSet`/`Map`  |
| Binary data            | `Uint8List`, `ByteData` |

---

*End of Dart Data Structures*

