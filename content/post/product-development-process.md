---
title: "所属プロダクトの開発プロセス"
date: 2018-07-28T15:44:16+09:00
draft: false
categories: ["development"]
tags: ["environment"]
description: 前チームでの開発プロセスのやり方・流れ。自分用メモ
---

来週から期間限定で違うチームに異動するため、今のチームの開発プロセスをなるべく詳細に書き残しておこうと思う。
チームを異動して戻ってきた時に見返した時に何か得られるような気がするので。

# 進め方

進め方は全体的にやんわりスクラムで進めるような感じ。
やんわりというのは、スクラムマスターが居てちゃんとしたスクラムのやり方を実践していこう！というわけではないので、割と中途半端なスクラムでやってると思う。

## チームメンバーの構成

- 開発責任者 1人 ※後述の朝会・レビュー会には参加しない
- PO 1人
- サーバーサイド・インフラエンジニア 2人
- iOSエンジニア 2人
- フロントエンドエンジニア 1人

## 使ってるツール

- GitHub
- ZenHub（GitHubの拡張）
- esa
- instagantt
- スプレッドシート（KPTのシート等）

## やっている事ハイライト

1. 朝会（デイリースクラム）
2. レビュー会
3. 振り返り（スプリントレトロスペクティブ）
  - スプリントレビュー
  - スプリントプランニング
  - KPT
4. スプリントプランニングミーティング（スプリント計画）
5. 分報（雑談）

![雰囲気イラスト](/images/kaigi-shifuku.png)

### 1週間の予定

| 曜日 | 内容 |
| --- | --- |
| **月** | 10:00~朝会 <br/> 11:00~レビュー会 |
| **火** | 10:00~朝会 <br/> 11:00~レビュー会 <br/> 18:00~振り返り（スプリントレトロスペクティブ） |
| **水** | 10:00~スプリントプランニングミーティング（スプリント計画） |
| **木** | 10:00~朝会 <br/> 11:00~レビュー会 |
| **金** | 10:00~朝会 <br/> 11:00~レビュー会 |

### 1.朝会（デイリースクラム）

定時が10~19時であるため、10時から朝会を行う。
参加メンバーはチーム全員。
朝会では4つのコーナーがある。

- 開発メンバーのタスク進捗共有
- New Issueの確認
- POのタスク進捗共有
- 共有事項（挙手制で勤怠の連絡等があれば伝える）

遅刻するメンバーが来るまで待ってから朝会をする事はなく、到着しているメンバーだけで朝会を行うようにしている。
遅刻するメンバーが多くいた時は夕会も検討したけど実施はしなかった。

### 2.レビュー会

毎日11時〜12時まで、レビュー会がある。
参加メンバーは開発メンバーだけ。開発責任者とPOは参加しない。

スプリントレビューではない。

#### やること

- プルリクのレビュー
- 設計した内容のレビュー（esaなど）
- 空いた時間で雑談

#### レビューはほぼ全て行う

スプリントレビューではないと説明したが、実際に動くものを見ながらレビューする場合もある。

開発メンバー全員のコードをレビューする（サーバーサイド、Webフロント、iOS）。
現段階では開発メンバーの数が少ないので問題になってないけど、開発者の人数が多くなってきた場合はこの会はそれぞれの領域毎に分けたほうが良いものだと思う。

今のチームは開発メンバーが5人であるためレビューは大体1時間以内に終るし、ちゃんとワークしてると思う。

レビュー会で認識の齟齬や考慮漏れがかなり解消されてる気がする。
そのため、 `この領域はレビューしなくていいや` ・ `このソースコードは個人的にこの人にレビューしてもらおう` とならないように気をつける必要がある。

もちろん急ぎでリリースが必要なものについては、個別でレビューしてもらってる感じ。

### 3.振り返り（スプリントレトロスペクティブ）

火曜日の終業前（18時〜19時）に行う。

以下を順番に行う。

#### やること

1. 導入進捗率の確認
- チケットの進捗確認
- 問い合わせ内容の共有
- ユーザーの声共有
- KPT
- スプリントレビュー

#### Problemを洗い出す事を意識して進める

振り返り（スプリントレトロスペクティブ）では、この1週間を振り返ると同時に、1週間で起こったことからプロブレムを洗い出す事を意識してミーティングを行っている。

##### チケットの進捗確認

ZenHubのボードをざっと確認する感じで、チケットの進捗を確認を俯瞰的にと行います。

##### 導入進捗率の確認
ユーザーのインストール数のようなものです。
この数字に届くのか届かないのか、目標に対しての進捗等の共有を行います。
スプレッドシートで数字が集計されており、表で前回差分が確認できるようになっており、それを全員で共有します。

##### 問い合わせ内容の共有
はユーザーから実際にあった問い合わせ内容を確認します。

##### ユーザーの声共有
実際に「こういう機能が欲しい」というものや、「ここが使いにくい」という声をまとめたものを共有します。
これによってユーザーがどんな機能を求めているかを感じ取る事ができます。
自分はなるべく、「何ができなくてユーザーはこう言ってるのか」という Problemに落とせるように意識しています。

##### KPT
ごく普通のKPTだと思います。
スプレッドシートは水曜0時頃にスクリプトで自動生成され、スプリント開始（水曜10時〜）スプリント終了（火曜18時）までの間に書き込めるようになっています。
この振り返りミーティングでは、更に5分間の書き込み時間を設けて、全員に書き込んでもらうようにしています。

##### スプリントレビュー
1週間で作った成果物を全員で共有します。
挙手制でレビューできる人はする形となっています。

### 4.スプリントプランニングミーティング（スプリント計画）

水曜日の始業直後（10時〜）から行う

#### やること

- 今回のスプリントで行う内容の決定（全員で話し合いの上決める）
- 決定したタスクの共有
- スプリントゴール決め

#### 全員の認識が取れた状態でスプリントを開始できるように意識している

タスクは重さや優先度もそれぞれ違うので、重すぎるタスクに着手してタスクのステータスが全然変わらないので進捗がわからない、という事が無いようにしている。

##### タスクの粒度は意識してわける

まず全員の進捗が毎日わかりやすくなるように、タスクはなるべく細かく分割していく。

例えば、以下のようなタスクがあった場合と

```task.txt
ユーザー問い合わせ機能開発
```

以下のようなタスクがあった場合とでは、タスクのステータスが頻繁に動く↓のほうが進捗がわかりやすい

```task.txt
ユーザー問い合わせ機能・設計
ユーザー問い合わせ機能・APIモック開発
ユーザー問い合わせ機能・サーバーサイドバリデーション実装
ユーザー問い合わせ機能・サーバーサイド登録処理実装
ユーザー問い合わせ機能・フロントエンド画面実装
ユーザー問い合わせ機能・結合テスト
ユーザー問い合わせ機能・リリース作業
```

##### 決定したタスクの共有で全員の認識を合わせる

やるタスクを決めた後に再度、誰が何をやるかを一人一人再確認していく。

もし疑問や話し合いたいものがある場合は、OST（オープンスペーステクノロジー）で議論をする。

### 5.分報（雑談）

#### 取られているコミュニケーション

- 雑談
- わからない事をボヤく
- 気になったWebサイトのシェア

気軽にやり取りできるコミュニケーションの場として活用している。

# 最後に

バックログはPOだけが管理しているので一応あるけどほぼノータッチな感じです。たまに `これってやらなくて良いんでしたっけ` みたいなつつきを開発メンバーから入れる事はあります。

ストーリーポイント・ストーリーポイントの計測は以前は行っていたけど、誰がどのくらい重いものをやってるかというのは認識が合っている状態でスプリントが開始されるのと、計測したからといってどう活かすかという事が見えなかったので廃止した。

異動先チームでのやり方についてもそのうち投稿したいと思う。
