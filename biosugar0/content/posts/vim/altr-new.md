+++
title = "vim-altrを使ってGoのテストファイルにジャンプする際に存在しなかったらファイルを作る設定"
description = "vim-altrを使ってGoのテストファイルにジャンプする際に存在しなかったらファイルを作る設定"
date = "2021-08-26 01:03:00 +0900"
tags = ["Vim","Go"]
slug = "vim-altr-new"
+++

Goでテストを書くときに、Vimで書いていてテストファイルとの行き来には[kana/vim-altr](https://github.com/kana/vim-altr)をつかっている。
その際に、まだテストファイルを作っていないことがあって自動で作ってほしかったので設定した。

<!--more-->

その設定は以下のようなもので、
normal modeの `<leader>t` でvim-altrを呼び出す前に自作のGoのテストファイルが存在しなかったら作成する関数を呼んでいる。
これで、移動した際にファイルが存在しなかったら作成してからジャンプすることができる。

```vim
function! s:makegotest() abort
  let src=expand("%:p:r")
  let filex=expand("%:p:e")
  if filex != "go"
      return
  endif
  if expand("%:p:r") !~ "test$"
      let src.="_test"
  endif
  let src.=".go"
  let chk=getftype(src)
  if chk == "file" 
      return
  endif
  call writefile(["package"],src)
endfunction

nnoremap <SID>(makegotest) <Cmd>call <SID>makegotest()<CR>
nmap <leader>t <SID>(makegotest)<Plug>(altr-forward)
```
