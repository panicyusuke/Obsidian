#Python 

# Pythonのバージョンを変更方法
クソな事に`Tensorflow` が`Python 3.8` までしか対応していない。
3.9 が公開されてから相当時間が立っているのになんだこれは。
Webアプリ屋のことも考えてほしい...

余談はこのくらいにしておいて、`Pipenv` や`virtualenv`などを使いまわしている状態で、Pythonのバージョンを変更する必要が出てきた。
(`Pipenv` はGCPで未対応、正確にはデプロイ先に対応していないので、すべて`Pipenv`にすることはできない。これも本当にクソな話...)

Pythonのバージョンを変更する方法は様々であるが、ここでは現時点でGCPが対応している`virtualenv`のサブクラス`venv`を使用してバージョン切り替えを行う。

## virtualenvで切り替える
1. 任意のPythonのバージョンを選択しインストール
	```bash
	$ sudo apt-get update
	$ sudo apt-get install python3.8
	```
2. バージョンに合わせた`virtualenv` をインストール
	```bash
	$ sudo apt install python3.8-venv
	```
3. 仮想環境の作成と有効化
     ```bash
	$ python3.8 -m venv venv
	$ source venv/bin/activate
	 ```

これでまあ適当に`python --version` とかしてバージョンが切り替わっていたら成功。

## Tensorflow のセットアップ

なんと`tensorflow` 1系は`python3.7` までしか対応していないよう。
しかも`pip`  のバージョンが低いと`tensorflow==1.5`をインストール出来ないときた。なんだそれ。
その際のマジックコマンド

```bash
python -m pip install --upgrade pip setuptools
pip install cython
```

## 参考

```bash
$ python3.8 -m venv -h
usage: venv [-h] [--system-site-packages] [--symlinks | --copies] [--clear] [--upgrade] [--without-pip] [--prompt PROMPT] ENV_DIR [ENV_DIR ...]

Creates virtual Python environments in one or more target directories.

positional arguments:
  ENV_DIR               A directory to create the environment in.

optional arguments:
  -h, --help            show this help message and exit
  --system-site-packages
                        Give the virtual environment access to the system site-packages dir.
  --symlinks            Try to use symlinks rather than copies, when symlinks are not the default for the platform.
  --copies              Try to use copies rather than symlinks, even when symlinks are the default for the platform.
  --clear               Delete the contents of the environment directory if it already exists, before environment creation.
  --upgrade             Upgrade the environment directory to use this version of Python, assuming Python has been upgraded in-place.
  --without-pip         Skips installing or upgrading pip in the virtual environment (pip is bootstrapped by default)
  --prompt PROMPT       Provides an alternative prompt prefix for this environment.

```
