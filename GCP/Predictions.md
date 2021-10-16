#GCP 

## SYNOPSIS

`AI Platform` は`PyTorch`, `Tensorflow`, `Keras`などで定義したモデルをデプロイすることの出来るコンテナ実行環境です。
ローカル環境で動作確認済みのDockerfile からコンテナを作成し、`Artifact Registory`  に`PUSH` して実行します。
コンテナ作成、PUSHはGCPの`Cloud Shell` (GCEのインスタンスを用いて実行されるShell環境) で下準備をして`AI Platform Prediction` にデプロイして使います。

今回は `Yolov5`で実装された画像識別のモデルを`AI Platform`にデプロイしたいと思います。

## 下準備

### 1. Dockerfileを作成

```Dockerfile

FROM pytorch/torchserve:latest
USER root
RUN apt-get update
RUN apt-get install -y libgl1-mesa-glx
RUN apt-get install -y libglib2.0-0
RUN apt-get install -y python3-distutils
COPY ./ressources/ /home/model-server/ressources/
RUN chmod -R a+rw /home/model-server/
USER model-server
RUN pip3 install --upgrade pip
RUN pip install torch-model-archiver
RUN pip install opencv-python
#We test opencv :
RUN python3 -c "import cv2"
RUN pip install -r /home/model-server/ressources/yolov5/requirements.txt
EXPOSE 8080 8081
ENV PYTHONPATH "${PYTHONPATH}:/home/model-server/ressources/yolov5/"
RUN python /home/model-server/ressources/yolov5/models/export.py --weights /home/model-server/ressources/weights.pt --img 640 --batch 1
RUN torch-model-archiver --model-name yolo \
--version 0.1 --serialized-file /home/model-server/ressources/weights.torchscript.pt \
--handler /home/model-server/ressources/torchserve_handler.py \
--extra-files /home/model-server/ressources/index_to_name.json,/home/model-server/ressources/torchserve_handler.py
RUN mkdir model_store
RUN mv yolo.mar model_store/yolo.mar
CMD [ "torchserve", "--start", "--model-store", "model_store", "--models", "yolo=yolo.mar" ]


```

### Dockerイメージを作成
AI Platformではコンテナのタグ名をエンドポイントとして解釈するため定められたルールに沿ってタグを設定する。

```bash
$ REGION=asia-northeast1 # (AI Platformが動くリージョンが決められているので注意)
$ PROJECT_ID=crowd-322605
$ docker build --tag=$REGION-docker.pkg.dev/$PROJECT_ID/crowd/yolo .
```
するとイメージが作成されているのがわかる。

```bash
$ docker images
REPOSITORY                                               TAG       IMAGE ID       CREATED          SIZE
asia-northeast1-docker.pkg.dev/crowd-322605/crowd/yolo   latest    9fd7eb904942   48 minutes ago   4.53GB
pytorch/torchserve                                       latest    e38d077aa5ff   4 weeks ago      2.86GB
```

### Docker run して動作するか確認

```bash

docker run -d -p 8080:8080 --name=yolo $REGION-docker.pkg.dev/$PROJECT_ID/crowd/yolo

```

うまくコンテナが動いていれば`curl`  でステータスが返ってくる。
異常があれば ` "status": "Healthy"`が返ってくる。

　
```bash

$ curl localhost:8080/ping
{
  "status": "Healthy"
}


```

コンテナが正常に動作していても処理が正しくできているとは限らない。
人の写った適当な画像をperson.jpgとして保存して、`curl` で確認。

すると`x座標の最小値`, `y座標の最小値`, `x座標の最大値`、`y座標の最大値`, `信頼度`, `クラス名`がJSONで返ってくる。（複数の場合も同様）

```bash
$ curl -X POST \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d @instances.json localhost:8080/predictions/yolo
{
    "x1": 0.978759765625,
    "y1": 0.6526968479156494,
    "x2": 1.000288963317871,
    "y2": 0.860630214214325,
    "confidence": 0.36800479888916016,
    "class": "person"
}...


```
これで正しく動作していることを確認出来る。
ローカルで動作していることが確認できればもうコンテナは止めていいので`stop` する。　

```bash

$ docker stop yolo
yolo

```

### イメージをArtifact RegistoryにPUSH

⚠ `<ホスト名>/<プロジェクトID>/<レポジトリ>/<モデル名>` の構成になってないとPUSHできないので予め`Artifact Registory` にレポジトリを作成しておく。

```bash
$ gcloud beta artifacts repositories create crowd \
 --repository-format=docker \
 --location=REGION
Created repository [crowd].
```

レポジトリが作成できればいよいよArtifact RegistoryにPUSHする。

```bash

$ docker push $REGION-docker.pkg.dev/$PROJECT_ID/crowd/yolo

```

### モデルを作成

```bash

gcloud beta ai-platform models create crowd \
  --region=$REGION \
  --enable-logging \
  --enable-console-logging

```

### モデルのバージョンとリソースを割り当てる

```bash

gcloud beta ai-platform versions create v1 \
  --region=$REGION \
  --model=yolo \
  --machine-type=n1-standard-2 \
  --image=$REGION-docker.pkg.dev/$PROJECT_ID/crowd/yolo \
  --ports=8080 \
  --health-route=/ping \
  --predict-route=/predictions/yolo

```

### リソースにアクセスして正常に動作しているか確かめる

```bash

curl -X POST \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d @instances.json \
  https://$REGION-ml.googleapis.com/v1/projects/$PROJECT_ID/models/yolo/versions/v2:predict

```

### For GPU

- [[for_GPU]]