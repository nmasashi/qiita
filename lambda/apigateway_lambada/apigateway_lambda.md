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
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda01.png?raw=true)
1. 「一から作成」を選択し、関数名を入力（ここでは、「sample-function」としてます。）
1. その他の設定はデフォルトのままで「関数の作成」クリック
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda02.png?raw=true)
1. lambda 関数が作成できていたら OK
   5 行目の'Hello from Lambda!'がレスポンスになるので、好きな値に変えてもいいでしょう
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda03.png?raw=true)

## API Gateway 作成

### API リソースの作成

1. [API Gateway ](https://ap-northeast-1.console.aws.amazon.com/apigateway/main/apis?region=ap-northeast-1)にアクセス
1. REST API の構築をクリック
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda04.png?raw=true)
1. プロトコル等を以下の画像のように設定し「API の作成」をクリック
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda05.png?raw=true)

### メソッドの作成

1. 作成した API のページにアクセスし、アクションからメソッドの作成をクリック
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda06.png?raw=true)

1. 「GET」を選択し、右のチェックマークをクリック
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda07.png?raw=true)

1. 統合タイプは Lambda 関数を選択し、Lambada 関数に作成した関数名（手順通りなら「sample-function」）を設定し、保存をクリック
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda08.png?raw=true)

### API のデプロイ

1. アクションから「API のデプロイ」をクリック
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda09.png?raw=true)

1. 以下の画像のようにステージの情報を入力
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda10.png?raw=true)

1. URL の呼び出しの URL をコピー
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda11.png?raw=true)

1. Firefox などブラウザに打ち込んで確認
   ![](https://github.com/nmasashi/qiita/blob/main/lambda/apigateway_lambada/images/lambda12.png?raw=true)

めでたしめでたし

# cleanup

## API Gateway 削除

1. [API Gateway](https://ap-northeast-1.console.aws.amazon.com/apigateway/main/apis?region=ap-northeast-1) にアクセス
1. 削除する API（ここでは「sample-api」）を選択し、アクションから Delete を選択
1. 削除をクリック

## Lambda 関数削除

1. [Lambda 関数一覧](https://ap-northeast-1.console.aws.amazon.com/lambda/home?region=ap-northeast-1#/functions)にアクセス

1. 作成した Lambda 関数（ここでは「sample-function」）を選択し、アクションから削除をクリック

# 最後に
