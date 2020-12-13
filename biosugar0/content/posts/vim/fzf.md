+++
title = "fzf.vimでクリップボードからコピペする"
description = "fzfでクリップボードからコピペする"
date = "2020-12-13"
tags = ["Vim"]
slug = "fzf-clipboard"
+++


[2020-09-12のコミット](https://github.com/junegunn/fzf/commit/c60ed1758315f0d993fbcbf04459944c87e19a48)
で、[fzf](https://github.com/junegunn/fzf)のデフォルト表示がpopup windowなった。
これはこれでいいのだが、popup window表示の場合、クリップボードのペーストができずに、ペーストしようとすると
popup windowが閉じてしまう。
ちょっと困るので、以前の表示方法に戻した。

```
let g:fzf_layout = { 'down': '40%' }
```

調査の過程で、terminal modeでも通常のように貼り付けできないことがわかったので、

```
tnoremap <expr> <C-v> getreg("")
```

のように設定してCtrl+vで貼り付けできるようにした。


残念ながらfzfのpopup window表示でこれは効かないようだ。
