---
title: "docker compose up -dが「Synchronized File Shares」で止まる"
emoji: "🐷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Docker","docker-compose"]
published: true
---

# 概要
いつものようにDev Containerでコンテナを立ち上げようとしたところ、いつまで経っても終わらない。試しにdocker compose up -dを実行しても同様。

```
08:33:49 ~/projects/api $ docker compose up -d
[+] Running 0/2
[+] Running 6/2 File Shares [⠀]    // ここで止まる
```

# 対処方法
原因は不明。Docker DesktopのFile Sharingを削除後にdocker compose up -dしたら解消。

# 環境
- Docker Desktop 4.36.0 (175267)
- Docker version 27.3.1

# 試したこと
- PC再起動 ❌
- Docker Desktop再起動 ❌
- docker compose build --no-cache ❌

# 調査
## verboseオプション
compose.ymlの先頭にversionつけるのは非推奨とかは出るが、File Shareのところで5分以上止まり続け原因は出力されない。
```
08:41:16 ~/projects/api $ docker compose --verbose up -d
WARN[0000] /Users/projects/api/compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
DEBU[0000] Found existing suitable file share fshr_e24158524e2dae7b0e26285731fec059ce2e244c000 for path "/Users/projects/api" [/Users/projects/api] 
[+] Running 0/2
[+] Running 4/2 File Shares [⠀]         
```

## Docker DesktopのFile Sharing
Settings->Resources->File Sharingを確認。該当のFile Sharing(赤枠)のstatusがDisconnectedとなっている。ちなみに黒塗りつぶしされているpathも間違いないことを確認。
![](https://storage.googleapis.com/zenn-user-upload/cdcd72b1f457-20241209.png)

## ググった結果
「ローカルの変更がコンテナに反映されない」というイシューだが、削除したら解消されたとのこと。File Sharingの問題なのは同じなので試してみた。
https://arc.net/l/quote/szmvttir

Deleteボタンから削除し、docker compose up -dを実行したところ解消。statusもWatching for filesystem changesになった。
![](https://storage.googleapis.com/zenn-user-upload/dab20af64827-20241209.png)
