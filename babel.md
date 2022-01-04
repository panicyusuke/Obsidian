
`Babel`... 

`JavaScript` の文法、ES5 <=> ES6トランスパイラー

クソキモES5で書かれた無秩序なコードをES6に変換してくれる神。

# 再現手順

```bash

npm install --save-dev babel-cli
npm install --save hapi # サンプル用httpサーバモジュール hapi をインストール

```

```:.babelrc

{
  "presets": ["es2015"]
}

```

トランスパイル元

