# Flutter Fundamentals

# 1. Introduction

- flutter is like video game engine
- flutter does not use native renders
- it just mocks natives to render by itselfs

# 2. Hello Flutter

## 2.0. Installation

- local install

```sh
brew install --cask flutter
flutter doctor
```

- web

  https://dartpad.dev

## 2.2. Running Flutter

```sh
flutter create toonflix
```

## 2.3. Hello World

### Design Systems

- Material widget (Google, recommanded at first) VS Cupertino widget (IOS)
- Scaffold: wire frame like CSS grid

# 3. UI Challenge

## 3.0. Header

- padding

```dart
Padding(
          padding: EdgeInsets.symmetric(
            horizontal: 40, // == px-40
          ),
```

- align

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.end, // main of Row: horizontal
  ..
)
Column(
  crossAxisAlignment: CrossAxisAlignment.end, // cross of Column: horizontal
  ..
)
```

# 3.3. VSCode Settings

- codeActionsOnSave

```dart
"editor.codeActionsOnSave": {
    "source.fixAll": true
  },
```

- previewFlutterUiGuides

```dart
  "dart.previewFlutterUiGuides": true
```

# 3.4. Code Actions

- cmd + `.` -> `Wrap with widget`

# 3.5. Resuable Widgets

- extract Widget by code action

# 3.7. Icons and Transforms

- clip overflowed edge

```dart
clipBehavior: Clip.hardEdge,
```

- Transform without affecting to other widgets around

```dart
Transform.scale(
  scale: 2.2,
  child: Transform.translate(
    offset: const Offset(-5, 12),
    child: const Icon(
      Icons.euro_rounded,
      color: Colors.white,
      size: 88,
    ),
  ),
)
```

## 3.8. Reusable Cards

- SingleChildScrollView to prevent horizontal overflow

```dart
body: SingleChildScrollView(
```

- define static memeber var staring with `_`

```dart
class CurrencyCard extends StatelessWidget {
  final String name, code, amount;
  final IconData icon;
  final bool isInverted;

  final _blackColor = const Color(0xFF1F2123);
  ..
```

# 4. Statefull Widgets

## 4.1. setState

- Stateful Wedget rerender all(?) whenever setState is called
- state is `rarely` used in flutter

```dart
// lib/main.dart
import 'package:flutter/material.dart';

void main() {
  runApp(const App());
}

class App extends StatefulWidget {
  const App({super.key});

  @override
  State<App> createState() => _AppState();
}

class _AppState extends State<App> {
  int counter = 0;

  void onClicked() {
    setState(() {
      counter += 1;
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        backgroundColor: const Color(0xFFF4EDDB),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const Text(
                'Click Count',
                style: TextStyle(fontSize: 30),
              ),
              Text(
                '$counter',
                style: const TextStyle(fontSize: 30),
              ),
              IconButton(
                iconSize: 40,
                onPressed: onClicked,
                icon: const Icon(
                  Icons.add_box_rounded,
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

## 4.3. BuildContext

- A handle to the location of a widget in the widget tree.
- Can access to higher widget tree like Theme

```dart
MaterialApp(
      theme: ThemeData(
        textTheme: const TextTheme(
          titleLarge: TextStyle(
            color: Colors.red,
          ),
        ),
..
color: Theme.of(context).textTheme.titleLarge!.color,
```

- exclamation mark means it is not null for sure
