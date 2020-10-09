+++
title = "Vimのpopup windowに表示されたテキストを翻訳して表示する"
date = "2020-07-14"
tags = ["Vim"]
slug = "vim-ptranslate"
+++

[前回の記事](http://localhost:1313/posts/2020/07/vim%E3%81%AEpopup-window%E3%81%AB%E8%A1%A8%E7%A4%BA%E3%81%95%E3%82%8C%E3%81%9F%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88%E3%82%92yank%E3%81%99%E3%82%8B%E3%83%97%E3%83%A9%E3%82%B0%E3%82%A4%E3%83%B3vim-popyank/)で述べたvim のpopup windowに表示されたテキストをyankするプラグイン[vim-popyank](https://github.com/biosugar0/vim-popyank)と、google翻訳をするプラグイン[translate.vim](https://github.com/skanehira/translate.vim)を使って便利な設定をしたのでメモ。

以下のように[vim-lsp](https://github.com/prabirshrestha/vim-lsp)経由の警告popup(もちろん他のpopupも)を翻訳することができる。

![ptranslate](/images/ptranslate.gif)


## 目的

vim-lspで表示されたpopup windowのテキストを簡単に翻訳する。(ついでに原文をyank。後でググるかもしれないので)

## 設定

vim-popyankにはpopup windowのテキストを取得する関数が含まれている。
仕組みとしては、それを使って取得したテキストをtranslate.vimに渡すだけだ。
vim-popyankとtranslate.vimがインストールされている状態で、`.vimrc`かどこかに以下の設定をする。

```vim
function! s:Ptranslate()
    call popyank#exec()
    let pop = popyank#pop()
    if len(pop) == 0
        echom "no popup window"
    endif
    exec 'Translate'. " ".pop
endfunction

command! -nargs=0 Ptranslate call s:Ptranslate()
nmap <leader>y :<C-u>Ptranslate<CR>
```

まず、popup window テキストの翻訳関数を定義している。

ここではまず、`popyank#exec()` でpopup windowテキストをyankしている。(あとでググったりする用)


その後、`popyank#pop()` でテキストを取得し、

`exec 'Translate'. " ".pop`でtranslate.vimのコマンド`Translate`に変数に入れたテキストを渡して翻訳する。

するとtranslate.vimが翻訳結果をpopup windowに表示してくれる。

この関数を実行するだけでもいいのだが、
最後の2行で`<leader>y` をタイプするだけで実行されるようにマッピングした。


これで思い立ったときにすぐpopupされたテキストを翻訳、yankできて便利。
