+++
title = "Ambassador API GatewayがCNCF Incubating Projectに参加しEmissary Ingressに改名。実行中システムには影響なし。"
date = "2021-06-03 01:20:22.908 +0900"
tags = ["kubernetes","microservices","Ambassador API Gateway","Emissary Ingress"]
slug = "ambassador-to-emissary"
+++


## Ambassador API Gateway
[Ambassador API Gateway](https://www.getambassador.io/products/api-gateway/)は、Kubernetes,Envoy上で動くマイクロサービス向けのAPI Gatewayだ。
パスルーティングや認証、タイムアウトやカナリアリリースなど数多くの機能を備えている。

いつも便利に使っているが、このたび[CNCF Incubating Projectに参加](https://blog.getambassador.io/emissary-ingress-ambassadors-api-gateway-is-officially-an-incubation-project-at-the-cncf-5030a3754c2)したことでEmissary-Ingressと名前も変わったようだ。


<!--more-->


今年の4月に発表されていたのに気づかなかった。
https://blog.getambassador.io/emissary-ingress-ambassadors-api-gateway-is-officially-an-incubation-project-at-the-cncf-5030a3754c2

CNCFのIncubating Projectは、ArgoやgRPCなども含まれるCNCDの習熟度レベルで言うと3段階のうちの真ん中のレベルのプロジェクトだ。

## 変更点
このタイミングでの改名での影響としては、自分の観測範囲では


1. GithubのURLが https://github.com/datawire/ambassador からhttps://github.com/emissary-ingress/emissary に変更
2. Datawire OSSslackの#ambassador チャンネルが #emissaryに変更


くらい。
ただ、**古いGithub URLはまだ引き続き機能するので既存の実行中のシステムには影響はない**だろう。
slackのチャンネル名も旧名で検索ができるので影響なし。

現時点での最新のリリースは
[Ambassador 1.13.6](https://github.com/emissary-ingress/emissary/releases/tag/v1.13.6)だが、
次のリリースからはEmissaryに変更されるのだろうか？
docker pullの参照先も変わるかな？今はまだ`docker.io/datawire/ambassador:1.13.6`のようだ。

いつまでも古い名前を使うのもあれなので、リリースしたら変更しようと思う。
今後も便利になっていくのを期待したい。
