#GCP 

`GCP(Google Cloud Platform)` とは...

![340*340](google.png.png)

Google が提供しているエンドユーザー10億人つよつよサービスのうちの一つ。
Google の8番目のプロダクトと言われている。

1. Google検索
2. Android
3. Google Map
4. GMail
5. Google Play
6. YouTube
7. Google Chrome
8. `Google Cloud Platform`

# GCPのコアインフラ

Webシステムに必要な五大インフラを提供する。

- BigData and ML Products
- Compute Power
- Storage
- Networking
- Security

この中でも有名なのは`Compute Power` で、
`Compute Engine`, `Kubernetes Engine` などのリソースが提供されている。

類似サービスに[Amazon Web Services](AWS)や[Microsoft Azure](https://azure.microsoft.com/ja-jp/)がある。

GCPの素晴らしさについて語りたいので興味ある人は 👉 [[GCPはいいぞ]]
Googleの思想、素晴らしさに興味ある人は 👉 [[Googleの思想]]

## Firebaseとの違い

`GCP` は、あらゆるサービスを内包しているので、XaaS(X as a service)と言える。
`Firebase` は、mBaaS(micro Backend as a service)。
`Firebase` は、バックエンドの実装を省略または縮小できるので、バックエンドエンジニアがいないチームや、簡易的なシステムを作る場合は色々と楽できることで知られている。（データベースや認証なども、`Firebase` に用意されているAPIを用いることで、ロジックの実装コストをスキップできる。）

- ？aaSがややこしすぎる 👉 [◯aaSに何があるか知っていますか？](https://career.levtech.jp/guide/knowhow/article/389/)
- FirebaseとGCPの違い 👉 [FirebaseとGoogle Cloudの違い](https://cloud-ace.jp/column/detail210/)

## 開発

さて、GCPで開発を進めようと思っても、実際に何から始めたらいいのだろうか。
とりあえずローカルにコードを書く...とか？
GCPアカウントを作る...とか？



- GCPのクラウド実行環境: [[Cloud Shell]]
- GCPリソースをローカルで実行するためのコマンド群: [[commands]]

## コンピューティング

Webサーバー、ファイルサーバー、データベースサーバー、踏み台サーバーなどオンプレのサーバーを使うのと同じような感覚でGCP上のコンピュータリソースを`仮想マシン` として使用できる。
サーバーのソフトウェア・ハードウェア要件を自由に設定できる。
これまでのオンプレサーバーからの移行も比較的容易。
コンピュータを間借りすることになるので、コストは高くなりやすい。

- [[Compute Engine]]
- `Kubernets Engine` (T.B.D.)
- `VMware Engine` (T.B.D.)

## サーバーレス

サーバーレス・コンピューティング（Serverless computing）とは、
クラウド・プロバイダーがアプリケーションに必要なマシン・リソースの割り当てを動的に管理し、事前に購入されたことに基づく請求ではなく、各アプリケーション要求を満たすために使用される個々のリソース量に基づいた請求を行うようなクラウド・コンピューティングの実行モデルである。
もちろん実態はサーバーが存在しない、というわけではなく、サーバーを意識せずアプリケーションを実行、展開できるという意味でサーバーレスという言葉を使っている。

GCPのサーバーレスについては[GoogleのサーバーレスサービスCloud Run、Cloud Functions、App Engineの比較](https://www.splunk.com/ja_jp/blog/devops/gcp-serverless-comparison.html) が大変参考になる。

### 特徴

料金体系: システムの実行時間に応じた課金
管理範囲: システムのみ
データ: ストリームデータ（データはメモリ上に乗る、ファイルシステムを利用できない）
オートスケール: 対応済み

### リソース

- 関数: [[Cloud Functions]]
- コンテナ: [[Cloud Run]]
- アプリケーション: [[GAE]]

### 認証・認可

ログインが必要なアプリケーションには、[[認証・認可]]システムを組み込む必要がある。

### コード管理

### ストレージ

### オブジェクトストレージ

- [[Cloud Storage]]

### ファイルストレージ

- [[Cloud Filestore]]

### RDB

- [[Cloud SQL]]

### NoSQL

### セキュリティ

DDoS, WAF, 機密情報の管理, シングルサインオン, 脅威検出, ID管理,  アプリケーションID管理などを行うリソース。
Cloud Armorがあれば大体のことは行ってくれるが細かなユースケースに特化したリソースもある。

- [[Security]]

### Network

ネットワーク: 

### LB

### Deploy 

- [[Cloud Build]]

### 機械学習

- [[AI_Platform]]
- [[GCP/機械学習]]

### タスク管理、スケジュール

- [[Pub Sub]]

# Reference

[AWS/Azure/GCPサービス比較 2021.10](https://qiita.com/hayao_k/items/906ac1fba9e239e08ae8)