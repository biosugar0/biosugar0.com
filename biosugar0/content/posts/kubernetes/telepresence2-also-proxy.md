+++
title = "Telepresence2でAWS ElastiCacheエンドポイントのようなプライベートIPアドレスに接続する"
date = "2021-07-17"
tags = ["microservices","kubernetes","Telepresence2"]
slug = "telepresence2-also-proxy"
+++

以前、[Kubernetesクラスターと同期する、マイクロサービスのためのローカル開発環境](https://tech.smartshopping.co.jp/k8s_microservice)という題でブログを書きました。

それからしばらく経ち、[Telepresence2が登場](https://www.telepresence.io/announcing-telepresence-2/)し、無印のTelepresenceはサポート対象外になり、今後はTelepresence2を利用することが推奨されるようになっています。

ということでTelepresence2に移行しようとしているのですが、まだ解決すべき問題が残っていて移行しきれていません。
問題はAWS ElastiCacheエンドポイントのようなプライベートIPアドレスに接続するための設定です。
喜ばしいことに、接続自体は2021年6月14日にリリースされたvesion 2.3.1のアップデートで追加されたAlsoProxyできるようになりました。

## AlsoProxy設定でプライベートIPアドレスに接続する

<!--more-->

以前の無印のtelepresenceではコマンドのalso-proxyオプションがありましたが、2の設定は以下のようにkubernetesのconfigファイルで設定します。


`~/.kube/config`
```yaml
- cluster:
    certificate-authority-data: *********************************
    server: https://*******.ap-northeast-1.eks.amazonaws.com
    extensions:
    - name: telepresence.io
      extension:
        also-proxy:
        - 172.29.0.0/16
  name: arn:aws:eks:ap-northeast-1:******:cluster/my-cluster
```

AlsoProxyの設定は、extensionsの部分です。

```yaml
    extensions:
    - name: telepresence.io
      extension:
        also-proxy:
        - 172.29.0.0/16
```

このようにIPアドレスの範囲を指定することで、それに当てはまるアウトバウンドはTelepresenceによってプロキシされてクラスター内からの通信のように振る舞うので、クラスター内からの通信しかできないAWS ElastiCacheエンドポイントのような接続先とも通信できるようになります。

### 残された課題

これで機能としては実用的になったと思うのですが、この設定ファイルに設定するという方法だと

```bash
aws eks update-kubeconfig --name my-cluster
```
のようにconfigを更新するようなコマンドを叩くとextentions部分が消えてしまうので毎回更新のたびに追加しないといけないという面倒なことになっています。どうにかならないものか。。

開発環境だと夜は落としてたりするので認証情報を更新する必要があり、よくこれを叩いているのです。

またいい方法が見つかったらどこかで書きます。
