### 問題 1

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> delayedTask() {
  return Future.delayed(Duration(seconds: 1), () => '1秒後の処理');
}

void main() {
  print('プログラム開始');
  
  delayedTask()
    .then((value) {
      print(value);
    });
  
  print('プログラム終了');
}
```

<details>
<summary>答え</summary>

```
プログラム開始
プログラム終了
1秒後の処理
```

</details>

### 問題 2

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> delayedTask() async {
  await Future.delayed(Duration(seconds: 1));
  return '1秒後の処理';
}

void main() async {
  print('プログラム開始');
  
  String result = await delayedTask();
  print(result);
  
  print('プログラム終了');
}
```

<details>
<summary>答え</summary>

```
プログラム開始
1秒後の処理
プログラム終了
```

</details>

### 問題 3

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> taskA() async {
  await Future.delayed(Duration(seconds: 2));
  return 'タスクA完了';
}

Future<String> taskB() async {
  await Future.delayed(Duration(seconds: 1));
  return 'タスクB完了';
}

void main() async {
  print('開始');
  
  String resultA = await taskA();
  print(resultA);
  
  String resultB = await taskB();
  print(resultB);
  
  print('終了');
}
```

<details>
<summary>答え</summary>

```
開始
タスクA完了
タスクB完了
終了
```

</details>

### 問題 4

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> taskA() async {
  await Future.delayed(Duration(seconds: 2));
  return 'タスクA完了';
}

Future<String> taskB() async {
  await Future.delayed(Duration(seconds: 1));
  return 'タスクB完了';
}

void main() async {
  print('開始');
  
  Future<String> futureA = taskA();
  Future<String> futureB = taskB();
  
  String resultA = await futureA;
  String resultB = await futureB;
  
  print(resultA);
  print(resultB);
  print('終了');
}
```

<details>
<summary>答え</summary>

```
開始
タスクA完了
タスクB完了
終了
```

</details>

### 問題 5

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> taskA() async {
  print('タスクA開始');
  await Future.delayed(Duration(seconds: 2));
  print('タスクA終了');
  return 'A';
}

Future<String> taskB() async {
  print('タスクB開始');
  await Future.delayed(Duration(seconds: 1));
  print('タスクB終了');
  return 'B';
}

void main() async {
  print('開始');
  
  List<String> results = await Future.wait([taskA(), taskB()]);
  
  print('結果: ${results.join(', ')}');
  print('終了');
}
```

<details>
<summary>答え</summary>

```
開始
タスクA開始
タスクB開始
タスクB終了
タスクA終了
結果: A, B
終了
```

</details>

### 問題 6

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> riskyTask() async {
  await Future.delayed(Duration(seconds: 1));
  throw Exception('エラーが発生しました');
}

void main() async {
  print('開始');
  
  try {
    String result = await riskyTask();
    print(result);
  } catch (e) {
    print('エラーをキャッチ: ${e.toString()}');
  }
  
  print('終了');
}
```

<details>
<summary>答え</summary>

```
開始
エラーをキャッチ: Exception: エラーが発生しました
終了
```

</details>

### 問題 7

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> innerTask() async {
  print('内部タスク開始');
  await Future.delayed(Duration(seconds: 1));
  print('内部タスク終了');
  return '内部結果';
}

Future<String> outerTask() async {
  print('外部タスク開始');
  String inner = await innerTask();
  print('内部タスクから受け取った: $inner');
  await Future.delayed(Duration(seconds: 1));
  print('外部タスク終了');
  return '外部結果';
}

void main() async {
  print('プログラム開始');
  String result = await outerTask();
  print('最終結果: $result');
  print('プログラム終了');
}
```

<details>
<summary>答え</summary>

```
プログラム開始
外部タスク開始
内部タスク開始
内部タスク終了
内部タスクから受け取った: 内部結果
外部タスク終了
最終結果: 外部結果
プログラム終了
```

</details>

### 問題 8

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> quickTask() async {
  print('クイックタスク実行');
  return 'クイック完了';
}

Future<String> slowTask() async {
  print('スロータスク開始');
  await Future.delayed(Duration(seconds: 2));
  print('スロータスク終了');
  return 'スロー完了';
}

void main() async {
  print('開始');
  
  Future<String> slowFuture = slowTask();
  
  print('同期処理1');
  
  String quickResult = await quickTask();
  print('クイック結果: $quickResult');
  
  print('同期処理2');
  
  String slowResult = await slowFuture;
  print('スロー結果: $slowResult');
  
  print('終了');
}
```

<details>
<summary>答え</summary>

```
開始
スロータスク開始
同期処理1
クイックタスク実行
クイック結果: クイック完了
同期処理2
スロータスク終了
スロー結果: スロー完了
終了
```

</details>

### 問題 9

**問題:** 以下の仕様を満たす非同期挨拶関数を実装してください。

**仕様:**
1. `greetAsync(String name)` 関数を実装する
2. 関数は1秒待機してから挨拶メッセージを返す
3. 戻り値は `'こんにちは、$name さん！'` という文字列
4. `main` 関数で `greetAsync` を呼び出し、結果を表示する

<details>
<summary>答え（実装例）</summary>

```dart
Future<String> greetAsync(String name) async {
  await Future.delayed(Duration(seconds: 1));
  return 'こんにちは、$name さん！';
}

void main() async {
  String greeting = await greetAsync('太郎');
  print(greeting);
}
```

</details>

### 問題 10

**問題:** 以下の仕様を満たす関数を実装してください。

**仕様:**
1. `divideAsync(int a, int b)` 関数を実装する
2. bが0の場合は `Exception('0で割ることはできません')` を投げる
3. 正常な場合は1秒待機してから `a / b` の結果を返す
4. `main` 関数で `divideAsync(10, 0)` を呼び出し、try-catchでエラーをキャッチして適切なメッセージを表示する

<details>
<summary>答え（実装例）</summary>

```dart
Future<double> divideAsync(int a, int b) async {
  if (b == 0) {
    throw Exception('0で割ることはできません');
  }
  await Future.delayed(Duration(seconds: 1));
  return a / b;
}

void main() async {
  try {
    double result = await divideAsync(10, 0);
    print('結果: $result');
  } catch (e) {
    print('エラーが発生しました: $e');
  }
}
```

</details>

