---
title: "多相について今一度調べてみた"
date: 2018-11-24T13:17:05+09:00
draft: true
---

## なぜ調べたのか

エンジニアになって5年くらいが経つけど、恥ずかしい事に意外と基礎の基礎を知らなかったりする。
Scalaの勉強してて `高カインド多相` なるものが出てきて他の多相も気になり調べてまとめてみた。
Wikipediaや、調べて出てきた他の方のブログ、本なんかを参考にしてます。
間違いがあったら🙏

## 多相性・多態性・多様性・ポリモーフィズムとは

- 全部同じ
- これらの実態は性質

## 部分型多相・派生型多態（サブタイピング多相・部分型付け）とは

 - この多相を指して単にポリモーフィズムと呼ぶ場合がある。
 - 複数種類の型を１つの型として扱える性質。

```Subtype.scala
trait Animal {
  def call(): String
}
case class Cat() extends Animal { override def call(): String = "bark" }
case class Dog() extends Animal { override def call(): String = "mew" }
case class Cow() extends Animal { override def call(): String = "low" }
```

Animal型としてCat,Dog,Cowを扱うという事は部分型多相である。
例えば以下の関数のシグニチャは全て部分型多相となる。

```signture.txt
// 引数が Animal であるため部分型多相
def connectLeads(a: Animal): Unit
// 戻り値が Animal であるため部分型多相
def randomAnimal(): Animal
```

見た目上では、型によらない処理を記述する事が可能である。
例えば `connectLeads` では、`Animal`が `Cat` か `Dog`であるかは関知しない。

`randomAnimal` でも同様である。

この多相は `Animal`の部分型として `Cat,Dog,Cow` が存在しており、それらは以下のように表記できる。
（この表記が何なのかは知らない）

```Example.txt
Cat <: Animal
Dog <: Animal
Cow <: Animal
```

ScalaやJavaは、extends(継承)によって部分型多相の性質を提供している。

## アドホック多相とは

- Javaでいうオーバーロードがそれに値する性質。
- Scalaで実現すると以下のようになる。（と思う

```Adhoc.scala
case class Book()
case class MobilePhone()
case class Computer()
class SecondHandBuyer {
  def sell(book: Book): Int = 100
  def sell(mobilePhone: MobilePhone): Boolean => Int = isSetAccessories => if (isSetAccessories) 20000 else 17000
  def sell(computer: Computer): Int = 40000
}
```

決め打ちで本、携帯、パソコンがそれぞれ 100円、20,000円 or 17,000円、40,000円で売れるというメソッド。

このような同名で全く関係ない異なる型の引数・戻り値に対応、関数の振る舞いはそれぞれ異なるような関数をアドホック多相という。

コンパイラから見るとこれらは全く別個の関数3つとして見られている。

`アドホック` （その場しのぎ）の由来は、型システムの基本的な機能でない事を指すため `アドホック` という名前になっている。

## パラメータ多相・パラメトリック多相

- 多相性をパラメータ多相と呼ぶ場合がある。
- 特定の型を指定せず書ける性質、オブジェクト指向ではジェネリクス、Scalaでは以下のようになんでも入る型Aを指す時に使う。
- Go言語にはこれが無い。（1.11時点）

```Parameter.scala
def get[A](l: List[A], no: Int): A = l match {
  case _ if l.length < no => throw new Exception
  case h :: Nil => h
  case _ :: t => get(t, no - 1)
}
get(List(1,2,3), 3)
get(List("a", "b", "c"), 3)
```

`get` には、IntでもStringでも型はなんでも問わない。

## 高階多相・高カインド多相とは

- Scalaでよく見る `[F[_]]` が該当。HaskellやC++にも同等の性質があるらしい。
- 型コンストラクタ（≠ 普通の型）を受け取り型を返す性質で、型コンストラクタは `List, Stream, Option` などが該当する。
- 例えば以下のモナドのトレイトとファンクタのトレイトは高階多相。

```HigherKind.scala
trait Monad[F[_]] {
  def unit[A](a: => A): F[A]
  def flatMap[A,B](ma: F[A])(f: A => F[B]): F[B]
}

trait Functor[F[_]] {
  def map[A,B](fa: F[A])(f: A => B): F[B]
}
```

## その他:ダックタイピング

- Perl、Python、Ruby、Goで採用される作法。
- Go言語で実現すると以下のようになる。

https://play.golang.org/p/EsYs9NMmit-

`Ahiru` と `Karasu` は `Tori` インターフェースを満たしているので、 `Tori` として扱える。

> もしもそれがアヒルのように歩き、アヒルのように鳴くのなら、それはアヒルである

という言葉があり、そのインターフェースの通りに実装すればその型として扱えるというもの。
Go言語の場合、ダックタイピングを使う事で初めて部分型多相を実現できる。

