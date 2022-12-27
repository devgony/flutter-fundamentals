# Dart Fundamentals

#0 INTRODUCTION

## #0.1 Why Dart

### JIT + AOT

- Just In Time compile
  For development mode, it compiles just in time on Dart VM regardless of platform
- Ahead Of Time compile
  For Production mode, it compiles to specific platform to optimize performance

## #0.2 How To Learn

- <https://dartpad.dev/?>
- install => VSC plugin

# 1 VARIABLES

## #1.0 Hello World

- void main() is mandatory
- semicolon `;` is mandatory

## #1.1 The Var Keyword

- 1. var {varName}

```dart
var name = 'henry'; // inferring type
name = 'ferris'; // can only update with same type
```

- 2. type {varName}

```dart
String name = 'henry';
```

## #1.2 Dynamic Type

- `dynamic` keyword like `any`

```dart
dynamic name;
if (name is String) {
    name.{anyStringMethod}
  }

if (name is int) {
    name.{anyIntMethod}
  }
```

## #1.3 Nullable Variables

```dart
bool isEmpty(String string) => string.length == 0;

main() {
    isEmpty(null); // crash
  }
```

- with `?`, variable can be null
- without `?`, default is `not null`

```dart
String? name = 'henry';
name = null;
name?.isNotEmpty;
```

## #1.4 Final Variables

```dart
final name = 'nico'; // immutable
name = 'nico';
```

## #1.5 Late Variables

```dart
late final String name;
// do something, go to api
print(name) // error => null safe! should assign first
name = 'nico'; // assign late
```

## #1.6 Constant Variables

- compile time constant

```dart
const API = '121212';
const API = fetchApi(); // error => cannot know at compile time
```
