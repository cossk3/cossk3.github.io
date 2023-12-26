---
layout: post
title: "[Flutter] Flutter BLoC Pattern (1)"
categories: ["Language", "Flutter"]
tags: ["flutter", "flutter bloc pattern", "플러터"]
---

Provider 패턴보다 다소 복잡해 보이는 BLoC 패턴에 대해 알아보려고 한다.

---

# BLoC 패턴이란? 

`BLoC 패턴`을 사용하면 `Presentation`과 `Business Logic`을 쉽게 분리하여 코드를 빠르고, 테스트 하기에 용이하며, 재사용이 가능하게끔 한다.


`BLoC 패턴`은 다음과 같은 사항들과 그 이상을 충족하도록 설계되어있다고 한다.
+ 언제든지 Application의 상태를 알 수 있다.
+ 모든 경우를 쉽게 테스트하여 Application이 적절하게 응답하는지 확인할 수 있다.
+ 데이터 기반 결정을 내릴 수 있도록 Application의 모든 단일 사용자 상호 작용을 기록한다.
+ 최대한 효율적으로 작업하고 Application 내부와 다른 Application 전반에서 구성 요소를 재사용한다.
+ 많은 개발자들이 동일한 패턴과 규칙에 따라 단일 코드 기반 내에서 원할하게 작업 가능하다.
+ 빠르고 반응성이 뛰어난 Application 개발이 가능하다.


또, `BLoC 패턴`은 3가지 핵심 가치가 있다.
+ `Simple` : 이해하기 쉽고, 다양한 기술 수준을 가진 개발자들이 사용 가능하다.
+ `Powerful` : 더 작은 구성 요소를 구성하여 복잡한 Application 개발에 용이하다.
+ `Testable` : Application의 모든 측면을 쉽게 테스트 가능하다.

# BLoC 패턴을 사용하기 전에 알아야할 개념
## Streams

`Streams`는 비동기 데이터의 시퀀스이다. `BLoC 패턴`을 사용하기 위해서는 `Streams` 작동 방식에 대한 기본적인 이해가 중요하다고 한다.


#### 예시
```dart
Stream<int> countStream(int max) async* {
    for (int i = 0; i < max; i++) {
        yield i;
    }
}
```


위 코드처럼 함수에 `async*`를 사용하면 `yield` 키워드를 사용하여 `Stream` 데이터를 반환할 수 있다. `async*` 함수에서 `yield`를 사용할 때마다 `Stream`을 통해 해당 데이터를 푸시한다.



```dart
Future<int> sumStream(Stream<int> stream) async {
    int sum = 0;
    await for (int value in stream) {
        sum += value;
    }
    return sum;
}
```
또, 위 코드처럼 함수에 `async`를 사용하면 `await` 키워드를 사용하고 `Future` 정수를 반환할 수 있다. 위 코드에서는 `Stream`의 각 값을 기다리고 `Stream`에 있는 모든 정수의 합계를 반환하게 된다.

그래서 아래 코드와 같이 `Stream`을 사용할 수 있다.
```dart
void main() async {
    /// Initialize a stream of integers 0-9
    Stream<int> stream = countStream(10);
    /// Compute the sum of the stream of integers
    int sum = await sumStream(stream);
    /// Print the sum
    print(sum); // 45
}
```

## Cubit

<p>
<img src="https://github.com/cossk3/cossk3.github.io/assets/44231144/3d68b61f-6c28-4001-9bf7-d699bf36f217" width = "80%">
</p>

`Cubit`은 `BlocBase`를 확장하고 모든 유형의 상태 관리하도록 확장될 수 있는 클래스이다. `Cubit`은 상태 변경을 트리거하기 위해 호출할 수 있는 함수를 노출할 수 있다.

#### 예시 (1) : `Cubit` 생성
```dart
class CounterCubit extends Cubit<int> {
  CounterCubit() : super(0);
}
```
`Cubit`을 생성할 때 `Cubit`이 관리할 상태 유형을 정의해야 한다. 위 `CounterCubit`의 경우 상태는 `int`를 통해 표현될 수 있지만, 더 복잡한 경우 기본 유형 대신 `class`를 사용해야 할 수도 있다.

또, `Cubit`의 초기 상태를 지정해야 한다. 초기 상태 값으로 `super`를 호출하면 이를 수행 가능하다. 위 코드에서는 내부적으로 초기 상태를 0으로 설정하지만, 외부 값을 허용하여 `Cubit`이 더 유연해지도록 할 수도 있다. 바로 아래 코드처럼 말이다.

```dart
class CounterCubit extends Cubit<int> {
  CounterCubit(int initialState) : super(initialState);
}

final cubitA = CounterCubit(0); // state starts at 0
final cubitB = CounterCubit(10); // state starts at 10
```

#### 예시 (2) : 상태 변경
```dart
class CounterCubit extends Cubit<int> {
  CounterCubit() : super(0);

  void increment() => emit(state + 1);
}
```

`Cubit`은 `emit` 키워드를 통해 새로운 상태를 출력하는 기능이 있다. 위 코드에서 `CounterCubit`은 상태를 1씩 증가시키는 것을 `CounterCubit`에 알리기 위해 외부에서 호출할 수 있는 `increment`라는 공개 함수를 노출한다. `increment`가 호출되면 `state` getter를 통해 `Cubit`의 현재 상태에 액세스하고, 현재 상태에 1을 추가하여 새 상태를 `emit`할 수 있다.

#### 예시 (3) : `Cubit` 사용

```dart
void main() {
  final cubit = CounterCubit();
  print(cubit.state); // 0
  cubit.increment();
  print(cubit.state); // 1
  cubit.close();
}
```

위 코드에서는 먼저 `CounterCubit` 인스턴스를 생성한다. 그리고 초기 상태인 cubit의 현재 상태를 출력한다.

그 다음, `increment` 함수를 호출하여 상태 변경을 트리거하고 출력하면 cubit의 상태는 `0`에서 `1`로 변경된 것을 볼 수 있다. 마지막으로 `close` 함수를 호출하여 내부 상태 스트림을 닫는다.

```dart
Future<void> main() async {
  final cubit = CounterCubit();
  final subscription = cubit.stream.listen(print); // 1
  cubit.increment();
  await Future.delayed(Duration.zero);
  await subscription.cancel();
  await cubit.close();
}
```

`Cubit`은 실시간 상태 업데이트를 받을 수 있는 `Stream`을 노출한다. 위 코드에서 `CounterCubit`를 구독하고 각 상태 변경 시 print를 호출한다. 그 다음 새로운 상태를 생성하는 `increment` 함수를 호출한다. 더 이상 업데이트를 받고 싶지 않으면 `cancel`을 호출하고 `Cubit`을 닫는다.

#### 예시 (4) : `Cubit` 관찰

```dart
class CounterCubit extends Cubit<int> {
  CounterCubit() : super(0);

  void increment() => emit(state + 1);

  @override
  void onChange(Change<int> change) {
    super.onChange(change);
    print(change);
  }
}

void main() {
  CounterCubit()
    ..increment()
    ..close();
}
```

`Cubit`이 새로운 상태를 `emit`하면 `Change`가 발생한다. `onChange`를 overriding하여 특정 `Cubit`에 대한 모든 변경 사항을 관찰 가능하다.

위 코드는 아래와 같이 출력된다.

```
Change { currentState: 0, nextState: 1 }
```

##### BlocObserver

bloc 라이브러리 사용에 있어서 모든 `Changes`를 한 곳에서 액세스 할 수 있다. 모든 `Changes`에 응답하여 뭔가를 할 수 있기를 원한다면 단순하게 `BlocObserver`를 생성하면 된다.

```dart
class SimpleBlocObserver extends BlocObserver {
  @override
  void onChange(BlocBase bloc, Change change) {
    super.onChange(bloc, change);
    print('${bloc.runtimeType} $change');
  }
}

void main() {
  Bloc.observer = SimpleBlocObserver();
  CounterCubit()
    ..increment()
    ..close();  
}
```

`BlocObserver`를 확장하고 `onChange` 메서드를 overriding하면 된다. `BlocObserver`에서는 `Change` 자체 외에도 `Cubit` 인스턴스에 액세서가 가능하다. 위 코드는 아래와 같이 출력된다.

```
CounterCubit Change { currentState: 0, nextState: 1 }
Change { currentState: 0, nextState: 1 }
```

#### 예시 (4) : Error Handling

```dart
class CounterCubit extends Cubit<int> {
  CounterCubit() : super(0);

  void increment() {
    addError(Exception('increment error!'), StackTrace.current);
    emit(state + 1);
  }

  @override
  void onChange(Change<int> change) {
    super.onChange(change);
    print(change);
  }

  @override
  void onError(Object error, StackTrace stackTrace) {
    print('$error, $stackTrace');
    super.onError(error, stackTrace);
  }
}
```

모든 `Cubit`에는 에러가 발생했음을 나타내는 데에 사용 가능한 `addError` 메서드가 있다. 특정 `Cubit`에 대한 모든 에러를 처리하기 위해서 `Cubit` 내에서 `onError` 메서드를 overriding 가능하다.

```dart
class SimpleBlocObserver extends BlocObserver {
  @override
  void onChange(BlocBase bloc, Change change) {
    super.onChange(bloc, change);
    print('${bloc.runtimeType} $change');
  }

  @override
  void onError(BlocBase bloc, Object error, StackTrace stackTrace) {
    print('${bloc.runtimeType} $error $stackTrace');
    super.onError(bloc, error, stackTrace);
  }
}
```

위 코드처럼 `onError` 메서드는 보고된 모든 에러를 global하게 처리하기 위해 `BlocObserver`에서도 overriding 가능하다. `onChange`와 마찬가지로 내부 `onError` override 도 전역 `BlocObserver` override 전에 호출된다.

---
다음 2편에서는 BLoC 패턴을 어떻게 사용하는가를 다루려고 한다.