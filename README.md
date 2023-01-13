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
- Scaffold: wire frame like CSS grid (basic layout)

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

## 4.4. Widget lifecycle

- initState
  - is called once before build
  - use to subscribe API
- dispose: is called when any(?) widget is died

```dart
..
class _MyLargeTitleState extends State<MyLargeTitle> {
  @override
  void initState() {
    super.initState();
    print('initState!');
  }

  @override
  void dispose() {
    super.dispose();
    print('dispose!');
  }

  @override
  Widget build(BuildContext context) {
    print('build!');
    return Text(
      'My Large Title',
      style: TextStyle(
        fontSize: 30,
        color: Theme.of(context).textTheme.titleLarge!.color,
      ),
    );
  }
}
```

# 5. Pomodoro APP

## 5.1. User Interface

- expanded widget: expanded horizontally like `w-full`

```dart
Expanded(
  child: Container(
..
```

- flexible widget: split with flex

```dart
Flexible(
  flex: 1,
  child: Container(
..
```

- align bottomCenter: stick to bottom of flex

```dart
alignment: Alignment.bottomCenter,
```

## 5.1. Timer

- timer class

```dart
int totalSeconds = 1500;
late Timer timer;

void onTick(Timer timer) {
  setState(() {
    totalSeconds = totalSeconds - 1;
  });
}

void onStartPressed() {
  timer = Timer.periodic(
    const Duration(seconds: 1),
    onTick,
  );
}
```

## 5.2. Pause Play

- cancel timer

```dart
void onPausePressed() {
    timer.cancel();
    setState(() {
      isRunning = false;
    });
  }
```

## 5.3. Date Format

```dart
String format(int seconds) {
    var duration = Duration(seconds: seconds);
    return duration.toString().split(".").first.substring(2, 7);
  }
```

# 6. Webtoon APP

## 6.1. AppBar

- Key warning: Widget has key to identify

```dart
class App extends StatelessWidget {
  const App({super.key})
..
```

## 6.2. Data Fetching

- https://pub.dev/ : like npm
- pubspec.yaml
  - like package.json
  - able to add package by just editing this file
- by cmd

```
flutter pub add http
```

- http method

```dart
void getTodaysToons() async {
    final url = Uri.parse('$baseUrl/$today');
    final response = await http.get(url);
    if (response.statusCode == 200) {
      print(response.body);
      return;
    }
    throw Error();
  }
```

## 6.3. fromJson

- jsonDecode

```dart
final List<dynamic> webtoons = jsonDecode(response.body);
```

- named constructor with fromJson

```dart
WebtoonModel.fromJson(Map<String, dynamic> json)
    : title = json['title'],
      thumb = json['thumb'],
      id = json['id'];
```

## 6.5. waitForWebToons

- state + manual fetch

```dart
class _HomeScreenState extends State<HomeScreen> {
  List<WebtoonModel> webtoons = [];
  bool isLoading = true;

  void waitForWebToons() async {
    webtoons = await ApiService.getTodaysToons();
    isLoading = false;
    setState(() {});
  }
..
```

## 6.6. FutureBuilder

- FutureBuilder gives context(BuildContext) and snapshot(Future)
- snapshot.hasData can split loading states

```dart
body: FutureBuilder(
  future: webtoons,
  builder: (context, snapshot) {
    if (snapshot.hasData) {
      return const Text("There is data!");
    }
    return const Text('Loading....');
  },
),
```

## 6.7. ListView

- ListView gives pagenated infinite scroll
- itemBuilder take index and load dynamically
- separatorBuilder sets the gap between items
- CircularProgressIndicator gives loading bar

```dart
builder: (context, snapshot) {
  if (snapshot.hasData) {
    return ListView.separated(
      scrollDirection: Axis.horizontal,
      itemCount: snapshot.data!.length,
      itemBuilder: (context, index) {
        var webtoon = snapshot.data![index];
        return Text(webtoon.title);
      },
      separatorBuilder: (context, index) => const SizedBox(width: 20),
    );
  }
  return const Center(
    child: CircularProgressIndicator(),
  );
},
```

## 6.8. Webtoon Card

- Expanded: Columns need to know fixed size => use Expanded to fill to end of screen

```dart
return Column(
  children: [
    const SizedBox(
      height: 50,
    ),
    Expanded(child: makeList(snapshot))
  ],
);
```

- image load + border radius

```dart
 Container(
  width: 250,
  clipBehavior: Clip.hardEdge,
  decoration: BoxDecoration(
    borderRadius: BorderRadius.circular(15),
    boxShadow: [
      BoxShadow(
        blurRadius: 15,
        offset: const Offset(10, 10),
        color: Colors.black.withOpacity(0.3),
      )
    ],
  ),
  child: Image.network(webtoon.thumb),
),
```

- web image setting
  - set to `html`
  - default is `canvaskit` which is blocked by some api servers
  - https://docs.flutter.dev/development/platform-integration/web/renderers

```
"dart.flutterRunAdditionalArgs": [
    "--web-renderer",
    "html"
]
```
