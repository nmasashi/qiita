# 作ったもの

![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda.png?raw=true)

ただ、以下の json を返すだけの RestAPI です。

```json
{
	"statusCode": 200,
	"body": "\"Hello from Lambda!\""
}
```

# 手順

用意するもの

- AWS アカウント

## lambda 関数作成

1. 任意のアカウントでログイン
1. [Lambda 関数一覧](https://ap-northeast-1.console.aws.amazon.com/lambda/home?region=ap-northeast-1#/functions)にアクセス
1. 「関数の作成」をクリック
1. 「一から作成」を選択し、関数名を入力（ここでは、「sample-function」としてます。）
1. その他の設定はデフォルトのままで「関数の作成」クリック
1. lambda 関数が作成できていたら OK

## API Gateway 作成

### API リソースの作成

1. [API Gateway ](https://ap-northeast-1.console.aws.amazon.com/apigateway/main/apis?region=ap-northeast-1)にアクセス
1. REST API の構築をクリック
1. プロトコル等を以下の画像のように設定し「API の作成」をクリック

### メソッドの作成

1. 作成した API のページにアクセスし、アクションからメソッドの作成をクリック
1. 「GET」を選択し、右のチェックマークをクリック
1. 統合タイプは Lambda 関数を選択し、Lambada 関数に作成した関数名（手順通りなら「sample-function」）を設定し、保存をクリック

### API のデプロイ

1. アクションから「API のデプロイ」をクリック
1. 以下の画像のようにステージの情報を入力
1. URL の呼び出しの URL をコピー
1. Firefox などブラウザに打ち込んで確認
