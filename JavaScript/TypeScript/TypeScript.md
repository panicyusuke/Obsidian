#TypeScript 

## Setup TypeScript Environment

(tscコマンドは使わない。コンパイル→実行の流れが面倒なので)

初めて`TypeScript`を実行するときは次のコマンドを使用する。

```bash

# 管理を開始
Etude_React ❯❯❯ npm init -y

# 必要モジュールをインストール
Etude_React ❯❯❯ npm install @mui/material typescript @types/node ts-node ts-node-dev axios

# index.ts をrootに作成
Etude_React ❯❯❯ touch index.ts

# 現在地を確認
Etude_React ❯❯❯ pwd
/home/harapeco/go/src/gitlab.com/panicysk/etude_react

# ts-nodeを使用してコンパイルと実行を同時に行う(一括実行)
Etude_React ❯❯❯ npx ts-node-dev index.ts

# ts-nodeを使用してコンパイルと実行を同時に行う(開発モード)
Etude_React ❯❯❯ npx ts-node-dev --respawn index.ts
{ message: 'message' }

```

`index.ts`
```ts

const message: string = "message"  
console.log({message})

```
## 基本文法

### 宣言

JavaScriptはモジュールシステムを採用していないため、宣言した変数はすべてグローバル変数として扱われる。(外部ファイルを参照したりアクセスする機能を有さない、普通の言語とは大きく違う点)
そのため、`name`などの既にシステムに使われているような単語（予約語）を宣言するとコンパイルエラーになる。（汎用的な名前は使わないほうが良い）
`export {}` があれば単一モジュール（ファイル）内の変数として認識される。

```typescript
export {}
let name = "TypeScript"
```


注意したいのが次の宣言方法による違いである。
let やconstはES2015から定義された新しい記法であり、var使用による問題点を防ぐために整備された経緯がある。
例えば、varは関数内であればどこからでも再定義、呼び出し可能であるが、const, let は明示してやらないとブロック内のスコープにとどまるため、基本的には安全な気がする。


```typescript

const pie: number = 3.14 // 定数
let radius: number = 10 // 変数
let circumference: number = 2 * 3.14 * radius // 変数


```


### 型

- 真偽値: [[Boolean]]
- 配列: [[配列]]
- 型定義: [[AnyとInterfaceとType]]
- 型複合: [[Intersection]]
- 返り値なし: [[void]]
- 連想配列: [[Object]]

### 関数とクラス

- 関数: [[function]]
- クラス: [[クラス]]

## UI

- [[React]]