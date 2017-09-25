# Scala Collection

---

## 今日やること
ScalaのCollectionを使えるようにする

### 何故ScalaのCollection？
- Scalaがとりあえず使えるようになる
- Scalaのメリットの中ではでかい
- Rubyとの差がでかい

---

## とりあえずScalaを試す
scalaコマンドでREPLを立ち上げる

```
scala> 1 + 1
res0: Int = 2
```

---

## Collectionってなに？
### 複数の要素を纏めて扱う
Rubyのarrayとかhashみたいなやつ

---

## Option[T]
### nullやnilに代わって使う
Scalaでnullを使うと基本マサカリ
### [T]はGenericsで型引数のようなもの
### サイズが1or0のCollection

---

## Option[T]のSample
```scala
// 中身があるとき
val num: Option[Int] = Some(1)
// 中身が無いとき
val num2: Option[Int] = None
// DBからidのレコードを1件取得するメソッド
def find(id: Long): Option[Hoge] = ...
// 中身があるときだけなんかする
opt.map(_ + 1)
// デフォルト値を使って中身を取り出す
opt.getOrElse(0)
// 成功時と失敗時で処理を分岐する
opt.fold(failFunc)(successFunc)
// nullを返す駄目なAPIをラップする
Option(null) // => None
Option("Success") // => Some("Success")
```

---

## Collection
2以上の要素が持てるやつ

### 抽象型
![抽象型](https://docs.scala-lang.org/resources/images/collections.png)

---

### immutable
![immutable](https://docs.scala-lang.org/resources/images/collections.immutable.png)

---

### mutable
![mutable](https://docs.scala-lang.org/resources/images/collections.mutable.png)

どれ使えばええねん

---

## immutable VS mutable
### immutable
後から変更できない。基本はこっち

### mutable
要素を変更できる。使うとき

- パフォーマンス上必要なとき
- 特定のアルゴリズムが簡潔

---

## [補講] valとvar
変数宣言。valは変更不可。varは可

- 基本はval
- 数行のスコープのvarは許される
- mutable collectionよりはマシ

値を書き換えられる(副作用)は悪

---

## 抽象型と具象型
- Collectionの特性を示す: 抽象型
- 具体的な実装を示す: 具象型

### Seq抽象型とList具象型
- Seqは添字アクセス可能なことを示す
- Listは単方向リストで実装されている

```scala
scala> Seq(1, 2, 3)
res0: Seq[Int] = List(1, 2, 3)
scala> res(2) // Seqなので添字アクセスできる
res1: Int = 3
```

Seqの生成を宣言したが返ってきたのはList

---

## Seq抽象型
- 添字アクセス可能=順序を保存
- サブクラス
  - IndexedSeq抽象型
  - LinearSeq抽象型

最も良く使われる

---

## Map抽象型
- KeyとValueを持つ
- 重複キーが無いことが保証される

Rubyで言うところのHash

---

## Set抽象型
- 添字アクセス不可
- 重複データが無いことが保証される

Hashからvalueを除いたもの

---

### Traversable抽象型
- 何でもいいから値が出てくるやつ
- 何度でも値が取り出せる
- Seq, Set, Mapの親

### TraversableOnce抽象型
- 1度だけ値が取り出せるやつ

Iterator等何度も値が取り出せない奴むけ

---

## よく使う具象型

### Seq > LinearSeq > List
関数型言語では基本の単方向リスト

### Seq > IndexedSeq > Vector
大体何でも早いのでこちらも良く使われる

内部実装は32要素のツリー

---

### Seq > LinearSeq > Stream
遅延リスト。無限リスト等テクニカルな使い道がある

### TraversableOnce > Iterator
Javaにもある。1度しか走査して欲しくないときに使う

デザインパターンにあるIteratorパターン

---

## 生成方法
### 基本
```scala
Seq(1, 2, 3)
List(1, 2, 3)
1 :: 2 :: 3 :: Nil // Listの別パターン(早い
Map(1 -> "1", 2 -> "2")
set.toSeq
set.toList
```

### builderパターン
```scala
val builder = Seq.newBuilder[Int]
builder += 1
builder += 2
builder += 3
builder.result // => Seq(1, 2, 3)
```

---

### 無限Stream
- Streamは処理を遅延するList
- 無限に続くStreamが作れる

```scala
val fibs = 0 #:: 1 #:: fibs.zip(fibs.tail).map { case (x, y) => x + y }
```

### 無限Iterator
ファイル終端までファイルを読み込む例

```scala
val lines = Iterator.continually(readLine())
  .takeWhile(_ != null)
```
