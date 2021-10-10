#GCP #Command
# gcloud beta artifacts repositories 

## SYNOPSIS(概要)

gcloud beta artifacts repositories COMMAND [GCLOUD_WIDE_FLAG …]

## 一覧

`gcloud beta artifacts repositories list`

```bash

REPOSITORY               FORMAT  DESCRIPTION  LOCATION         LABELS  ENCRYPTION          CREATE_TIME          UPDATE_TIME
getting-started-pytorch  DOCKER               asia-northeast1          Google-managed key  2021-08-28T19:02:22  2021-08-28T19:02:22
shinshin                 DOCKER               asia-northeast1          Google-managed key  2021-08-23T08:47:25  2021-08-23T08:47:25
test-suekuni             DOCKER               asia-northeast1          Google-managed key  2021-08-23T08:48:01  2021-08-23T08:48:01
yolo                     DOCKER               asia-northeast1          Google-managed key  2021-08-28T18:54:39  2021-08-28T18:54:39

```

## 削除

`gcloud beta artifacts repositories delete --location $REGION <REPOSITORIES_NAME>`

 ⚠ --locationを入れないと削除できないので注意。
