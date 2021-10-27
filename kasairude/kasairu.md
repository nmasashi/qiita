# 仕様

1. 毎朝午前 6 時にお天気情報を取得
1. 雨が降る場合はツイート

# 構成図

![](https://raw.githubusercontent.com/nmasashi/qiita/main/kasairude/image/kasairu.png?token=AT5IP6FSITYG5II4WWXSGMTBPFRAS)

### それぞれの役割

- [Lambda](https://aws.amazon.com/jp/lambda/)
  - 本日の主役
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

## 環境

## AWS アカウント準備

## AWS CLI インストール

## nodejs インストール

## Open Weather Map の API key 取得

## Twitter API の API key 取得

## System Manager に API key を設定

## デプロイ

## テスト
