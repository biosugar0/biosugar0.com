+++
title = "SalesforceのカスタムオブジェクトでIntを使いたかった"
date = "2020-11-12"
tags = ["Salesforce"]
slug = "salesforce-int"
+++

Salesforceのカスタムオブジェクトには数値型があるが、整数と小数が別れていない。
数値型の項目に整数で `12` のように登録してみたところ、API経由で取得する際には `12.0` のように小数で返ってきた。
問い合わせたところ、`.0` を消して返してもらうにはテキスト型などを使用する(もちろん `""` がつく)しかないようだ。
Go言語からAPIを使って整数として値を受け取りたかったが残念。
数値をテキストとして扱うのも嫌なので今回はfloatとして受け取ってIntに変換するようにした。
