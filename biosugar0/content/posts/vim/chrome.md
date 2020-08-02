+++
title = "VimでChromeを操作するプラグインchrome.vimを作った"
description = "VimでChromeを操作するプラグインを作った"
date = "2020-08-02"
tags = ["Vim","vim-plugin"]
+++

普段ターミナル上のVimでコードを書いていると、ちょっとググりたいときにChromeを操作する。
その後にVimに戻ってくるときは、Chromeがアクティブになっているのでターミナルをクリックして再活性化しないといけない。
ちょっと面倒だったので、VimからChromeを操作したくなった。

ググっているとちょうどやりたいことをほぼやっている記事[Chromeでのブラウジングを快適にするTips](https://qiita.com/syui/items/d3c0721de71ebbe6461e)を見つけた。

これを参考にして、VimからChromeを操作して

* google検索
* スクロール
* タブを閉じる
* 任意の文字をChromeに送る
* 任意key codeをChromeに送る

コマンドが使えるプラグイン[chrome.vim](https://github.com/biosugar0/chrome.vim)を作ってみた。
Chrome操作時にはchromeがアクティブになってしまうので、Vimに戻ってくるために操作後に
ターミナルに戻ってくる処理も実装している。

<!--more-->

任意の文字、key codeをVim からChromeに送れるというのがミソで、
これを利用するとChromeの拡張のVimiumとの連携ができ、設定するといろいろな操作ができるようになる。

自分は[vim-submode](https://github.com/kana/vim-submode)を連携して以下のように設定している。

```vim
nmap mc  :<C-u>ChromeStroke
let g:chromevim#scroll=200


let g:submode_timeout=v:false

" scroll down and enter chromectl mode.
call submode#enter_with('chromectl', 'n', 'r', '<leader>j', '<Plug>(ChromeDown)')
" scroll up and enter chromectl mode.
call submode#enter_with('chromectl', 'n', 'r', '<leader>k', '<Plug>(ChromeUp)')
" close current tab and enter chromectl mode.
call submode#enter_with('chromectl', 'n', 'r', '<leader>x', '<Plug>(ChromeTabClose)')
"

" in chromectl mode{{{
" scroll down
call submode#map('chromectl', 'n', 'r', 'j', '<Plug>(ChromeDown)')
" scroll up
call submode#map('chromectl', 'n', 'r', 'k', '<Plug>(ChromeUp)')
" close current tab
call submode#map('chromectl', 'n', 'r', 'x', '<Plug>(ChromeTabClose)')
" Open a link in the current tab by Vimium
call submode#map('chromectl', 'n', 'r', 'i', ':<C-u>ChromeStroke "i"<CR>')
" Go one tab left by Vimium
call submode#map('chromectl', 'n', 'r', 'H', ':<C-u>ChromeStroke "H"<CR>')
" Go one tab right by Vimium
call submode#map('chromectl', 'n', 'r', 'L', ':<C-u>ChromeStroke "L"<CR>')
" Go forward in history by Vimium
call submode#map('chromectl', 'n', 'r', 'l', ':<C-u>ChromeStroke "l"<CR>')
" Go back in history by Vimium
call submode#map('chromectl', 'n', 'r', 'h', ':<C-u>ChromeStroke "h"<CR>')
" enter caret mode for Visual mode by Vimium
call submode#map('chromectl', 'n', 'r', 'c', ':<C-u>ChromeStroke "vc"<CR>')
" Visual mode by Vimium
call submode#map('chromectl', 'n', 'r', 'v', ':<C-u>ChromeStroke "v"<CR>')
" send Esc by Vimium
call submode#map('chromectl', 'n', 'r', 'E', ':<C-u>ChromeStrokeCode 53<CR>')
" send Enter by Vimium
call submode#map('chromectl', 'n', 'r', 'e', ':<C-u>ChromeStrokeCode 76<CR>')
"}}}

```


上記のように設定すると、`<leader>j`でchromectl modeに入り、その後`j`を連打することで下にスクロールしたり、
`i` を押すことでページ上のリンクを開くためのVimiumのキーが表示されたりする。(表示された文字をChromeに送るとリンク先に移動する)

![](/images/vimiumlink.png)

単純にググるくらいの操作ならVimからできるようになるので、毎回VimとChromeを行き来する手間がなくなって嬉しい。

## 参考

* [Chromeでのブラウジングを快適にするTips](https://qiita.com/syui/items/d3c0721de71ebbe6461e)
