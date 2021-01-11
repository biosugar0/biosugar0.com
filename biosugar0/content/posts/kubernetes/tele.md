+++
title = "マイクロサービス開発を効率化するTelepresenceのWrapperツールを作った。"
date = "2020-07-12"
tags = ["kubernetes","Telepresence","go","development"]
slug = "tele"
+++

以前、[マイクロサービスのためのローカル開発環境の記事](https://www.biosugar0.com/posts/2020/01/telepresence/)を書いたが、より開発環境を便利にするためにTelepresenceのWrapperツール[tele](https://github.com/biosugar0/tele)を作った。
以下のようなコマンドでローカル環境で実行したコマンドをリモートのk8sクラスターと接続することができる。

<!--more-->

```
tele --port 8080:8080 go run main.go
```

上記のコマンドが実行するtelepresenceのコマンドは以下のようなものなので、だいぶ単純化される。
```
telepresence --namespace default --method inject-tcp --new-deployment yuto-tele-master --expose 8080:8080 --run bash -c "go run 'main.go'"
```


主な特徴は以下のようなものである。



* 複数人での開発を前提とするため、Telepresenceのswap deployment機能は使わず、new deploymentを使用する。そのため、deploymentのオプションを指定する必要がない。
* deploymentに使用する名前は自動で`ホームディレクトリ名`-`gitリポジトリ名`-`ブランチ名`で指定され、指定しなくていい。
* deploymentに使用する名前には文字数制限や使用できない文字があるが、自動で使用可能な名前に変換される。
* コマンドに`'(\.go$|go\.mod)'`のような特殊文字が含まれる文字列が含まれる場合でもそのまま使用可能。

最後の特殊文字についもう少し補足すると、

```
reflex -d fancy -r '(\.go$|go\.mod)' -s -- bash -c 'GODEBUG=netdns=cgo go run -v main.go -env development'
```

のようなコマンドを実行したい場合、telepresenceではダブルクォーテーションで囲んだり、
`bash -c`に渡したりする必要があるが、teleコマンドではそのまま渡して実行することができる。
