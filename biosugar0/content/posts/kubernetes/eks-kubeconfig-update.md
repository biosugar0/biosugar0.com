+++
title = "EKSでkubeconfigの認証情報のみを更新する"
date = "2021-07-18 00:10:00 +0900"
tags = ["microservices","kubernetes","Telepresence2","EKS"]
slug = "rewright-auth-kubeconfig"
+++

前回の[Telepresence2でAWS ElastiCacheエンドポイントのようなプライベートIPアドレスに接続するで挙げた課題](https://www.biosugar0.com/posts/2021/07/telepresence2-also-proxy/#%E6%AE%8B%E3%81%95%E3%82%8C%E3%81%9F%E8%AA%B2%E9%A1%8C)をなんとかする方法を見つけたのでメモ。

課題は認証情報を更新するときに

```bash
aws eks update-kubeconfig --name my-cluster
```

を叩いてconfigに手で書き加えた設定が消えてしまうことでした。
これをどう解決するかというと、タイトル通り認証情報のみを書き換えます。

<!--more-->

書き換えは以下のようにzsh functionでやります。

```zsh
function k-update(){
  clusterName="my-cluster"
  awsID="*****"
  region="ap-northeast-1"

  clusterARN="arn:aws:eks:${region}:${awsID}:cluster/${clusterName}"
  jsonPathString="{.clusters[?(@.name == \"$clusterARN\")].cluster.certificate-authority-data}"

  kubeConfig=$(aws eks update-kubeconfig --name ${clusterName} --profile prd-admin --dry-run | base64)
  crt=$(kubectl --kubeconfig <(echo "${kubeConfig}" | base64 -d ) config view --raw -o jsonpath="${jsonPathString}")

  current=$(kubectl config get-clusters | grep ${clusterARN})
  if [ "${current}" = "" ]; then
      KUBECONFIG=~/.kube/config:<(echo "${kubeConfig}" | base64 -d ) kubectl config view --raw >| ~/.kube/config
      echo "new cluster ${clusterName} added to kubeconfig."
      return
  fi

  kubectl  config set "clusters.${clusterARN}.certificate-authority-data" "${crt}"
}
```

これは何をやっているかというと、以下のようなことをしています。

1. aws eks update-kubeconfig を `--dry-run` をつけて実行することで標準出力のみに出力
2. シェルのプロセス置換を利用してkubectl が`dry-run`結果(認証情報更新済み)をconfigとして使用するようにし、認証情報 `certificate-authority-data` のみを取り出す。
3. 新しいクラスターの場合はそのまま`dry-run`結果を`~/.kube/config`に保存
4. 既に設定が存在するクラスターの場合は認証情報 `certificate-authority-data` のみを`dry-run`結果(認証情報更新済み)のものに更新

これで `k-update` を叩くことで認証情報が更新され、手で書き加えた設定も消えないということになります。
