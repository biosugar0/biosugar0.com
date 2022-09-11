+++
title = "Copilot.vimを使おうと思ったらNot authenticated: NotSignedInエラーで悩んだ話"
date = "2022-09-11 19:03:00 +0900"
tags = ["Vim"]
slug = "vim-copilot-notsignedin-err"
+++

Copilot使おうと思ったら認証エラー `Not authenticated: NotSignedIn` が出て悩んだので対処方法をメモ。
原因はすっかり忘れてたけどCopilotを有効化するずっと前にCopilot.vimで `:Copilot setup` してみたことがあって、ずっとその認証情報が使われてたようだ。

<!--more-->

Copilotの応答は`b:_copilot`に入るので、その中身を入力中に見れるようにインサートモードの適当なキーにマッピングする。

```
inoremap <C-l> <Cmd>echom b:_copilot<CR>
```

これで応答を見てみると、

```
{'first': {'error': {'code': 1000, 'message': 'Not authenticated: NotSignedIn'}...
```

のようなエラーが出ていた。

そこで、もしかしてと思って `:Copilot signout` してからもう一度 `:Copilot setup` して1 time tokenを入力する手順を
行ったら動いた。
