+++
title = "tmuxのdisplay-popupとfzfでSessionとWindowの切り替えを便利にする"
date = "2022-08-19 23:54:00 +0900"
tags = ["Terminal","Tmux"]
slug = "tmux-popup-fzf-select"
+++

```
bind -n M-e choose-session
```

をよく使っていたが、

数が少ないときはいいけど、ある程度増えてくると上から順番に移動して選択するのが面倒。

あんまりプレビュー見ないし、session nameで曖昧検索して切り替えたいなと思って以下の設定をしたら便利だった。

<!--more-->
![session](/images/session.png)

```
bind M-e display-popup -E "tmux list-sessions -F '#S' | grep -v \"^$(tmux display-message -p '#S')\$\" | fzf --reverse | xargs tmux switch -t"
```


![session](/images/popup-session.png)


window切り替えもできる。
```
bind -n M-w display-popup -E "tmux list-windows -F '#W' | grep -v \"^$(tmux display-message -p '#W')\$\" | fzf --reverse | xargs | xargs tmux select-window -t"
```
