#Python 
#PyTorch

# TorchServeってなんだ?

モデルサービングライブラリの一つです。
PyTorchで実装されたモデルをそのままAPIとして切り離せます。
一般的にPyTorchやTensorflowなどで推論用モデル（学習用は別）を作成する場合は次のような構成になります。

入力: 説明変数
処理: NNなど
出力: 分類ならクラス、回帰なら数値など

しかし、実装したモデルは`Jupyter Notebook` や`VSCode` 上でしか再現できないですよね。
モデルを作成して、なんらかのプロダクトを実装したい場合、

> 開発者は、モデル開発にさまざまなオープンソースフレームワークを使用します。ここ数年、PyTorch は、ML を利用したアプリケーションを開発する多くの研究者、開発者、およびデータサイエンティストが選択する深層学習のフレームワークとなっています。PyTorch は、そのシンプルさに加えて、Python 的な方法でモデルを実装およびトレーニングでき、Eager モードと Graph モードをシームレスに切り替える機能があることから好まれています。しかしながら、これまで、PyTorch モデルを本番環境で大規模に提供することについて、簡単かつネイティブにサポートされた方法はありませんでした。PyTorch モデルを TorchScript を使用して Eager モードまたは Graph モードでデプロイし、複数のモデルを同時に提供し、A/B テスト用に本番モデルをバージョン管理し、モデルを動的にロードおよびアンロードし、詳細なログとカスタマイズ可能なメトリクスを監視できます。TorchServe は使いやすいです。ローカルにデプロイするのに便利な CLI が付属しており、コンテナにパッケージ化して [Amazon SageMaker](https://aws.amazon.com/sagemaker/) または [Amazon EKS](https://aws.amazon.com/eks/) で簡単にスケールアウトできます。画像分類、オブジェクト検出、画像セグメンテーション、テキスト分類などの一般的な問題のデフォルトハンドラーを使用して、数行のコードでデプロイできます。初期化、前処理、後処理用の長いサービスハンドラーを記述する必要はありません。TorchServe はオープンソースです。つまり、完全にオープンであり、デプロイのニーズに合わせて拡張できます。

つまり何ができるかと言うと、`Torch`で実装されたモデル＋リクエスト処理を統合し、デプロイまでの時間を短縮してくれるもの。
機械学習エンジンのデプロイのような用途に最適。
ただし、`Webアプリ` + `Docker` + `GCP`の理解が求められ、概念としても一般的に知られるものではないので学習コストは高いかも...?
(ちなみに有名ライブラリは `/GCP` 以下にデプロイ用の設定ファイルなどがあることが多い。`Yolov5`の場合、`GAE` にデプロイするようの`app.yaml`  が用意されている)

`TorchServe` の概念については以下図を参考にされたい。

![TorchServe](https://cdn-ak.f.st-hatena.com/images/fotolife/t/takaherox/20200726/20200726152512.png)

## TorchServeのメリット (引用)

素晴らしいまとめがあったので以下引用してます。cf.  [TorchServe入門](https://takaherox.hatenablog.com/entry/2021/01/02/144216)

-   **[機械学習](http://d.hatena.ne.jp/keyword/%B5%A1%B3%A3%B3%D8%BD%AC)モデルを活用した機能のサービスへの導入高速化**
    [機械学習](http://d.hatena.ne.jp/keyword/%B5%A1%B3%A3%B3%D8%BD%AC)で作成したモデルをTorchServeによって[API](http://d.hatena.ne.jp/keyword/API)提供することができるため，サービス導入までのリードタイムを短縮することが可能となります。
-   **モデル構築部分へのリソース注力**  
    実際のビジネス現場ではモデルを構築して終わりではなく，それを活用してオペーレーションを最適化したり，ユーザに価値を届けたりします．そのため，モデル以外にもソフトウェア開発等にリソースを割く必要があるのですが．TorchServeによってその一部が簡略化できるためモデル部分にリソースを集中することができるようになります。
-   **オペレーションにおけるモデル運用の効率化**  
    複数のモデルを統一されたプラットフォームでサービングやモデル管理ができるため，運用時のオペレーションを効率化することが可能になります。
-   **アナリストによるソフトウェア開発への介入**  
    データサイエンティストがソフトウェア開発の領域に介入することを容易にするため，データサイエンティストがよりシームレスな顧客へのデリバリを可能にすることができます。

## TorchServeの構成

TorchServeは次の要素で構成される。

- `Frontend`
	- 基本的にリクエストとレスポンスを制御する
	- Interface APIを定義してエンドポイントを作成する
	- モデルと呼ばれる
	- 複数のモデルを複数のエンドポイントとして定義することが出来る
	- モデルのライフサイクル（初期化、終了のタイミング）を管理
- `Backend`
	- Model Worker
		- モデル推論を行い、実行時にはインスタンス化される
		- ひとつまたは複数の`Worker Proccess` から構成される
	- Model
		- NNの内部構造を辞書型で定義した`state_dict` などのモデルアーティファクトとともに、データに対してカスタマイズされた前処理や後処理を提供する。
		- モデルはクラウドストレージやローカルホストからもインポート出来る。
- `Other`
	- Plugin
		- TorchServeの起動時に利用者によってカスタマイズされたエンドポイントや認証認可を指定できる。（Artifact RegistryなどのGCPサービスを使用する場合はあまり使わない）
	- Model Store
		- 読み込み可能なモデル（`.pt`, `.plt`ファイル）のディレクトリを指定する（重要！）
- `Mangement API` & `Process Otherstration`

## TorchServeの実装

### 1. Python環境の構築

```python
# PyTorchのインストール: 
# pip install torch==1.9.0+cu111 torchvision==0.10.0+cu111 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html
pip install torch torchvision 

# TorchServeのインストール
pip install torchserve torch-model-archiver
```

### 2. TorchServe用モデルのエクスポート
`custom_handler.py`
```python

import time
from ts.torch_handler.base_handler import BaseHandler
import numpy as np
import torch
import torchvision
import cv2


class ModelHandler(BaseHandler):
    """
    A custom model handler implementation.
    """

	# コンストラクタ
    def __init__(self):
        super().__init__()
        self._context = None
        self.initialized = False
        self.batch_size = 1
        self.img_size = 640

	# 前処理
    def preprocess(self, data):
        """
        Transform raw input into model input data.
        :param batch: list of raw requests, should match batch size
        :return: list of preprocessed model input data
        """
        list_img_names = ["img" + str(i) for i in range(1, self.batch_size + 1)]
        inputs = torch.zeros(self.batch_size, 3, self.img_size, self.img_size)
        for i, img_name in enumerate(list_img_names):
            try:
                # Take the input data and make it inference ready
                byte_array = data[0][img_name]
                file_bytes = np.asarray(bytearray(byte_array), dtype=np.uint8)
                # yolov5 preprocessing
                img = cv2.imdecode(file_bytes, cv2.IMREAD_COLOR)
                img = img[:, :, ::-1].transpose(2, 0, 1)  # BGR to RGB, to 3x416x416
                img = np.ascontiguousarray(img)
                input = torch.from_numpy(img)
                input = input.float()
                input /= 255.0  # 0 - 255 to 0.0 - 1.0
                inputs[i, :, :, :] = input
            except:
                pass
        return inputs
	
	# 後処理
    def postprocess(self, inference_output):
        """
        Return inference result.
        :param inference_output: list of inference output
        :return: list of predict results
        """
        # Take output from network and post-process to desired format
        postprocess_output = inference_output
        return ["ok"]


```

`TorchServe用モデルのエクスポート`
```shell

$ torch-model-archiver --model-name servemodel --version 1.0 --serialized-file yolov5x.pt --handler custom_handler.py --export-path model_store

```

### 3. TorchServeの実行

```shell

$ torchserve --start --ncs --model-store model_store --models servemodel.mar

```



## 参考

- [公式TorchServe(英語版)](https://pytorch.org/serve/)
- [TorchServe入門](https://takaherox.hatenablog.com/entry/2021/01/02/144216)