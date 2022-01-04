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

