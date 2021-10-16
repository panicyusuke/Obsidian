#GCP 

[Cloud Build](https://cloud.google.com/build/docs?hl=ja) は、[Google Cloud Platform](https://cloud.google.com/gcp/) インフラストラクチャでビルドを行うサービスです。Cloud Build は、Cloud Storage、Cloud Source Repositories、GitHub、Bitbucket からソースコードをインポートし、仕様に合わせてビルドを実行して、Docker コンテナや Java アーカイブなどのアーティファクトを生成します。

# gcloud app deployが遅いとき

1. Cloud Buildのマシンスペックをあげよう(カスタム仮想マシンサイズを使用する)！
2. キャッシュイメージ(for Docker)を利用しよう！
3. GCSでディレクトリキャッシュを使ってディレクトリを再利用しよう！