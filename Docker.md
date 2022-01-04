#Docker
[[基本操作]]


# Dockerとは

## 基本コマンド

`docker + <上位コマンド> + <副コマンド>` が基本 

`参考`: `docker run container` など


### Docker の起動、終了、自動起動 for Linux
Dockerインストール後や、サーバーの立ち上げ時など、特別な場合にのみ用いる
起動: 
`sudo systemctl start docker`

終了: 
`sudo systemctl stop docker`

自動起動:
`sudo systemctl enable docker`

### Dockerイメージを`docker hub` から取得

`docker image pull <image_name>` 
または、
`docker pull <image_name>`

### Dockerイメージを実行

`docker container run <container_name>`
または
`docker run <container_name>`

### Option

`-d`: バックグラウンドで実行
`-i`: キーボード操作

## Docker 実践

### 基本操作

1. [[Network]]
2. [[Volume]]


## Docker DB

[[DB]]


