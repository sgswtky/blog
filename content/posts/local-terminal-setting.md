---
title: "ローカルMacのターミナルを個人的に良い感じにした"
date: 2018-07-09T17:08:18+09:00
draft: false
categories: ["local"]
tags: ["iTerm2", "Terminal", "git"]
---

## きっかけ

Macのバッテリーが膨らみすぎて、交換してもらった。
性能面では特に問題なかったのに新品の希望通りのPCを手配して頂いてこの辺の柔軟さ・理解がある所には会社に感謝🙏

がっつりイジり倒して玄人感あるターミナルっていうよりは基本機能に配色調整したシンプルなターミナルにしました。

## いままで

- iTerm2
- zsh
- oh-my-zsh
- デフォルトテーマ

## これから

- iTerm2
- iTerm2のフォント変更
- japaneseque（配色）
- zsh
- oh-my-zsh
- avit（avit）
- ターミナルの表示カスタマイズ（ユーザーとか表示される部分）

### iTerm2

- https://www.iterm2.com/

### iTerm2のフォント変更

- `13pt Menlo Regular` に変更して使ってます
  - InteliJIdeaが Menloなのでそちらに合わせる形で設定しました

### japaneseque

- iTerm2の配色の設定ファイル
  - https://github.com/aereal/dotfiles/blob/master/colors/Japanesque/Japanesque.itermcolors
- 作者のブログ記事
  - https://this.aereal.org/entry/2013/01/02/222304

### zsh

- wikipedia
  - https://ja.wikipedia.org/wiki/Z_Shell
- macだとbrewで入ります。入れ方はググると出てきます。
  - https://qiita.com/nenokido2000/items/763a4af5c161ff5ede68

### oh-my-zsh

- github（入れ方もREADMEにあります）
  - https://github.com/robbyrussell/oh-my-zsh

### avit

- `~/.zshrc` の `ZSH_THEME` を、`ZSH_THEME="avit"` にするだけ

### ターミナルの表示カスタマイズ

ここまではインストール＆設定するだけでしたが、この表示カスタマイズだけ色々イジりました。

- ブランチ表示変更の参考にした記事
  - https://qiita.com/Kakuni/items/a8025e075926272f491d
-　`# ls collor` の所で、zshのls配色設定は見えづらいため変更しています
  - http://neko-mac.blogspot.com/2015/03/mac_18.html

```zsh.bash
mkdir ~/Settings
touch ~/Settings/my-zsh
```

```my-zsh
# ls collor
export LSCOLORS=gxfxcxdxbxegedabagacad

# zsh
PROMPT='%{${fg[green]}%}⌚ %* %{${fg[cyan]}%} 📁 %~ %{${reset_color}%}
▶ '
# git branch color view
function rprompt-git-current-branch {
  local branch_name st branch_status

  if [ ! -e  ".git" ]; then
    return
  fi
  branch_name=`git rev-parse --abbrev-ref HEAD 2> /dev/null`
  st=`git status 2> /dev/null`
  if [[ -n `echo "$st" | grep "^nothing to"` ]]; then
    # clean
    branch_status="👍 %F{green}"
  elif [[ -n `echo "$st" | grep "^Untracked files"` ]]; then
    # editing
    branch_status="%F{red}?"
  elif [[ -n `echo "$st" | grep "^Changes not staged for commit"` ]]; then
    # There is a file which is not add
    branch_status="😀 %F{red}+"
  elif [[ -n `echo "$st" | grep "^Changes to be committed"` ]]; then
    # There is a file which is not commit
    branch_status="😆 %F{yellow}!"
  elif [[ -n `echo "$st" | grep "^rebase in progress"` ]]; then
    # conflict
    echo "🤯  %F{red}!(no branch)"
    return
  else
    branch_status="%F{blue}"
  fi
  echo "${branch_status}[$branch_name] %{${reset_color}%}"
}
# プロンプトが表示されるたびにプロンプト文字列を評価、置換する
setopt prompt_subst

# プロンプトの右側(RPROMPT)にメソッドの結果を表示させる
RPROMPT='`rprompt-git-current-branch`'
```

こんな感じになります。

![ターミナル参考画像](/images/local-terminal-example.png)


### iTerm2の設定

- 配色の設定とか書いてないけど若干変えてる所があるので設定をエクスポートしておきました。

[settings.json](/files/iterm2-settings.json)

