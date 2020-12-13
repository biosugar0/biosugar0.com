+++
title = "fzf.vimでクリップボードの中身をペーストする"
description = "fzf.vimでクリップボードの中身をペーストする"
date = "2020-12-13"
tags = ["Vim"]
slug = "fzf-paste"
+++


[2020-09-12のコミット](https://github.com/junegunn/fzf/commit/c60ed1758315f0d993fbcbf04459944c87e19a48)
で、[fzf](https://github.com/junegunn/fzf)のデフォルト表示がpopup windowになった。
これはこれでいいのだが、popup window表示の場合、クリップボード,レジスタの中身のペーストができずに、ペーストしようとすると
popup windowが閉じてしまう。
そこで色々調査した結果、以下のように設定すると無名レジスタの中身をコピーできることがわかった。

<!--more-->

前提として、以下の設定がしてある。

```vim
set clipboard=unnamed,unnamedplus " OSのクリップボードをレジスタ指定無しで Yank, Put 出来るようにする
```

これでシステムのクリップボードにコピーしたものは無名レジスタに入りVimからアクセスできる。

そして、以下のように設定すると、terminal mode, popup window中のterminalでもCtrl + vで無名レジスタの中身をペーストできる。


```vim
tnoremap <expr> <C-v> getreg("")
```
