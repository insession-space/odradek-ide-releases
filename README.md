# Session Desk — 配布物置き場

[Session Desk](https://github.com/insession-space/session-desk)（走っているエージェントと、落ちかけているタスクを1画面で見る macOS アプリ）の
ビルド成果物を置くためのリポジトリ。**ソースコードは無い。** 中身は [Releases](../../releases) だけ。

ソースを private のままにしつつ、アプリの自動更新チェックがマニフェストを取りに来られるようにするために分けている
（private リポジトリの Releases は認証なしで読めず、updater から参照できない）。

## ダウンロード

[最新リリース](../../releases/latest) から `SessionDesk_*_aarch64.dmg` を落とす。

**Apple Silicon (M1 以降) 専用。** Intel Mac 向けのビルドは出していない。

## 初回起動時に「検証できませんでした」と言われたら

**このアプリは Apple の公証（notarization）を受けていない。** そのため初回だけ macOS に止められる。

> Apple は、"Session Desk.app" に Mac に損害を与えたり、プライバシーを侵害する
> 可能性のあるマルウェアが含まれていないことを検証できませんでした。

アプリが壊れているわけではなく、Apple に審査を出していないという意味。想定どおりの表示で、次の手順で開ける。

### 1. まず Applications にコピーする

**`.dmg` の中から直接起動しない。** dmg は読み取り専用なので、その上では検疫属性を外せず、
何度やっても弾かれ続ける。`Session Desk.app` を `Applications` にドラッグしてから開くこと。

### 2. 検疫属性を外す

**ターミナルから（確実）**

```sh
xattr -dr com.apple.quarantine "/Applications/Session Desk.app"
```

**システム設定から**

1. 一度ダブルクリックして、弾かれる
2. **システム設定 → プライバシーとセキュリティ** を開く
3. 下の方に出る「"Session Desk" は開発元を確認できないため…」の横の **「このまま開く」** を押す

一度これをやれば、以降は普通にダブルクリックで起動できる。

> **macOS 15 (Sequoia) 以降、「右クリック →『開く』」による回避は Apple が廃止した。**
> 古い手順を案内している記事が多いが、いまの macOS では効かない。上の2つを使うこと。

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
