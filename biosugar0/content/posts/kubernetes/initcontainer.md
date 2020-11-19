+++
title = "CronJobで順番のあるプログラムを実行する設定"
description = "CronJobで順番のあるプログラムを実行する設定"
date = "2020-11-19"
tags = ["kubernetes"]
slug = "cronjob-init-container"
+++


k8sのCronJobで順番のあるプログラムを定期実行したい場合、以下のようにInit Containersを使用すると実現できる。

<!--more-->

```yaml
apiVersion: batch/v1beta1
kind: CronJob
spec:
  schedule: "10 4 * * *"
  concurrencyPolicy: Forbid
  startingDeadlineSeconds: 30
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 1
  jobTemplate:
    spec:
        spec:
          initContainers:
          - name: job1
            image: busybox
            securityContext:
                runAsUser: 1000
            command: ['sh','-c','echo "step1"']
            resources:
                requests:
                  cpu: 200m
                  memory: 250Mi
                limits:
                  cpu: 300m
                  memory: 500Mi
          - name: job2
            image: busybox
            securityContext:
                runAsUser: 1000
            command: ['sh','-c','echo "step2"']
            resources:
                requests:
                  cpu: 200m
                  memory: 250Mi
                limits:
                  cpu: 300m
                  memory: 500Mi
          containers:
            - name: jobcomplete
              image: busybox
              command: ['sh','-c','echo "finish!"']
          restartPolicy: Never
```


このように設定すると、毎回

1. job1
2. job2
3. jobcomplete

の順で実行される。
また、initContainersの中で各コンテナのリソースを設定しておくと、
全てのInitコンテナの中で定義された最も高い設定が適応される。
