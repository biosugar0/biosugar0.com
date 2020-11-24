+++
title = "gina.vimで別ブランチのファイルを開く"
description = "gina.vimで別ブランチのファイルを開く"
date = "2020-11-24"
tags = ["Vim"]
slug = "gina-show"
+++

別ブランチのファイルを開きたいときに、[https://github.com/lambdalisue/gina.vim](gina.vim)を使っていたら、

```
:Gina show feature/testbranch:hoge.go
```

のようにして開ける。

さらに、画面分割して開きたかったら

```
call gina#custom#command#option(
      \ '/\%(branch\|changes\|grep\|log\|show\)',
      \ '--opener', 'vsplit'
      \)
```

のようにvimrcでvsplitして開くコマンドに `show` を追加設定する。
