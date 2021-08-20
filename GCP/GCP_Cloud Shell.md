#GCP 

# What is Cloud Shell...?
まずは...
`公式ドキュメントより`

> Google Cloud Shell は、Google Cloud のインタラクティブなシェル環境です。これにより、Google Cloud を使った学習やテスト、ウェブブラウザからのプロジェクトやリソースの管理が簡単になります。Cloud Shell を使用すると、Cloud SDK [`gcloud`](https://cloud.google.com/sdk/gcloud?hl=ja) コマンドライン ツールと、その他の必要なユーティリティがプリインストールされて完全に認証され、必要なときにいつでも、最新の状態で利用できます。さらに、Cloud Shell には統合された Cloud Code 機能を備えた組み込みのコードエディタが付属しており、クラウドベースのアプリの開発、ビルド、デバッグ、デプロイをすべてクラウド内で行うことができます。Cloud Shell の仮想マシン インスタンスで、インタラクティブ チュートリアルを起動することや、クローン作成したリポジトリを開くこと、ウェブアプリをプレビューすることもできます。[詳細](https://cloud.google.com/shell/docs/how-cloud-shell-works?hl=ja)

つまり、`Cloud Shell` は `<PROJECT>` 内の仮想マシン上(Linux)に接続し、ファイル操作、バージョン制御、アプリケーションの実行等を行うことを可能としたサービスである。


## Cloud Shellへの接続方法

方法は次の二通りだが、`1. `はとても簡単なので説明しない。
1. [GCPコンソール](https://console.cloud.google.com/)を開き、上部の`Cloud Shellをアクティブにする`を選択
2. [ターミナルからsshを用いて接続する](https://cloud.google.com/shell/docs/using-the-cloud-shell-terminal?hl=ja)

### 1. SSHを用いてセッションを確立

`gcloud cloud-shell ssh --authorize-session`

ログインを求められるので、ブラウザを開き認証を行う。（入り方によってKeyをshellに貼り付けることがある。）
これでターミナルからGCP上のサービスにアクセス出来る。
基本は`Bash` で動作しているため、Aliasなど適宜追加して使いやすい環境にしてもらえば良いだろう。

ファイル移動、コピーなどは適宜sshを用いたコピーコマンド、`scp`  を用いると良い。

しかし、このままでファイル変更した後などにコマンドを叩かないといけないのでとても不便。

`ssh` を用いたマウントコマンド`sshfs`  を用いれば、ローカル・リモートの変更が直ちに反映されるので大変便利。

### 2. sshfsをインストール

[# sshfsで別サーバのディレクトリをマウントする](https://www.server-memo.net/tips/sshfs.html) が参考になった。
Mac, Linuxのバージョンによってインストール方法、依存関係が異なるので、各自自分の環境に合わせて設定すること。
詳細は[公式ドキュメント](https://wiki.archlinux.jp/index.php/SSHFS)を参考にされたい。
　

### 3. sshfsの実行コマンドを取得

`gcloud` コマンドの`get-mount-command`でターミナルに入力するコマンドを取得する。

#### コマンド:
`gcloud cloud-shell get-mount-command <LOCAL_DIR>` 

#### 例: 
`gcloud cloud-shell get-mount-command ~/cloud_shell_local`

#### 生成されたsshfcコマンド

`sshfs <GCP_USER_NAME>@<IP>: /home/user/cloud_shell_local -p 6000 -oIdentityFile=/home/user/.ssh/google_compute_engine -oStrictHostKeyChecking=no`

生成された`sshfs`コマンドをターミナルに入力すると画像のように、ローカル環境から`Cloud Shell` の環境に接続出来る。

