# Dart 非同期処理問題集

以下のブラウザ上でdartを実行できるサイトを使用しても大丈夫です

[https://dartpad.dev](https://dartpad.dev)

---

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

---

### 問題 2

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> taskB() {
  return Future.delayed(Duration(seconds: 2), () => 'B');
}

Future<String> taskC() {
  return Future.delayed(Duration(seconds: 1), () => 'C');
}

void main() {
  print('A');

  taskB().then((value) => print(value));
  taskC().then((value) => print(value));

  print('D');
}
```

<details>
<summary>答え</summary>

```
A
D
C
B
```

</details>

---

### 問題 3

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<void> taskA() {
  return Future.delayed(Duration(seconds: 1), () => print('タスクA'));
}

void main() {
  print('スタート');

  taskA()
      .then((_) => print('タスクA完了'));

  print('中間ポイント');
}
```

<details>
<summary>答え</summary>

```
スタート
中間ポイント
タスクA
タスクA完了
```

</details>

---

### 問題 4

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> getNestedData() {
  return Future.delayed(Duration(seconds: 1), () {
    print('Future処理');
    return 'ネストしたFuture';
  });
}

void main() {
  print('処理開始');

  getNestedData()
      .then((value) {
        print('.then処理: $value');
      });

  print('処理終了');
}
```

<details>
<summary>答え</summary>

```
処理開始
処理終了
Future処理
.then処理: ネストしたFuture
```

</details>

---

### 問題 5

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> fetchDataWithError() {
  throw 'エラー発生！';
  return Future.delayed(Duration(seconds: 1), () => "成功しました");
}

void main() {
  print('開始');

  fetchDataWithError()
      .then((value) => print('成功！'))
      .catchError((e) => print('キャッチ: $e'));

  print('終了');
}
```

<details>
<summary>答え</summary>

```
開始
終了
キャッチ: エラー発生！
```

</details>

---

### 問題 6

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> taskWithError() {
  throw 'エラー！';
  return Future.delayed(Duration(seconds: 1), () => "成功しました");
}

void main() {
  print('スタート');

  taskWithError()
      .catchError((e) {
        print('エラーをキャッチしました: $e');
        return 'リカバリーデータ';
      })
      .then((value) {
        print('.thenが実行されました: $value');
      });

  print('エンド');
}
```

<details>
<summary>答え</summary>

```
スタート
エンド
エラーをキャッチしました: エラー！
.thenが実行されました: リカバリーデータ
```

</details>

---

### 問題 7

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<void> runComplexTask() {
  return Future.delayed(Duration(seconds: 2), () => print('No.2'));
}

void main() {
  print('No.1');

  runComplexTask()
    .then((_) {
      print('No.3');
      Future.delayed(Duration(seconds: 1), () => throw 'Error!');
    })
    .then((_) => print('No.5'));
    
  print('No.6');
}
```

<details>
<summary>答え</summary>

```
No.1
No.6
No.2
No.3
No.5
```

</details>

---

### 問題 8

**問題:** 以下のコードを実行した際のコンソールの出力順を当ててください。

**コード:**
```dart
Future<String> startChain() {
  return Future.delayed(Duration(seconds: 1), () => 'B');
}

void main() {
  print('A');

  startChain()
    .then((b) {
      print(b);
      return Future.delayed(Duration(seconds: 2), () => throw 'C');
    })
    .then((_) => print('D'))
    .catchError((e) {
      print(e);
      return 'E';
    })
    .then((e) => print(e));
  
  print('F');
}
```

<details>
<summary>答え</summary>

```
A
F
B
C
E
```

</details>

---

### 問題 9 (実装問題)

**問題:**
二つの非同期関数を定義し、それらを`.then`で繋げて実行する`main`関数を実装してください。

**仕様:**
1.  最初の非同期関数を作成します。この関数は1秒待機した後に、文字列`"非同期処理の"`を返します。
2.  二つ目の非同期関数を作成します。この関数は1秒待機した後に、文字列`"世界へようこそ"`を返します。
3.  `main`関数で、最初の関数を実行し、その完了後に二つ目の関数を実行してください。
4.  二つの関数の結果を結合し、最終的に`"非同期処理の世界へようこそ"`とコンソールに出力してください。

<details>
<summary>答え（実装例）</summary>

```dart
// 仕様1を満たす関数
Future<String> fetchPart1() {
  return Future.delayed(Duration(seconds: 1), () => '非同期処理の');
}

// 仕様2を満たす関数
Future<String> fetchPart2() {
  return Future.delayed(Duration(seconds: 1), () => '世界へようこそ');
}

void main() {
  String firstPart = '';

  // 仕様3, 4を満たす処理
  fetchPart1()
      .then((part1) {
        firstPart = part1;
        // 最初のFutureの結果を使い、次のFutureを返す
        return fetchPart2();
      })
      .then((part2) {
        // すべてのFutureが完了した後、最終的な結果を出力
        print(firstPart + part2);
      });
}
```

</details>

---

### 問題 10 (実装問題)

**問題:**
意図的に失敗する非同期関数を定義し、そのエラーを`catchError`で処理する`main`関数を実装してください。

**仕様:**
1.  非同期関数を一つ作成します。この関数は1秒待機した後に、`Exception('タスクが失敗しました')`を`throw`します。
2.  `main`関数で、作成した関数を実行してください。
3.  成功時の処理は実行せず、`catchError`を使用してエラーを処理してください。
4.  コンソールに`"エラーをキャッチしました: Exception: タスクが失敗しました"`と出力してください。

<details>
<summary>答え（実装例）</summary>

```dart
// 仕様1を満たす関数
Future<String> failingTask() {
  throw Exception('タスクが失敗しました');
  
  return Future.delayed(Duration(seconds: 1), () {
    return "成功しました";
  });
}

void main() {
  // 仕様2, 3, 4を満たす処理
  failingTask()
      .then((value) {
        // この部分は実行されない
        print(value);
      })
      .catchError((e) {
        // エラーをキャッチしてメッセージを出力
        print('エラーをキャッチしました: $e');
      });
}
```

</details>