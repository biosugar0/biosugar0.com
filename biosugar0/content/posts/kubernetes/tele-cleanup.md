+++
title = "TelepresenceのWrapperツールにコマンド終了後のCleanUp処理を入れた"
date = "2021-01-11"
tags = ["kubernetes","Telepresence","go","development"]
slug = "tele-cleanup"
+++

昨年、[マイクロサービス開発を効率化するTelepresenceのWrapperツール](https://www.biosugar0.com/posts/2020/07/tele/)の[tele](https://github.com/biosugar0/tele)を作ったが、このツールにCleanUp処理を入れた。
Ctrl + Cでコマンドを止めたときも自動でTelepresence が作成したPodとServiceを削除してくれるので便利。
もちろん正常終了したときも掃除してからコマンドが終了する。

