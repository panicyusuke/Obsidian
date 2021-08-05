#Prediction

#   退会日の予測

任意の会員の傾向から、退会日を予測する。


## 説明変数

- gender (性別):  `[Male, Female]`
- age (年齢): `INTEGER`
- distance (ジムまでの距離): `FLOAT`
- 1_2 to 23_24 (時間帯別入館数の合計): `INTEGER`
- days0 to days6 (曜日別入館数の合計): `INTEGER`
- visit_count_before_N (Nヶ月前の総入館数): `INTEGER`
- visit_count_week_before_N (N週前の総入館数): `INTEGER`

## 目的変数

一ヶ月以内に退会するか否か.

退会する: 0
退会しない: 1

## 予測モデル

- XGBoost
- Random Forest
- DNN

## 結果

### コロナの影響を考慮(グリッドサーチなし)

```
テストデータ=======

precision    recall  f1-score           
0       0.61      0.58      0.59  
1       0.62      0.65      0.63

本番データ=========

precision    recall  f1-score           
0       0.62      0.54      0.58  
1       0.60      0.68      0.63

```

### コロナの影響を考慮しない(グリッドサーチなし)

```

テストデータ=======

precision    recall  f1-score           
0       0.65      0.67      0.66
1       0.66      0.63      0.65

本番データ=========

precision    recall  f1-score           
0       0.52      0.59      0.56  
1       0.53      0.46      0.49


```


## 懸念

1. distance: 登録ジムによって距離が変わるため、説明変数として、過度に信用してはいけない。（移籍した場合、 数十キロなどの説明変数になってしまうため。）
2. コロナ禍による影響をどのように除去するか
3. 説明変数として妥当なのかいくつかの疑念がある

