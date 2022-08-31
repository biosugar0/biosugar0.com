+++
title = "Control連打でターミナルとChromeをスムーズに行き来する設定(Mac)"
date = "2022-08-19 22:54:00 +0900"
tags = ["Terminal"]
slug = "toggle-terminal-and-browser"
+++

けっこう前から、キーボードだけでChromeとターミナルを行き来したいなと思っていたのですが、
ようやく[Karabiner-Elements](https://karabiner-elements.pqrs.org/)を入れて実現しました。

以前[VimでChromeを操作するプラグイン](https://www.biosugar0.com/posts/2020/08/chrome-vim/)を作っておいてあれですが、
やっぱりChromeの操作は[Vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb)が快適です。

ですが、やはり黒い画面からできるだけ離れたくないのですぐにターミナルに戻ってこれる設定をしました。
Karabiner-Elementsで以下の設定をすれば、Controlを2連打することで以下のことができます。

* ターミナルにフォーカスが当たっている時はChromeにフォーカスを移す。
* **ターミナル以外** にフォーカスが当たっている時はターミナルにフォーカスを移す。

キーボードだけでいつでもターミナルとChromeを行き来できるようになる訳です。

快適なのでおすすめ。

ターミナルに戻るのはSlackにいてもいつでも戻りたいのでそういう設定にしてます。


## 設定

### 1. Karabiner-Elements
まずは、Karabiner-Elements が必要なのでインストールしてください。

```sh
brew install --cask karabiner-elements
```

### 2. キーマッピングの設定ファイルを書く

`~/.config/karabiner/assets/complex_modifications/` に以下のようなjsonファイルを置きます。

<!--more-->


`termBase.json`

```json
{
  "title": "TermBase",
  "rules": [
    {
      "description": "Toggle Terminal and Chrome",
      "manipulators": [
        {
          "type": "basic",
          "conditions": [
            {
              "type": "frontmost_application_unless",
              "bundle_identifiers": [
                "^com\\.apple\\.Terminal$",
                "^com\\.googlecode\\.iterm2$",
                "^io\\.alacritty$"
              ]
            },
            {
              "type": "variable_if",
              "name": "left_control_key",
              "value": 1
            }
          ],
          "from": {
            "key_code": "left_control"
          },
          "to": [
            {
              "shell_command": "osascript -e 'tell application \"iTerm\" to activate' &"
            }
          ]
        },
        {
          "type": "basic",
          "conditions": [
            {
              "type": "frontmost_application_if",
              "bundle_identifiers": [
                "^com\\.apple\\.Terminal$",
                "^com\\.googlecode\\.iterm2$",
                "^io\\.alacritty$"
              ]
            },
            {
              "type": "variable_if",
              "name": "left_control_key",
              "value": 1
            }
          ],
          "from": {
            "key_code": "left_control"
          },
          "to": [
            {
              "shell_command": "osascript -e 'tell application \"Chrome\" to activate' &"
            }
          ]
        },
        {
          "type": "basic",
          "conditions": [
            {
              "type": "variable_if",
              "name": "left_control_key",
              "value": 0
            }
          ],
          "from": {
            "key_code": "left_control",
            "modifiers": {
              "optional": [
                "any"
              ]
            }
          },
          "to": [
            {
              "set_variable": {
                "name": "left_control_key",
                "value": 1
              }
            },
            {
              "key_code": "left_control"
            }
          ],
          "to_delayed_action": {
            "to_if_invoked": [
              {
                "set_variable": {
                  "name": "left_control_key",
                  "value": 0
                }
              }
            ],
            "to_if_canceled": [
              {
                "set_variable": {
                  "name": "left_control_key",
                  "value": 0
                }
              }
            ]
          }
        }
      ]
    }
  ]
}
```

これでKarabiner-Elementsの `Complex Modifications` → `Add rule` を選択すると `Toggle Terminal and Chrome` が出てくるようになるので、`Enable` すればこのマッピングが有効になります。


何をしているかというと、以下の設定をしています。

1. Controlを押すとKarabiner内部の `left_control_key` 変数が1になる
2. `left_control_key` 変数が1かつもう一度Controlを押した時、
    * ターミナルにフォーカスが当たっている時はAppleScriptでChromeをActiveにする
    * ターミナルにフォーカスが当たっていない時はAppleScriptでターミナルをActiveにする
3. Controlキーを押してから500ms経過した時と、最初のキーと違うキーを押した場合に `left_control_key` 変数の値を0に初期化する


これでControl連打でターミナルとChromeをスムーズに行き来できるようになりました。
これはだいぶ便利に感じているので使っていきます。
