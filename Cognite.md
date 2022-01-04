#AWS

# What is Cognito

`Amazon Cognite` は、ウェブアプリケーションおよびモバイルアプリに素早く簡単にユーザーのサインアップ/サインイン、アクセスコントロールの機能を追加できるサービス。

`AWS Cognito`を使うと、面倒かつ複雑な実装を要する「`サインイン`・`サインアップ機能`・`ユーザー情報の管理`」 を、`AWS`側に任せることが出来る。
IDのライフサイクルやセキュリティ、すらも面倒を見てくれるため大幅な工数削減が期待できる。

## サポート範囲

1. 相手の身元を確認する「認証」（ユーザープール）
2. 正規ユーザの情報を「ユーザ管理」（ユーザープール）
3. 相手に権限を与える「認可」（IDプール）

また、`Facebook`、`Amazon`、`Google`、`Apple` などのサードパーティーを通じたサインインを行うことも可能。（ソーシャルログインという）


## Pythonで認証機能の実装

### サインアップ用関数

### リクエスト例
`payload`:
```json

{
	"family_name":"",
	"given_name":"",
	"email": "test@gmail.com", 
	"password":"Password",
	"gender":"Male",
	"phone_number":"000-000-000",
}

```

### レスポンス例

#### 登録内容が不適切であった場合: 
status code: 403 Forbidden(サーバーがリクエストを理解したが、承認を拒否)
`Error: Password policy is insufficient.` 

#### 

## 参考

- [AWS Cognito](https://aws.amazon.com/jp/cognito/)
- [AWS 診断を事例としたクラウドセキュリティ。サーバーレス環境の不備や見落としがちな Cognito の穴による危険性](https://flattsecurity.hatenablog.com/entry/cloud_security_aws_case)
