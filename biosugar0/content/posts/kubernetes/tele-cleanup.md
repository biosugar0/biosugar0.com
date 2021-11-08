+++
title = "TelepresenceのWrapperツールにコマンド終了後のCleanUp処理を入れた"
date = "2021-01-11"
tags = ["kubernetes","Telepresence","go","development"]
slug = "tele-cleanup"
+++

昨年、[マイクロサービス開発を効率化するTelepresenceのWrapperツール](https://www.biosugar0.com/posts/2020/07/tele/)の[tele](https://github.com/biosugar0/tele)を作ったが、このツールにCleanUp処理を入れた。
Ctrl + Cでコマンドを止めたときも自動でTelepresence が作成したPodとServiceを削除してくれるので便利。
もちろん正常終了したときも掃除してからコマンドが終了する。
<!--more-->

この実装をするときに、`kubectl get` のときに出てくるPodのステータスは内部的には
`pod.Status.Phase`のようにPodのオブジェクトに格納され定数として定義もされているが、
`Terminating` は同じようには定義されておらず、`pod.ObjectMeta.DeletionTimestamp` みたいな `DeletionTimestamp`の有無で判定して表示していることを知った。
Podの削除完了を待つ処理を作るのにこれに気づかずちょっと手間取った。
