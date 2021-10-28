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
   ![](https://github.com/nmasashi/qiita/blob/main/kasairude/image/add_user01.png?raw=true)
1. 権限の設定
   - 今回は「AdministratorAccess」を使用してますが、ちゃんと必要最小限の権限設定にしましょう..
     ![](https://github.com/nmasashi/qiita/blob/main/kasairude/image/add_user02.png?raw=true)
1. タグなどは設定せず、ユーザー作成までクリック
1. ユーザーの完成
   - アクセスキーとシークレットアクセスキーは誰にも教えないようにしましょう（マジで）
     ![](https://github.com/nmasashi/qiita/blob/main/kasairude/image/add_user03.png?raw=true)

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
素直に `apt install nodejs` だと v10.x.x がインストールされる（2021/10/28 時点）ので、何かしたんだけど何したか忘れた。<br>

[参考ページ](https://www.stewright.me/2021/03/install-nodejs-14-on-ubuntu-20-04/)（これかな？知らんけど）

## Open Weather Map の API key 取得

## Twitter API の API key 取得

## System Manager に API key を設定

## プログラム書く

### テンプレート作成

AWS SAM CLI を使用して任意のフォルダにテンプレートを作成する

1. `sam init` を打つ
1. テンプレートは AWS Quick Start Templates を選択
1. パッケージタイプは Zip を選択
1. runtime は nodejs14.x を選択
1. Project name は kasairude
1. クイックスタートテンプレートはスケジュールイベントを選択

こんな感じ ↓

```shell
$ sam init
Which template source would you like to use?
        1 - AWS Quick Start Templates
        2 - Custom Template Location
Choice: 1
What package type would you like to use?
        1 - Zip (artifact is a zip uploaded to S3)
        2 - Image (artifact is an image uploaded to an ECR image repository)
Package type: 1

Which runtime would you like to use?
        1 - nodejs14.x
        2 - python3.9
        3 - ruby2.7
        4 - go1.x
        5 - java11
        6 - dotnetcore3.1
        7 - nodejs12.x
        8 - nodejs10.x
        9 - python3.8
        10 - python3.7
        11 - python3.6
        12 - python2.7
        13 - ruby2.5
        14 - java8.al2
        15 - java8
        16 - dotnetcore2.1
Runtime: 1

Project name [sam-app]: kasairude

Cloning from https://github.com/aws/aws-sam-cli-app-templates

AWS quick start application templates:
        1 - Hello World Example
        2 - Step Functions Sample App (Stock Trader)
        3 - Quick Start: From Scratch
        4 - Quick Start: Scheduled Events
        5 - Quick Start: S3
        6 - Quick Start: SNS
        7 - Quick Start: SQS
        8 - Quick Start: Web Backend
Template selection: 4

    -----------------------
    Generating application:
    -----------------------
    Name: kasairude
    Runtime: nodejs14.x
    Architectures: x86_64
    Dependency Manager: npm
    Application Template: quick-start-cloudwatch-events
    Output Directory: .

    Next steps can be found in the README file at ./kasairude/README.md
```

### 動くか確認

```shell
# 先ほどの手順で設定したProject nameのフォルダができているはず
cd kasairude

# パッケージインストール（脆弱性の警告が出るがスルー）
npm install

# テストを実行
npm run test
```

テストがパスしたら OK

## デプロイ

## テスト
