+++
title = "ターミナルからMacをスリープさせる設定をした"
description = "ターミナルからMacをスリープさせる設定をした"
date = "2020-12-12"
tags = ["Terminal"]
slug = "mac-sleep"
+++

普段MacBook Proを使っているが、スリープをするのにGUIでやるのが面倒になったのでターミナルからスリープするaliasを設定した。
<!--more-->


`~/.zshrc`に

```sh
alias ms='pmset sleepnow'
```

これでターミナルで `ms`してスリープできるようになった。便利。
