---
title: "チーム期間限定異動2週間目"
date: 2018-08-08T22:17:08+09:00
draft: false
categories: ["Scala"]
tags: ["初心者"]
---

# 6日目


```import.scala
// ※記事中の `DateTime型は org.joda.time.DateTimeです`
import org.joda.time.DateTime
```

週次の計画MTGの後に、ペアプロでリファクタリングを行った。

Scalaの `ス` の字もわからない状態でJOINしたので、とりあえず書いてみたコードをペアプロで認識合わせつつリファクタリングを行った。

元のコードは `var`, `if` が存在するコードで、ゴールとしては `var`, `if`, `case` を無くしたようなコードを書く事を目標としてリファクタリング開始。

実際のリファクタリング対象のコード中に、DateTimeの範囲を年月単位のタプルに分割する処理があった。例えば以下の様な感じ

```input.txt
// input
from: 2018-01-04, to: 2018-05-08
```

```output.txt
// output
(2018-01-04, 2018-01-31)
(2018-02-01, 2018-02-28)
(2018-03-01, 2018-03-31)
(2018-04-01, 2018-04-30)
(2018-05-01, 2018-05-08)
```

自分は以下のようなコードを書いてた。

```uncode.scala
def getRange(fromDate: DateTime, toDate: DateTime): Stream[(DateTime, DateTime)] =
  Stream
    .range(0, Integer.MAX_VALUE)
    .map(i => {
      var rangeFrom = fromDate.plusMonths(i).dayOfMonth.withMinimumValue()
      var rangeTo   = fromDate.plusMonths(i).dayOfMonth.withMaximumValue()
      // 期間の開始日
      if (i == 0) rangeFrom = fromDate
      // 期間の終了日
      if (toDate.compareTo(rangeTo) == -1) rangeTo = toDate

      (rangeFrom, rangeTo)
    })
    .takeWhile(range => (range._1.compareTo(range._2) != 1) && (range._2.compareTo(toDate) != 1))
```

Javaをかじった程度の知識で Streamを触ってこうなった。

 - 処理としては、 `range` で処理をずっと繰り返し、 `map` で from, to を見つつ当月分のタプルを `(from, to)` 生成
 - `takeWhile` にて終了条件を記述

`var` も `if` も使ってるので、このコードはリファクタリング対象となった。

リファクタリング時には関数型の Tipsをいくつか教えて頂き、それを意識してコードを記述した。

 - `var` 禁止
 - `if` 半禁止
 - `++` （Stream結合）はスタックアンセーフ
 - `zip` をうまく使う。 `zip` は少ない方が基準となる

zipを教えて頂いたのはデカかった。これを知るだけで以下のように書けるようになる。

```疑似コード.scala
(fromのStream) zip (toのStream)
```

ある程度ペアプロでリファクタリングした所で、持ち帰って自分でリファクタリングの続きをする事となった。

社内の勉強会で `Scala関数型デザイン` (https://amzn.to/2Og2kQS) の輪読会をしていて、遅延評価と無限ストリームを使った処理が書けるのをひらめいたので以下のように書いてみた。

```kirei.scala
private[this] def getFromsByMonth(fromDate: => DateTime): Stream[DateTime] =
  fromDate #:: getFromsByMonth(fromDate.plusMonths(1).dayOfMonth().withMinimumValue())

private[this] def getTosByMonth(toDate: => DateTime): Stream[DateTime] =
  toDate #:: getTosByMonth(toDate.minusMonths(1).dayOfMonth().withMaximumValue())

private[this] def rangesByMonth(fromDate: DateTime, toDate: DateTime): Stream[(DateTime, DateTime)] =
  getFromsByMonth(fromDate) zip getTosByMonth(toDate).takeWhile(t => t.compareTo(fromDate) == 1).reverse
```

- getFromsByMonth: DateTime型を受け取り、翌月1日をずっと生成する関数
- getTosByMonth: DateTime型を受け取り、前月末日をずっと生成する関数
- rangesByMonth: `getFromsByMonth` と `getTosByMonth` を呼び出し zip をする。 `getTosByMonth` は有限となるよう takeWhileでループ条件を記載

これで遅延評価＆無限ストリームで処理されるようになる。

### 6日目の感想

本で勉強したことが業務コードに落とし込めて良かった。

記事内で紹介してる `Scala関数型デザイン` の本は所々難読な箇所があって進めにくい感じだったけど、ある程度勉強しておいてよかった。

記事内の遅延評価＆無限ストリームを使ったコードは `rangesByMonth` 関数が `reverse` をしているためスタックアンセーフとなってしまっているのでもう少しどうにか考えてスタックセーフなコードが書けたのかもしれないと若干後悔。

---

# 7日目

7日目は主に前のチームの仕事をした。

自分主体で動かしていたタスクの確認作業があったので、その作業を半分、半分はレビューしてもらったコードの修正とテストコードを書いた。

元々テストコードの無いリポジトリだったけどテストコードを書かせてもらった。
CIはCircleCIの設定が書いてあり、ちゃんと稼働したのでCIの構築はしなかった。

テストコードを書いてみた件については別の記事で書こうと思う。

### 7日目の感想

前のチームの仕事が入ってきて異動先チームの事はあまりできなかったけど、Scalaでテストコードを書く貴重な経験ができた。

---

# 8日目

前のチームの計画MTGの後、台風のため帰宅したので特になし。

### 8日目の感想

同様に特になし

---

# 9日目

なんの変哲も無い staticなメソッドを書いてレビューして頂いた。

### 9日目の感想

---

特になし

# 10日目

異動先チームで今日までで自分が作った物をリリースした。

手取り足取り教えて頂いたチームの皆様に感謝🙏
そして、開発環境で結合テストする前にリリースしちゃう痛恨の初心者ミスを犯して本番で少しの間バグらせてしまうという気の緩んだ事をしてしまい反省🙇🙇🙇🙇

午後は異動前のチームで自分に完全に属人化している範囲の問い合わせが来ていたのでその対応を行った。

### 10日目の感想

リリースの件に関しては猛反省🙇

11日目からはSQLのチューニングをする。

異動前チームの属人化はなるべく無くしていきたい。

今回自分がチームから離れる事で属人化してる部分が浮き上がってきたので、これをどううまく処理するか考えていきたい。

自分の周りだけかもしれないけど、スコープを絞って属人化する事が多々ある気がする。

属人化で自分が感じるには以下のようなメリットデメリットがあると思う。

- メリット
 1. 共有する必要が無く脳内インデックスが効くので受け答えが早い
 2. 実装速度が早い
 3. 会社や環境によるけど、自分のやってみたい技術でチャレンジできる可能性がある
 4. ビジネスの速度向上が可能
- デメリット
 1. 孤独で不安
 2. 視野が狭くなりがち
 3. 長く続ける事による他人の影響が受けられない事による曲がった成長がある可能性
 4. 担当者の不在時に発生したダウンタイムへの対応

`1` は本当にこの実装、このコードで良いのか、これであってるのか、わかりにくくないか、これを採用して良いのか、などの不安。
`2`,`3`は同じ意味かもしれない。
`4` は属人化してるスコープによる所はあると思うけど、それ以上に会社や環境によって精神衛生上良くないと思う。旅行とかも落ち着いて行けない気がする。

環境にもよるけどデメリットはある程度のスキル・経験があれば `ある程度は` 回避できる気がする。
例えばどこの範囲まで属人化するか、ここは共有しておく、みたいな事をやっていけばそんな気がするのでちゃんと切り分けて全てがプラスになるようにタスクを調整していきたい。

> `例えばどこの範囲まで属人化するか、ここは共有しておく、みたいな事`

この辺がうまくできたら属人化じゃないじゃんって言われたらその通りかもしれない…。

異動前のチームで、自分が興味があって採用してもらった技術とかをその技術に関して特に興味無い人に共有したり属人化辞めたりするのに押し付けなんじゃないかと気を使っちゃう…。Dockerとか。ElaticBeanstalkとか。Go言語とか。
塩梅が難しい。

---

### 今週の感想

リリースに関しての痛恨の初心者ミスを反省🙇

Scala力の向上を感じていて非常に有意義な時間を過ごせている。期間限定のため、あと1ヶ月くらいしか居ないのが非常に残念。
