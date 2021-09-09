+++
title = "tmuxで現在のwindowをcurrent windowと共に左右に動かす"
date = "2021-07-09 18:10:22 +0900"
tags = ["tmux","terminal","shell","config"]
slug = "tmux-swap-window"
+++


tmuxで現在のwindowを左右に入れ替えるときに、ググるとよく出てくるのが以下のような設定だ。
しかしこれだと、フォーカスが当たっているcurrent windowは移動前の位置にいるままになってしまう。
<!--more-->

```
bind-key -n M-Left swap-window -t -1
bind-key -n M-Right swap-window -t +1
```


current windowも一緒に移動したい場合は以下のように設定することで実現できる。


```
# 現在のwindowを左右に動かす
bind -n M-Left run "tmux swap-window -t -1 && tmux previous-window"
bind -n M-Right run "tmux swap-window -t +1 && tmux next-window "
```


`run` を使うことでシェル経由の実行となり、おなじみの `&&` で入れ替え後にcurrent windowも移動することができる。

