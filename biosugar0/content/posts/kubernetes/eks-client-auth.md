+++
title = "kubectlv1.24によるEKS操作のためにawscliをv2.6.3に更新した"
date = "2022-05-10"
tags = ["kubernetes","AWS","EKS"]
slug = "eks-client-auth"
+++

## 問題

[kubernetes1.24のchange log](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md)にあるように、EKSの認証情報を更新するために使っていた
```zsh
aws eks update-kubeconfig
```
が従来想定していた `client.authentication.k8s.io/v1alpha1` APIが1.24で削除されました。

> - The `client.authentication.k8s.io/v1alpha1` ExecCredential has been removed. If you are using a client-go credential plugin that relies on the v1alpha1 API please contact the distributor of your plugin for instructions on how to migrate to the v1 API. ([#108616](https://github.com/kubernetes/kubernetes/pull/108616), [@margocrawf](https://github.com/margocrawf))


これによって、同じく`kubectl` をv1.24にアップデートすると
```zsh
aws eks update-kubeconfig
```

を実行するとkubectlでの操作の際に

```
invalid apiVersion "client.authentication.k8s.io/v1alpha1"
```

のエラーが出て認証が通らなくなっていました。


## 解決策

これを解決するには、2022年5月7日にリリースされたawscliの `2.6.3` 以上に更新する必要があります。

[この問題に対処するPR](https://github.com/aws/aws-cli/pull/6476)が含まれているためです。

この変更によって、

```zsh
aws eks update-kubeconfig
```

コマンドは `client.authentication.k8s.io/v1beta1` をkubeconfigに書き込むようになるため、
invalid apiVersionエラーは発生せずに正常に認証が通るようになります。


## おまけ

いつのまにか

```
brew install awscli
```

でawscliのv2がインストールでるようになっていたのでbrew管理に移行しました。
以前はこのコマンドではv1がインストールされてしまっていたので使っていなかったんですよね。
