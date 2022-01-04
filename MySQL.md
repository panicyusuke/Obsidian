#Docker #DB #MySQL #TBD 

まずは、DockerでMySQLを立ち上げれるようにする。

# 目標
- `local` のボリュームと同期させる
- `network` でアクセスできるようにする
- `docker-compose` とやらを試してみる
- `K8S` とやらを試してみる

# MySQLサーバの起動

1. `docker run --name mysql000 -dit -e MYSQL_ROOT_PASSWORD=password mysql`

## Options

- `-it`: キーボード操作を許可
- `d`: バックグラウンドで実行
- `-e`: 環境変数の設定
	- `MYSQL_ROOT_PASSWORD=MySQLのパスワード`(ここではpasswordとする)

## コンテナ間ネットワークの作成

`MySQL`と`Wordpress`間でネットワークを構築する。

### 基本コマンド
`docker network create <ネットワーク名>` 

### 実践

さて、非常に面倒だが、WordPressとMySQLのサーバーを立てて、ネットワークを構築していく。
コマンドが長いので注意。

### Network作成

test_networkという名前でネットワークを作成する。
`docker network create test_network`

### MySQLサーバー起動

覚悟。ほとんど環境変数の指定。
重要なのは、networkを指定しているところ。(見やすくするため複数行にして表示している)
(コマンドは分割されていない、ワンライナーである。)

```bash
$ docker run --name mysql000ex0 \
-dit \ # オプション: バックグラウンドで実行、キーボード操作を容認
--net=test_network \ # ネットワーク指定
-e MYSQL_ROOT_PASSWORD=pass \ # 実行者のパスワード指定
-e MYSQL_DATABASE=wordpress000db \ # MySQLデータベース名指定
-e MYSQL_USER=test_user \ # MySQLユーザー指定
-e MYSQL_PASSWORD=pass \ # MySQLのパスワード指定
mysql \ # Dockerイメージ名指定、PostgreSQLならここは postgres
--character-set-server=utf8mb4 \ # 文字コード指定、utfmb4は日本語アリ、絵文字アリ
--collation-server=utf8mb4_unicode_ci \ # 
--default-authentication-plugin=mysql_native_password \ # 認証方式の設定(今回は弱い設定)
```

### WordPress起動

MySQLが起動したとしても、それを確認するのは大変。
DBは、データの管理を行う役割しかもたないので、中のデータを簡単に（コマンドを実行したり設定ファイルを参照せずに）見ることは簡単ではない。
データの中身を確認したいとき、わざわざDBシェルを起動し、

```sql

USE <DATABASE_NAME>;
SELECT * FROM <DATABASE_NAME>

%% SELECT *（アスタリスク: ワイルドカード、すべての意） 構文は %%
%% 検索負荷を無駄に上げるので実務では私用禁止！絶対ダメ！ 説明のため書いています。%%

```

などとして確認するのはキツイ。
そこで、データを可視化（描画）するためのツールが欲しくなる。
`WordPress` はそのような要求に答えるためのツールの一つであり、Dockerイメージが公式で用意されており簡単に実行できるため、ここでは例として使用することとした。
（WordPressの本来の役割は、CMS(Contents Management System)、**ブログ形式**のウェブサイトを提供することですが、Contents参照もできるのでここではWordPressを使用します。）

他にももっとDB参照を楽にしてくれるサービスはあるので、
興味があれば 👉 [[DBクライアントソフト]]

```bash
$ docker run --name wordpress000ex0 \
-dit \ 
--network=test_network \ 
-p 8080:80 \ 
-e WORDPRESS_DB_HOST=mysql000ex0 \ 
-e WORDPRESS_DB_NAME=wordpress000db \ 
-e WORDPRESS_DB_USER=test_user \ 
-e WORDPRESS_DB_PASSWORD=pass \ 
wordpress

```

これで`localhost:8080`にアクセスすると、`wordpress` が立ち上がり、MySQLへの接続が成功しているのがわかる。

成功したのが確認できればコンテナを削除し、`docker network rm test_network
`   でネットワークも削除しておく。

# Reference

- [MySQLの文字コードとCollation](https://qiita.com/tfunato/items/e48ad0a37b8244a788f6)
- [認証プラグインを指定してユーザーを作成する](https://www.dbonline.jp/mysql/user/index9.html)