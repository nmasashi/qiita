# 仕様

1. 毎朝午前 6 時にお天気情報を取得
1. 雨が降る場合はツイート

# 構成図

![](https://raw.githubusercontent.com/nmasashi/qiita/main/kasairude/image/kasairu.png?token=AT5IP6FSITYG5II4WWXSGMTBPFRAS)

### それぞれの役割

- [Lambda](https://aws.amazon.com/jp/lambda/)
  - 主役
  - イベント発生時にプログラムを起動
  - プログラム実行用のサーバーを用意しなくていいので楽
  - 今回はお天気情報を取得して、必要応じてツイートするプログラムが稼働
- [EventBridge](https://aws.amazon.com/jp/eventbridge/)
  - トリガーを設定してイベントを起動
  - 今回は時間をトリガーにして Lambda を起動
- [System Manager](https://aws.amazon.com/jp/systems-manager/)
  - パラメータストアの機能を仕様
  - 今回は Twitter API の API key などの外部に公開できない情報を保存
- [Open Weather Map](https://api.rakuten.net/community/api/open-weather-map)
  - お天気情報 API
  - 無料版では、1 カ月 500 回, 1 分間 10 回のリクエスト制限
  - 今回は 3 時間毎の天気を取得できる API を仕様
- Twitter
  - ツイッター

### 処理フロー

1. EventBridge が午前 6 時に Lambda を起動
1. System Manager から Open Weather Map と Twitter API の API key を取得
1. Opwn Weather Map からお天気情報を取得
1. 当日の天気が雨ならばツイート、雨以外ならツイートしない

# 作り方

多分、作る人以内と思うので備忘録のテンションで書いていきます（笑）

## 環境

Windows10 (WSL2 Ubuntu)

## AWS アカウント作成

[ここ](https://aws.amazon.com/jp/)からアカウント作成<br>
アカウント作成後、しばらくはサービスが使用できない（支払情報の確認？？）

## IAM の作成

1. [IAM ダッシュボード](https://console.aws.amazon.com/iamv2/home#/home)のアクセス管理 -> ユーザーを選択
1. ユーザーを追加をクリック
1. ユーザー名等の設定
1. 権限の設定
   - 今回は「AdministratorAccess」を使用してますが、ちゃんと必要最小限の権限設定にしましょう..
1. タグなどは設定せず、ユーザー作成までクリック
1. ユーザーの完成
   - アクセスキーとシークレットアクセスキーは誰にも教えないようにしましょう（マジで）

## 各種インストール

### AWS SAM CLI

[参考ページ](https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/serverless-sam-cli-install-linux.html)

```sh
# ファイルダウンロード
curl -OL https://github.com/aws/aws-sam-cli/releases/latest/download/aws-sam-cli-linux-x86_64.zip

# 展開
unzip aws-sam-cli-linux-x86_64.zip -d sam-installation

# インストール
sudo ./sam-installation/install

# 確認
sam --version
```

### nodejs 14

どうやってインストールしたか忘れた...<br>
素直に`apt install nodejs`だと v10.x.x がインストールされる（2021/10/28 時点）ので、何かしたんだけど何したか忘れた。<br>

[参考ページ](https://www.stewright.me/2021/03/install-nodejs-14-on-ubuntu-20-04/)（これかな？知らんけど）

## Open Weather Map の API key 取得

## Twitter API の API key 取得

## System Manager に API key を設定

## デプロイ

## テスト
