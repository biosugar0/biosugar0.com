+++
title = "Telepresenceで作成するDeploymentに使える文字数の最大"
description = "Telepresenceで作成するDeploymentに使える文字数の最大"
date = "2020-07-01"
tags = ["kubernetes","Telepresence"]
slug = "telepresence-max-len"
+++


Telepresenceで長めの名前のDeploymentを作ろうとしたときに失敗することがあるので、最大文字数がどのくらいか試してみた。

<!--more-->
結論から書くと、**最大は57文字**のようだ。

ランダム文字列で作成すると、以下のようなpodができる。

```
isepupaiquixuab6saephai7nahxaixeeduebu8aith8iengohdei9aig-vzkwd
```

k8sではよく[RFC 1123](https://tools.ietf.org/html/rfc1123)で定義される以下のようなDNSラベル標準に従う命名ルールが適応されているらしい。

* 63文字以内
* 英小文字、数字または「-」のみを含む
* 英数字で始まる
* 英数字で終わる

ハイフン以降に5文字のpodを区別するためのランダム文字列が自動でつくが、これの最小が5文字だとすると、合計63文字になるのでそのへんが理由かなと思ったがどうなんだろう。

63文字はここでチェックしてるようだ。
https://github.com/kubernetes/kubernetes/blob/7f2d0c0f710617ef1f5eec4745b23e0d3f360037/pkg/util/validation.go#L40-L46
