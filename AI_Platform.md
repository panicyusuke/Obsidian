#GCP 

# はじめに

`旧 AI Platform(新 Vertex AI)` は、GCP の持つ機械学習リソースの総称である。

その中でも今回は`Prediction` を用いて、画像内の人間の数をカウントするシステムの構築方法について説明する。

ここでは、`YoLov5` が組み込まれた予測エンジンを[TorchServe](https://pytorch.org/serve/)を利用してデプロイし、base64でエンコードされた画像をサーバーに送信し、その予測結果をレスポンスとして返すシステムについて説明する。

参考: [Vertex AI](https://cloud.google.com/vertex-ai) 

## Requirements

- [Docker](https://www.docker.com/)
- [Google Cloud SDK](https://cloud.google.com/sdk/docs/install?hl=JA)
- [サービスアカウントキー(JSON)](https://cloud.google.com/iam/docs/creating-managing-service-account-keys?hl=ja)
- [Python仮想環境( -venv )](https://docs.python.org/ja/3/library/venv.html)
	- [PyTorch](https://pytorch.org/)
	- [TorchServe](https://pytorch.org/serve/)

## GCPリソース

- [Artifact Registory(旧 Container Registry)](https://cloud.google.com/artifact-registry?hl=ja)
- [[Cloud Storage]]
- [[Predictions]]
- [[Compute Engine]]

## AI Platform

`公式ドキュメントより`
> AI Platform を使用すると、大規模な機械学習モデルをトレーニングして、トレーニング済みモデルをクラウドでホストし、モデルを使用して新しいデータについて予測できます。

- 学習: [[Predictions]]
- 推論: [[Training]]
- データラベリング・アノテーション


#### 1. Cloud Storage でバケットを作成

`development_bucket` という名前でバケットを作成

```bash

BUCKET_NAME=test-crowd
REGION=asia-northeast1
PROJECT_ID=crowd-322605
gsutil mb -b on -l $REGION gs://$BUCKET_NAME

```

#### 2. APIの有効化

- `AI Platform Training`
- `Prediction`
- `Artifact Registry API`

#### 3. Cloud Shellにログイン

`gcloud cloud-shell ssh` を実行し`Cloud shell` に入る。

⚠  初回ログイン時はsshで公開鍵を作成するので`password` を要求される（pass   無しでも可、Cloud SDK経由で接続するため各PCに依存する）

#### 4. コンテナのビルドとPUSH

ここでは例として、`mnist`  の学習済み model をダウンロードし、そのモデルを用いながら説明する。

##### 4-1. Mnist学習済みモデルをダウンロード

```bash

$ git clone https://github.com/pytorch/serve.git --branch=v0.3.0 --depth=1
$ cd serve/examples/image_classifier/mnist

```

#### 4-2. Docker用コンテナイメージの保存

```bash

$ gcloud beta artifacts repositories create getting-started-pytorch --repository-format=docker --location=REGION

```

## おまけ

実は本記事の執筆中に悲しい事実を発見しました。
`YoLov5`  に限り、[Flask-RESTAPI](https://github.com/ultralytics/yolov5/tree/master/utils/flask_rest_api) を使うことで`Docker` の細かい設定いらずで(Webアプリを実装することになりますが...) お持ちのレンタルサーバーまたはクラウドへのデプロイがとても簡単に出来るようです...

##  参考
- [クイックスタート: Cloud Storage](https://cloud.google.com/storage/docs/quickstart-gsutil?hl=ja)
- [スタートガイド: Keras によるトレーニングと予測](https://cloud.google.com/ai-platform/docs/getting-started-keras?hl=ja)
- [カスタム コンテナを使用した PyTorch 予測の提供](https://cloud.google.com/ai-platform/prediction/docs/getting-started-pytorch-container?hl=ja)
- [Yolov5_torchserve](https://github.com/jdongca2003/Yolov5_torchserve)