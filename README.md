# Session Desk — 配布物置き場

[Session Desk](https://github.com/insession-space/session-desk)（走っているエージェントと、落ちかけているタスクを1画面で見る macOS アプリ）の
ビルド成果物を置くためのリポジトリ。**ソースコードは無い。** 中身は [Releases](../../releases) だけ。

ソースを private のままにしつつ、アプリの自動更新チェックがマニフェストを取りに来られるようにするために分けている
（private リポジトリの Releases は認証なしで読めず、updater から参照できない）。

## ダウンロード

[最新リリース](../../releases/latest) から `SessionDesk_*_aarch64.dmg` を落とす。

**Apple Silicon (M1 以降) 専用。** Intel Mac 向けのビルドは出していない。

## 初回起動時に「開けません」と言われたら

**このアプリは Apple の署名・公証を受けていない。** そのため初回だけ macOS の Gatekeeper に止められる。
これは想定どおりで、次のどちらかで開ける。

**Finder から（推奨）**

1. `/Applications/Session Desk.app` を **右クリック**（または Control + クリック）
2. 「開く」を選ぶ
3. 出てきたダイアログで、もう一度「開く」

一度これをやれば、以降は普通にダブルクリックで起動できる。

**ターミナルから**

```sh
xattr -dr com.apple.quarantine "/Applications/Session Desk.app"
```

## 更新

アプリが起動時に更新の有無を確認し、新しいバージョンがあれば画面下部に控えめに知らせる。
メニューバーの「Session Desk」→「更新を確認…」からいつでも手動で確認できる。

更新の**適用は手動**。知らせが出たら「リリースを見る」からこのページに来て、新しい `.dmg` に入れ替える。

## Releases に置いてあるもの

| ファイル | 用途 |
| --- | --- |
| `SessionDesk_<version>_aarch64.dmg` | 人がダウンロードしてインストールする用 |
| `SessionDesk_aarch64.app.tar.gz` | アプリの更新チェックが使う本体 |
| `SessionDesk_aarch64.app.tar.gz.sig` | 上記の minisign 署名 |
| `latest.json` | 更新チェックが最初に読むマニフェスト |

ファイル名から空白を抜いてあるのは、**GitHub が Release アセット名の空白をピリオドに置き換える**ため。
そのままだと `latest.json` に書いた URL と実際の URL が食い違って、更新チェックが 404 になる。

`latest.json` は常に `releases/latest/download/latest.json` で引ける。アプリはこの URL を見ている。

## Issue / 不具合

このリポジトリではなく、[insession-space/session-desk](https://github.com/insession-space/session-desk) 側で扱う
（ただし private なので、アクセス権のある人のみ）。
