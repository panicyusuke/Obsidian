#AWS #Docker 

# DynamoDB をローカルで動かす
    
## 開発用DB作成  
  
`docker-compose.yml` を用いてコンテナをBuildして、次のデータベースを使用可能な状態にする。  
  
- `Dynamodb local`: `DynamoDB` をローカルで実行するためのコンテナ  
- `Dynamodb admin`: `DynamoDB`をブラウザから管理するためのコンソール用コンテナ  
- `db`: `PostgreSQL` 用のコンテナ  
  
```bash  
    
>> docker-compose up -d --build  
Building db  
Sending build context to Docker daemon  18.43kB  
Step 1/2 : FROM postgres:latest  
 ---> 346c7820a8fbStep 2/2 : COPY ./init.sql /docker-entrypoint-initdb.d/  
 ---> Using cache ---> 2c26330fc024Successfully built 2c26330fc024  
Successfully tagged andii-family_backend_db:latest  
Creating test_dynamodb-local ... done  
Creating db-container        ... done  
Creating test_dynamodb-admin ... done  
  
```  
  
## 初期データ投入  
  
`PostgreSQL` は`docker-compose` で初期データを明示的に(`initdb.d`)投入しているが、 DynamoDBは別途コマンドで初期データを投入する必要がある。  
  
`python notify/read.py`   
上記コマンドはテーブルを作成し、`doc/dummy.json` の初期データを投入するコマンドである。 
既にテーブルが存在する場合は`Abort the new creation method because the table exists in DynamoDB...` のエラーメッセージを返すようにしてある.    
  
### DynamoDB 初期データの確認  
  
確認方法は次の2つ。 前者は公式ドキュメント奨励の確認方法だが、`docker-compose`でローカルDynamoDBを参照・操作を可能とした`Dynamo admin`コンテナを利用した後者の方法をオススメする.  
  
#### 1. `aws コマンドを用いてテーブルを取得`
 ```bash  
>> aws dynamodb scan --endpoint-url http://localhost:8000 --table-name Read  
{  
 "Items": [  
 { "user_id": {  
 "N": "10000"  
 },  
 ... } ], "Count": 1, "ScannedCount": 1, "ConsumedCapacity": null}  
  
```  
  
#### 2. `http://localhost:8001/` にアクセスして以下のAdminページを得る.  
  
![[aws_query_editor_front.png]]
  
## テスト  
  
今回はテーブル作成・データ投入等をテストケース毎に実行、削除を行う所謂 `setup`, `teardown` 等の処理を行っている。 詳細は`tests/conftest.py`内のコードを参照されたい。  
  
```bash  
  
>> pytest . -vv --ignore-glob="venv/**/*" --durations=0 --cov  
collected 5 items   
tests/test_read.py::test_get_read_positive PASSED                                                                                                                                                   [ 20%]  
tests/test_read.py::test_get_read_negative PASSED                                                                                                                                                   [ 40%]  
tests/test_read.py::test_put_read PASSED                                                                                                                                                            [ 60%]  
tests/test_topic.py::test_get_topic_all PASSED                                                                                                                                                      [ 80%]  
tests/test_topic.py::test_get_topic_detail PASSED                                                                                                                                                   [100%]  
  
---------- coverage: platform linux, python 3.8.10-final-0 -----------  
Name              Stmts   Miss  Cover  
-------------------------------------  
db/config.py         23      1    96%  
notify/read.py       68     14    79%  
notify/topic.py      11      0   100%  
-------------------------------------  
TOTAL               102     15    85%  
  
  
```