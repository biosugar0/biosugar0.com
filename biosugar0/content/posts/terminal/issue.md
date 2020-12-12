+++
title = "ターミナルからGitHubのリポジトリのissueを開く設定をした"
description = "ターミナルからGitHubのリポジトリのissueを開く設定をした"
date = "2020-12-12"
tags = ["Terminal"]
slug = "issue"
+++

普段業務ではひとつのリポジトリにissueが集約されているが、毎回そのリポジトリをブラウザから見に行くのが面倒になったのでターミナルから検索&Openできるようにした。
<!--more-->


`~/.zshrc`に

```zsh
function issue(){
  open "https://github.com/${repo-owner}/${repo-name}/issues?q=is:issue+is:open+$1"
}
```

これでターミナルで

```
issue hoge
```

と叩くとhogeに関するissueが検索された画面がブラウザで開かれる。便利。
