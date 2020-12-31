+++
title = "ECSでS3マウントしたSFTPサーバをたてるメモ"
date = "2020-12-31"
tags = ["ECS","Docker","SFTP"]
slug = "ecs-sftp"
+++

開発用にs3をマウントしたSFTPをECSでたてたのでメモ。

* SFTPはhttps://github.com/atmoz/sftp を利用
* S3のマウントは https://github.com/kahing/goofys を利用


## 1. Dockerfile
SFTPサーバをたてるのはatmoz/sftpを利用するので、
そのイメージをベースにDockerfileを書く。
仕組みとしては、atmoz/sftpでSFTPサーバを起動する前にS3をマウントするというものだ。
Dockerfileは以下のようになり、setup.shの中でS3をマウントした。

```dockerfile
FROM atmoz/sftp:alpine

ENV GOOFYS_VERSION 0.24.0
ENV STAT_CACHE_TTL 1m0s
ENV TYPE_CACHE_TTL 1m0s
RUN apk update && apk add gcc ca-certificates openssl musl-dev git fuse syslog-ng coreutils curl


RUN curl --fail -sSL -o /usr/local/bin/goofys https://github.com/kahing/goofys/releases/download/v${GOOFYS_VERSION}/goofys \
    && chmod +x /usr/local/bin/goofys
RUN curl -sSL -o /usr/local/bin/catfs https://github.com/kahing/catfs/releases/download/v0.8.0/catfs && chmod +x /usr/local/bin/catfs

COPY ./setup.sh  /setup.sh
RUN chmod +x /setup.sh

EXPOSE 22
ENTRYPOINT ["/setup.sh"]
```

2. S3のマウント

setup.shのなかでは、以下のようにgoofysコマンドを叩いてS3バケットをマウントしている。

```bash
goofys -o allow_other --stat-cache-ttl $STAT_CACHE_TTL --type-cache-ttl $TYPE_CACHE_TTL --file-mode=0766 --dir-mode=0766 --uid 1000 --gid 101 $BUCKET $MOUNT_PATH
```


S3マウント時にはSFTPユーザーのuidとgidを指定、さらに`--file-mode=0766 --dir-mode=0766`のようにディレクトリの権限をs3マウント時に読み書きできるように設定する必要がある。
Docker起動時のコマンドを実行するのはrootなので、マウントしたs3ディレクトリはそのままだとrootしか使用できない。
 `-o allow_other`をつけてマウントしたユーザー(=root) 以外のアクセスを許可する。
 `$STAT_CACHE_TTL`,`$TYPE_CACHE_TTL`はキャッシュの設定で、両方`1m0s`を設定した。

 マウント後に、`atmoz/sftp`のentrypointスクリプトを起動する。引数には、以下のように

* ユーザー名
* パスワード
* uid
* gid
を指定するとそのSFTPユーザーが作られてSFTPサーバが起動する。

```bash
/entrypoint "foouser:pass:1000:101:"
```

3. Dockerの起動オプション

前述のように設定したコンテナイメージを動作させるためには、例えばDockerでの実行時には以下のように
いくつかのオプションを渡す必要がある。
強めの権限をDockerにわたすことになるので、本番運用には向かないだろう。

```bash
docker run --cap-add MKNOD \
    --cap-add SYS_ADMIN \
    -p 2222:22 \
    --device /dev/fuse s3-sftp
```

3. ECSの設定

### ECSクラスタ作成

新しいVPC,subnetの作成とともにコンソールからぽちぽち作った。

### ECSのタスク定義時に、JSONで以下のような設定をすることでDockerの起動オプションを渡すことができる。

```json
            "linuxParameters": {
                "capabilities": {
                    "add": [
                        "MKNOD",
                        "SYS_ADMIN"
                    ]
                },
                "devices": [
                    {
                        "containerPath": "/dev/fuse",
                        "hostPath": "/dev/fuse",
                        "permissions": [
                            "read",
                            "write",
                            "mknod"
                        ]
                    }
                ]
            },
```

### ポートマッピングをホストとコンテナ側で違う設定をする

ネットワークモードをbridgeにして、containerDefinitionsで以下のように設定する。

```json
      "portMappings": [
        {
          "containerPort": 22,
          "hostPort": 2222,
          "protocol": "tcp"
        }
      ],
```

taskRoleにはs3の読み書き削除の権限をつけたRoleを作成して使用した。
この権限が足りないとマウントしたディレクトリでの操作が弾かれる。

### NLBの設定

ECSクラスタ作成時に作ったVPC,subnetに紐づくNLBを作っておき、ECSのサービス作成時に紐付ける。
ネットワークモードbridgeの場合NLBのタイプはinstanceで。
NLBのリスナーには前述した例だとホスト側のポート2222を設定した。

### サービスの作成

作成したNLBを指定して紐付けてサービスを作成する。 listenerは2222で。
作成後にサービスが起動するが、タスクが実行されるインスタンスに紐づくsecurity groupのInboundでポート範囲2222番を許可しておく。
そうしないとNLBのヘルスチェックと実際のSFTPアクセスができない。
IPアドレス制限をしたい場合はNLBを置くsubnetのCIDRをソースに設定したものが最低限必要で、追加で許可したいIPアドレスを設定する。
