#Python 
# Pytest

`公式っぽいものから引用`

Python には単体テストを書くためのフレームワークがいくつかあります。

| フレームワーク | 説明               |
|----------------|--------------------|
| unittest       | 標準ライブラリ     |
| nose           | かつては主流だった |
| pytest         | 現在主流のもの     |

上記の通り Python は標準ライブラリを使って単体テストを書くことができますが、サードパーティ製の pytest の使い勝手が良いため、pytest を使って書かれることが多いです。そこでここでは pytest の簡単な使い方について説明します。

`引用終わり`

Pytestをメインにした理由は
1. 愛すべきIDE, `PyCharm` が強力にサポートしていて使いやすそうだから。
2. [Code Cov](https://about.codecov.io/) との相性が良いから

## 参考文献

[pytest](https://rinatz.github.io/python-book/ch08-02-pytest/)