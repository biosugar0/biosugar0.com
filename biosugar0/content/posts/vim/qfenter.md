+++
title = "VimのQuickFixでジャンプするときに直前のWindowを使う"
date = "2021-07-22 19:40:00 +0900"
tags = ["Vim"]
slug = "vim-qfenter"
+++


`:vsplit` などでwindow分割しているときにQuickfixリストを使ったとき、デフォルトだとジャンプに使用されるwindowが直前のカーソルがフォーカスしているWindowにならない。
例えばvsplitでwindowを２分割しているとき、左のwindowでvim-lspの `<plug>(lsp-references)` を呼び出すと、Quickfixリストから選択してジャンプしたとき直前にいた左のwindowではなく右のwindowが切り替わってしまう。

直前にいたWindowでジャンプしてほしかったのでGithubを探索していたら、理想の挙動を実現するプラグイン見つけた。

<!--more-->

[yssl/QFEnter](https://github.com/yssl/QFEnter) がそのプラグインだ。
これをインストールするだけで、Quickfixリストからジャンプするときに直前にカーソルがあったWindowがジャンプで使用される。

ずっと気になってた挙動が理想の挙動になって素晴らしい。
