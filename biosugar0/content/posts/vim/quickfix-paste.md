+++
title = "複数のファイルの特定の文字列の下に行をペーストする"
description = "複数のファイルの特定の文字列の下に行をペーストする"
date = "2021-01-23"
tags = ["Vim"]
slug = "quickfix-paste"
+++


複数のファイルに存在する特定の文字列の下に数行のコードをペーストするタスクがあり、Vimで簡単にできたのでメモ。

## 検索

特定の文字列を検索するのは、 `:grep` を使った。
ripgrepを愛用しているので、以下のように使用するプログラムを変更している。

```vim
set grepprg=rg\ --vimgrep\ --no-heading
set grepformat=%f:%l:%c:%m,%f:%l:%m
```

## QuickFix

検索結果はquickfixリストとして出力される。
それらの行の下にペーストしたい。
これにはquickfixリストやlocationリスト中のバッファに対してコマンドを実行する`:cdo`
を使った。

```
:cdo normal p
```

ペーストする行はあらかじめyunkしてあるため、ノーマルモードの`p`を実行することでquickfixリストの全行に対象の行がペーストされる。
