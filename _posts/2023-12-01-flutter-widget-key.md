---
layout: post
title: "[Flutter] Widget 생성자에서 볼 수 있는 Key는 무엇일까?"
categories: ["Language", "Flutter"]
tags: ["flutter", "flutter widget key", "플러터"]
---

기본적으로 Flutter에서의 위젯은 생성자에서 `Key` 매개변수를 받을 수 있는 것을 볼 수 있다. 하지만 사용하는 경우는 많이 없어 `Key`가 무엇인지, 어떻게 사용되는지는 알 수가  없었다. 그래서 도대체 이 `Key`는 무엇인지, 또 언제 어디에서 사용되는지를 알아보자.

---
  
# Key란 무엇인가
Flutter API 문서에서의 정의는 다음과 같다.
```
A Key is an identifier for Widgets, Elements and SemanticsNodes.

A new widget will only be used to update an existing element if its key is the same as the key of the current widget associated with the element.
```
  
즉, `Key`는 위젯 트리에서 위젯이 움직일 때마다 현 상태를 보존하는 역할을 한다. 그래서 사용자의 스크롤 위치나, 컬렉션이 수정된 상태를 보존할 때 많이 사용된다고 한다.

---

# Key는 언제 필요할까
위에서 말했듯이 `Key`는 어떤 상태를 유지하고 있는 같은 종류의 위젯 컬렉션을 더하고, 제거하고, 또는 새로 정렬해야 할 때 사용하는 것이 적합하다.
  
Flutter 사이트에서 제공하고 있는 예제를 토대로 Key를 사용해보자.
이 예제는 버튼을 눌렀을 때 서로 다른 두 개의 컬러 타일 자리를 바꾸는 앱이다.

## Key 예제
#### StatelessWidget
PositionTiles라는 상태 기반 위젯을 포함하고 있는데 두 개의 상태가 없는 타일을 생성한 후 일렬로 정렬시킨다. 그리고 FloatingActionButton을 누르면 tiles 리스트에 있는 두 개의 타일 포지션을 바꾼다.

```dart
import 'dart:math';
import 'package:flutter/material.dart';

void main() => runApp(const MaterialApp(
      home: PositionedTiles(),
      debugShowCheckedModeBanner: false,
    ));

class PositionedTiles extends StatefulWidget {
  const PositionedTiles({super.key});

  @override
  State<StatefulWidget> createState() => _PositionedTilesState();
}

class _PositionedTilesState extends State<PositionedTiles> {
  late List<Widget> tiles;

  @override
  void initState() {
    super.initState();

    tiles = [
      StatelessColorfulTile(),
      StatelessColorfulTile(),
    ];
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Row(
          children: tiles,
        ),
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.sentiment_very_dissatisfied),
        onPressed: swapTiles,
      ),
    );
  }

  swapTiles() {
    setState(() {
      tiles.insert(1, tiles.removeAt(0));
    });
  }
}

class StatelessColorfulTile extends StatelessWidget {
  final color = Color(Random().nextInt(0xFFFFFFFF));

  StatelessColorfulTile({super.key});

  @override
  Widget build(BuildContext context) {
    return Container(
      width: 100,
      height: 100,
      color: color,
    );
  }
}
```

위 예제를 실행시켜 보면 아래 이미지와 같이 두 개의 타일의 포지션이 잘 바뀌는 것을 볼 수 있다.

<p width="100%">
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/bf4979c5-a2d5-4ab4-aafa-cd0a80bd8e79" width="50%">
</p>
  
그렇다면 `StatelessWidget`을 `StatefulWidget`으로 변경하면 결과가 어떻게 달라지는지 확인해보자.
  
#### StatefulWidget
```dart
...
@override
void initState() {
  super.initState();

  tiles = [
    StatefulColorfulTile(),
    StatefulColorfulTile(),
  ];
}

...
class StatefulColorfulTile extends StatefulWidget {
  const StatefulColorfulTile({super.key});

  @override
  State<StatefulWidget> createState() => _StatefulColorfulTileState();
}

class _StatefulColorfulTileState extends State<StatefulColorfulTile> {
  final color = Color(Random().nextInt(0xFFFFFFFF));

  @override
  Widget build(BuildContext context) {
    return Container(
      width: 100,
      height: 100,
      color: color,
    );
  }
}
```

위 예제를 실행시키면 버튼을 아무리 눌러도 두 타일의 포지션은 바뀌지 않는다.
<p>
<img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/1e5ade34-b499-40de-827d-a580bb730df6" width="50%">
</p>
하지만 각각의 타일에 키를 더해주게 되면 두 타일의 포지션은 정상적으로 바뀌게 된다.

```dart
...
  @override
  void initState() {
    super.initState();

    tiles = [
      StatefulColorfulTile(key: UniqueKey(),),
      StatefulColorfulTile(key: UniqueKey(),),
    ];
  }
...
```
<p width="100%">
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/a4559c5c-95fd-4b1c-b2af-7f9a07e84dd5" width="50%">
</p>

이처럼 `StatelessWidget`인 경우에는 `Key`를 사용할 필요가 없지만 `StatefulWidget`인 경우에는 위 예제처럼 필요한 상황이 생긴다.

## Detail
Flutter는 내부적으로 모든 `Widget`마다 `Element`를 생성한다. `Element`에는 대응되는 `Widget`의 타입 정보와 자식 `Element`에 대한 참조를 가지고 있다. `Element`는 Flutter App의 골격으로 볼 수 있다.

#### StatelessWidget Tile
<p>
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/c3b698ab-a48d-449b-a84c-e2f7a2aec5eb" align="center" width="70%">
</p>

Widget Tree에서는 Row Widget부터 시작해 자식 Widget으로 이동하며 업데이트된 사항이 있는지 검사를 수행하게 된다. Element Tree에서는 대응되는 Widget의 Type과 Key의 정보가 이전 Widget의 정보와 일치한지 확인한다.
  
예제에서는 Key를 사용하지 않았으니 같은 Type인지 확인한 후 일치하면 Widget에 대한 참조를 업데이트하게 된다. 이처럼 StatefulWidget 안에서 화면 변경이 일어난 StatelessWidget 타일은 Key를 사용하지 않아도 화면 변경이 가능하기에 Key를 사용하지 않는다.

#### StatefulWidget Tile without Key
<p>
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/fcb6e725-1e81-4624-972d-7c42e3ba090b" align="center" width="70%">
</p>

이번엔 Key를 주지 않은 StatefulWidget의 경우다. StatelessWidget과 달리 색상 정보는 Widget Tree에 저장되는게 아니라 Element Tree에서 State Object에 저장된다.

Widget Tree가 변경되면 Element Tree는 마찬가지로 대응되는 Widget의 Type과 Key가 일치하는지 확인하게 된다. 따라서 Type이 일치하므로 Widget의 참조만 업데이트 하게된다. Flutter는 Widget을 화면에 출력하기 위해 Element Tree와 State Object의 정보를 사용한다. 그래서 결국 Widget의 순서가 변경되어도 출력되는 화면은 변경되지 않는다.

그럼 StatefulWidget 타일에 Key를 주게되면 어떻게 달라질까?

#### StatefulWidget Tile with Key
<p>
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/99381e5a-1a99-4456-8573-5914cb5a2211">
</p>
  
Widget Tree가 변경되면 Tile Element에서 Key가 대응되는 Widget의 Key와 다르다는 것을 확인하고 Flutter는 해당 Tile Element를 비활성화한 후 Element Tree에서 삭제한다.
  
<p>
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/0e4a94b7-9e6a-485c-ab0b-db1f46687a5f" align="center" width="70%">
</p>
  
그 다음 Flutter는 불일치가 확인된 Row Widget의 자식 Widget들을 탐색하면서 Key가 일치하는 Widget을 찾아 다시 1:1 매칭을 하게 되고, Widget Tree와의 Tree 구조를 맞추기 위해 Element Tree는 재구성 된다.

이렇게 Element Tree가 재구성되고 화면을 출력하면 State Object의 위치도 같이 변경됐기에 우리는 두 개의 타일의 포지션이 변경된 화면으로 볼 수 있다.

`Key`는 컬렉션에 있는 StatefulWidget의 순서나 숫자를 수정해야 할 때 유용하게 사용된다.

---

# Key는 어디에 넣어야하는가?
`Key`는 **<u>유지해야 하는 상태 정보가 있는 위젯 트리의 최상단 위젯</u>**에 사용되어야 한다.

## Key 예제
위에서 실행했던 예제에서 두 개의 StatefulWidget 타일들을 Padding Widget으로 감싸보자.

```dart
  @override
  void initState() {
    super.initState();

    tiles = [
      Padding(
        padding: const EdgeInsets.all(10.0),
        child: StatefulColorfulTile(key: UniqueKey(),),
      ),
      Padding(
        padding: const EdgeInsets.all(10.0),
        child: StatefulColorfulTile(key: UniqueKey(),),
      ),
    ];
  }
```
<p>
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/363e5b8d-8a33-43d0-9ed0-796b18083a00" align="center" width="50%">
</p>
  
버튼을 누를때마다 두 타일의 포지션이 변경되지 않고 완전히 무작위하게 색상이 변경된다.

왜 그럴까?

## Detail
<p>
  <img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/253c6046-e00a-4f26-8b67-306445ff15fc">
</p>
  
위 이미지는 Padding Widget이 추가된 WidgetTree와 Element Tree의 관계도이다.

두 타일의 위치를 바꾸게 되면 Flutter의 `Element Widget 매칭 알고리즘`이 Tree 레벨 단위로 진행된다. Flutter는 Padding Widget에서 모든 것이 일치하는 것을 확인하고 아래 레벨로 내려가게 되고, Tile Element의 Key가 대응되는 Widget의 Key와 다른 것을 확인한다.

Flutter는 해당 Tile Element를 비활성화한 후 Element Tree에서 삭제하게 된다. 예제에서 사용한 Key는 `LocalKey` 유형인데, 이것은 Flutter가 전체 트리 중 특정 레벨 안에서 매칭되는 Key만 확인한다는 의미이다.

Flutter는 해당 레벨에서 일치하는 Key를 가진 Tile Element를 찾을 수 없으니 새로운 Element를 생성한 후 새로운 State Object를 초기화하게 된다. 그래서 버튼을 누를 때마다 무작위로 타일 색상이 변경되는 것이다.
  

이를 해결하려면 아래와 같이 Paddding Widget에 Key를 넣어주면 정상적으로 두 타일의 색상이 변경되는 것을 확인할 수 있다.
```dart
  @override
  void initState() {
    super.initState();

    tiles = [
      Padding(
        key: UniqueKey(),
        padding: const EdgeInsets.all(10.0),
        child: const StatefulColorfulTile(),
      ),
      Padding(
        key: UniqueKey(),
        padding: const EdgeInsets.all(10.0),
        child: const StatefulColorfulTile(),
      ),
    ];
  }
```

---

# 결론
- Widget Tree 내에서 상태를 유지하고 싶은 곳에 `Key`를 사용하자.
- 리스트와 같이 동일 타입 위젯 컬렉션을 수정하려 할 때가 가장 일반적이다.
- 유지하고자 하는 Widget Tree 최상단에 `Key`를 넣자.