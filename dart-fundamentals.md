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

## #2.0 Basic Data Types

```dart
String name = "henry";
bool alive = true;
int age = 12;
double mouney = 69.99;
num x = 12;
x = 1.1; // double extends num
```

## #2.1 Lists

```dart
var numbers1 = [1, 2, 3, 4];
List<int> numbers2 = [1, 2, 3, 4];
numbers2.add(5);
```

- collection if

```dart
var giveMeThree = true;
var numbers = [
  1,
  2,
  if (giveMeThree) 3,
];
```

## #2.2 String Interpolation

```dart
var name = "henry";
var age = 10;
var greeting = 'Hello I\'m $name and ${age + 2} years old';
```

## #2.3 Collection For

```dart
var oldFriends = ['nico', 'lynn'];
var newFriends = [
  'lewis',
  'ralph',
  'darren',
  for (var friend in oldFriends) "💖 $friend",
];
```

## #2.4 Maps

- Object = any

```dart
Map<String, Object> player = {
    'name': 'henry',
    'xp': 19.99,
    'superpower': false,
};
```

```dart
Map<List<int>, bool> player = {
    [1, 2]: true,
    [2]: false,
};
```

```dart
List<Map<String, Object>> players = [
  {'name': 'henry', 'xp': 199.99},
  {'name': 'hoon', 'xp': 199.99},
]
```

## #2.5 Sets

```dart
Set<int> numbers = {1, 2, 3, 4};
```

## #3.0 Defining a Function

```dart
String sayHello(String name) {
    return "Hello $name nice to meett you!"
}

String sayHello(String name) => "Hello $name nice to meett you!"

void main {
  sayHello("henry")
}
```

## #3.1 Named Parameters

- positional parameter

```dart
String sayHello(String name, int age, String country) {
    return "Hello $name, you are $age, and you come from $country";
  }

sayHello('nico', 19, 'cuba');
```

- named parameter

1. give default value to parameter

```dart
String sayHello(
      {String name = 'henry', int age = 19, String country = 'wakanda'}) {
    return "Hello $name, you are $age, and you come from $country";
  }

  print(sayHello());
```

2. `required` keyword (not-null)

```dart
String sayHello(
      {String name = 'henry', required int age, String country = 'wakanda'}) {
    return "Hello $name, you are $age, and you come from $country";
  }

  print(sayHello(age: 19));
```

## #3.3 Optional Positional Parameters

```dart
String sayHello(
    String name, int age, [String? country = 'wakanda']) { // [] + default value makes optional positional parameters but is `?` mandatory?
  return "Hello $name, you are $age, and you come from $country";
}

void main() {
  print(sayHello('henry', 1));
}
```

## #3.4 QQ Operator

- if left is null, use right

```dart
left ?? right
```

```dart
String capitalizeName(String? name) => name?.toUpperCase() ?? 'a';

print(capitalizeName('henry'));
print(capitalizeName(null));

String? name;
name ??= 'one';
name ??= 'another';
print(name);
```

## #3.5 Typedef

```dart
typedef ListOfInts = List<int>;

void main() {
  ListOfInts reverseListOfNumbers(ListOfInts list) {
    var reversed = list.reversed;
    return reversed.toList();
  }

  print(reverseListOfNumbers([1, 2, 3]));
}
```

- we can do `typedef` with map as well but, better to use Class!

```dart
typedef UserInfo = Map<String, String>; // => better to convert to Class
```

## #4.0 Your First Dart Class

```dart
class Player {
  late final String name;
  late int xp;

  Player(String name, int xp) {
    this.name = name;
    this.xp = xp;
  }

  void sayHello() {
    print("Hi my name is $name");
  }
}

void main() {
  var player1 = Player("henry", 1500);
  print(player1.name);
//   player.name = 'bla'; // error from final

  var player2 = Player("nico", 2000);
  player2.sayHello();
}
```

## #4.1 Constructors

- syntax sugar of constructor

```dart
class Player {
  final String name;
  int xp;

  Player(this.name, this.xp);
}
```

## #4.2 Named Constructor Parameters

```dart
class Player {
  final String name;
  int xp;
  String team;
  int age;

  Player({
    required this.name,
    required this.xp,
    required this.team,
    required this.age,
  });

  void sayHello() {
    print("Hi my name is $name");
  }
}

void main() {
  var player1 = Player(
    name: "henry",
    xp: 1,
    team: "red",
    age: 30
  );
  var player2 = Player(
    name: "nico",
    xp: 2,
    team: "orange",
    age: 31
  );
}
```

## #4.3 Named Constructors

```dart
Player.createBluePlayer({required String name, required int age})
      : this.age = age,
        this.name = name,
        this.team = 'blue',
        this.xp = 0;

...
var player3 = Player.createBluePlayer(name: "test", age: 1);
```

## #4.4 Recap + fromJson

```dart
Player.fromJson(Map<String, dynamic> playerJson)
      : name = playerJson['name'],
        xp = playerJson['xp'],
        team = playerJson['team'];
...
var apiData = [
  {"name": "henry", "team": "red", "xp": 0},
  {"name": "nico", "team": "blue", "xp": 0},
];

apiData.forEach((playerJson) {
  var player = Player.fromJson(playerJson);
  player.sayHello();
});
```

## #4.5 Cascade Notation (03:13)

- `..` means the `self` able to use cascade even including method

```dart
class Player {
  String name;
  int xp;
  String team;

  Player({required this.name, required this.xp, required this.team});

  void sayHello() {
    print("hello I'm ${this.name}");
  }
}

void main() {
  var henry = Player(name: 'henry', xp: 1200, team: 'red');
  var updated = henry
    ..name = 'new_henry'
    ..xp = 9999
    ..team = 'blue'
    ..sayHello();
}
```

## #4.6 Enums

```dart
enum Team { red, blue }
...
var henry = Player(name: 'henry', xp: 1200, team: Team.red);
```

## #4.7 Abstract Classes

```dart
abstract class Human {
  void walk();
}

class Player extends Human {
  String name;
  int xp;
  Team team;

  Player({required this.name, required this.xp, required this.team});

  void walk() {
    print("walk like player");
  }
...
}
```

## #4.8 Inheritance

```dart
enum Team { red, blue }

class Human {
  final String name;
  Human(this.name);
  void sayHello() {
    print("Hi my name is $name");
  }
}

class Player extends Human {
  final Team team;
  Player({
    required this.team,
    required String name,
  }) : super(name);

  void sayHello() {
    super.sayHello();
    print("and i play for $team");
  }
}

void main() {
  var henry = Player(name: 'henry', team: Team.red);
  henry.sayHello();
}
```

1. `constructor(): super(args)` syntax forward argument to super class

   - can use named parameter in super's constructor as well but it can be verbose

2. `super.{method}()` can call super's method

## #4.9 Mixins

- classes without any constructor
- to reuse static values?

```dart
class Strong {
  final double strengthLevel = 1500.99;
}

class QuickRunner {
  void runQuick() {
    print("r----u----n");
  }
}

class Player with Strong, QuickRunner {}
class Horse with Strong, QuickRunner {}
```
