---
layout: post
title: "Go事始め"
date: 2014-02-02 22:16
comments: true
categories: Go
---

# GVM（バージョン管理）インストール
`bash < <(curl -s https://raw.github.com/moovweb/gvm/master/binscripts/gvm-installer)`

# .zshrc（.bashrc）に以下追記
```sh .zshrc
export PATH=$PATH:$HOME/.gvm/bin
[[ -s "$HOME/.gvm/scripts/gvm" ]] && source "$HOME/.gvm/scripts/gvm"
```

# インストール可能なバージョンを確認
`gvm listall`

# Goインストール
`go install go1.2`

# インストールされているバージョンを確認
`gvm list`

# 使いたいGoのバージョンを指定
`gvm use go1.2`
> 常にこのバージョンを使いたい場合はこれを.zshrc（.bashrc）に追記しておく

# GoSublime（Sublime Text用プラグイン）インストール

1. Sublime Text 3を開き、Cmd+pで`Package Control: Install Package`と入力しエンター
1. さらに`gosublime`と入力しエンター
1. `Preferences -> Package Settings -> GoSublimge -> Settings – User`に以下入力

```json GoSublime.sublime-settings
{
    "env": { 
        "GOPATH": "$HOME/.gvm/pkgsets/go1.2/global",
        "PATH": "$HOME/.gvm/gos/go1.2/bin"
    },
    "gslint_cmd": ["go", "tool", "gotype", "-p", "$pkg", "$files"],
    "autocomplete_tests": true,
    "autocomplete_builtins": true,
    "fmt_tab_width": 4
}
```
> GVMが$GOPATHを設定してくれているのでecho `$GOPATH`で確認できる

# サンプルコード作成

```go main.go
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Hello, 世界")
}
```

# Sublime Text上で実行

1. Cmd+9でコマンドシェルを開く
1. cdコマンドでカレントに移動
1. `run main.go`で実行
