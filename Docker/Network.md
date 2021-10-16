#Docker 

# Network

Docker でWebサーバーや、SQLサーバーを構成しただけでは、外部のアプリケーションから接続することはできない。(直接繋がっていない)
`Docker` では、明示的にエンドポイントやポートを指定することで他のアプリケーションからアクセスできるようにすることができる。

1. サーバの作成と起動
	1. Apache
	2. Nginx
	3. MySQL
2. コンテナ間ネットワークの作成
	1. `create network` コマンド

## Apache, Nginx, MySQLの作成と起動

今回は、`localhost:8080` (`endpoint`: localhost, `port`: 8080)を用いて、Chromeからコンテナにアクセス出来る事を確かめる。

1. Apacheのコンテナの取得・起動
	`docker run --name apa000ex2 -d -p 8080:80 httpd` 
2. アクセス
	`http://localhost:8080/`
	Chrome で`It works!` という文字列が表示された
3. 停止
	`docker stop apa000ex2`
4. コンテナの削除
	`docker rm apa000ex2`
	
### Apacheサーバの同時起動

1. `docker run --name apa000ex0 -d -p 8080:80 httpd`
2. `docker run --name apa000ex1 -d -p 8081:80 httpd`
3. `docker run --name apa000ex2 -d -p 8082:80 httpd`

でApachサーバーを三台同時に起動する。
1. `http://localhost:8080/`
2. `http://localhost:8081/`
3. `http://localhost:8082/`
にアクセスして、`It works!` が正しく表示されることを確認する。

### Nginxサーバの同時起動

1. `docker run --name nginx000 -d -p 8080:80 nginx`
2. `docker run --name nginx001 -d -p 8081:80 nginx`
3. `docker run --name nginx002 -d -p 8082:80 nginx`

で実行して、各エンドポイントで
`Welcome to nginx!`
の表示があれば正しく動作している。

### MySQLサーバの起動

1. `docker run --name mysql000 -dit -e MYSQL_ROOT_PASSWORD=password mysql`

#### Options

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

#### Create Network

test_networkという名前でネットワークを作成する。
`docker network create test_network`

#### Start MySQL

覚悟。ほとんど環境変数の指定。
重要なのは、networkを指定しているところ。(見やすくするため複数行にして表示している)

```bash
$ docker run --name mysql000ex0 \
-dit \
--net=test_network \
-e MYSQL_ROOT_PASSWORD=pass \
-e MYSQL_DATABASE=wordpress000db \
-e MYSQL_USER=test_user \
-e MYSQL_PASSWORD=pass \
mysql \
--character-set-server=utf8mb4 \
--collation-server=utf8mb4_unicode_ci \
--default-authentication-plugin=mysql_native_password \
```

#### Start WordPress

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