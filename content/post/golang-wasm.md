---
title: "Go言語でwasm動かしてみた"
date: 2018-07-22T19:38:54+09:00
draft: false
categories: ["Go"]
tags: ["WebAssembly"]
---
wasmとは何かを調べてみて、Goでブラウザで動かしてみました。

## WebAssembly とは

- 略してwasm。
- メジャー4ブラウザ（Chrome、Edge、Firefox、Safari）でサポート
- ブラウザ上でGo等でコンパイルされたバイナリが動かせる👐
- 同じWeb API（Javascriptが操作できる領域、プロセスのVM?）でJsのコンテキストの呼び出しや、メソッドを呼び出せる感じ。
- ネイティブに近いパフォーマンスで動作する

### WebAssembly登場の背景

- コードのサイズが大きい場合のダウンロードのコストが高いという問題に対応するため、WebAssemblyが登場。
- 普通にWebやってると感じないと思うけど、ブラウザ上で動作するゲーム・3Dを使ったサービスのJsコードとかは数十MBあるらしいのでサイズ問題は無視できない。
![wasm雰囲気イラスト](/images/go-wasm-illust.png)

### 使える言語

- LLVMに変換できる言語なら動くらしい。（C・C++・Objective-C・Rust・Go?）
- kripken/emscripten というツールで LLVM to wasmが可能

### 将来的に

- Jsからwasmに置き換わる事はない
- Javascriptを補完するだけ

## Goで触るには

wasmは `go1.11` 系のバージョンから使用可能。

`go1.11` 系は正式にリリースされていないので、beta版となる。

僕は `go1.11beta2` をインストールして試しました。

※go1.11未満だとエラーになります

```failure.bash
> GOARCH=wasm GOOS=js go build -o test.wasm main.go
cmd/go: unsupported GOOS/GOARCH pair js/wasm
```

実際に触ってみるにあたり、golangのwasmをブラウザ上で実際に動かせるこちらのチュートリアルをやってみた https://blog.gopheracademy.com/advent-2017/go-wasm/

## 触ってみて

Goでビルドしたコードがブラウザで動いて感動。

wasmファイルのレスポンスヘッダの Content-Typeが `application/wasm` となっていた。

ネイティブに近いパフォーマンスという所についてはちゃんと検証してないのであまりわからなかった。

Golangは処理の速さに定評があるので、コレひとつ覚えるだけででサーバーサイドもフロントエンドも速度出せる感じのモノが作れるように今後なるのかなと思った。

## 実際に使えるのか

メリット・デメリットを見る感じ、今の所活かせそうな分野でいくとゲーム・3D描画・重い処理する系の所くらい…？という所感でした。

そのためWebシステム屋としては今の所、まだ使う所はしっかり想像できませんでした。
ネイティブに近いパフォーマンスで動いたとしても、フロントでそんな重い事ゴリゴリにやらないんじゃないかなーという印象です。

ただ、ある程度の棲み分けができれば開発者にとってのメリットはありそうだなと思いました。
例えば、JS + wasm(Go) で、主な処理はGoに書いてJSはDOM周りだけを担当するみたいな感じで責任範囲を分けて、なるべくサーバーサイドと同じ言語で書くみたいな。

周辺のエコシステムによってできることが増えていくと思うので、今後の発展に注目したいと思います。

## 参考にしたサイト

https://developer.mozilla.org/ja/docs/WebAssembly/Concepts

