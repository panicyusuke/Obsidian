# 概要
`React DnD`とは内部で[[Redux]]を使用しており、概念的にFluxやReduxのアーキテクチャに似ている

## Items and Types

[[Redux]]と同様にReactDnDは`view`ではなく`data`を使用します。
つまり、画面上のアイテムをドラッグするとき、コンポーネントやDOMノードがドラッグされているわけではなく、特定の**タイプ**の**アイテム**がドラッグされています。
```TypeScript
const toDoType={
	toDoId:number,
	toDoName:string
	}
	
const toDo={
	toDoId:1,
	toDoName:'起きる'
	}
```

```TypeScript
const pieceType={
	fromCellX:number,
	fromCellY:number,
	kindsOfPiece:'porn'|'knight'|'queen'
	}
	
const piece:pieceType={
	fromCellX:3,
	fromCellY:5,
	kindsOfPiece:'porn'
	}
```
- What is an item?
上記、例で言うと`{toDoId:1,toDoName:'起きる'}`や `{fromCellX:3,fromCellY:5,kindsOfPiece:'porn'}`などがアイテムとなります。
**ドラッグされたデータをプレーンオブジェクトとして記述すると、コンポーネントを分離して相互に干渉しないように記述することができます。**

- What is a type?
タイプとは、アプリケーション内のアイテムのクラス全体を一意に識別する文字列（または**シンボル**）のことです。ToDoアプリでは、IDを表す`number`タイプと、ToDoアプリの内容を表す`string`タイプがあります。チェスのピースでは、ピースの項目が決まっているため`kindOfPiece`のタイプは指定できるかもしれません。

## Monitors

ドラッグ&ドロップは本質的にステートフル（状態をもつ）です。ドラッグ操作が進行中であるかどうかではなく、現在の`Items`と`Types`があるかどうかです。


React DnDは、`Monitors`と呼ばれる内部状態ストレージ上のいくつかの小さなラッパーを解して状態をコンポーネントに渡します。**Monitorsを使用すると、ドラッグ&ドロップの状態の変化に応じてコンポーネントの`porps`を更新できます**

ドラッグ＆ドロップの状態を追跡する必要がある各コンポーネントに対して、モニターから関連する部分を取得する**収集関数**を定義することができます。React DnDは、収集関数をタイムリーに呼び出し、その戻り値をコンポーネントの`props`にマージすることを行います。

## Connectors

バックエンドがDOMイベントを処理し、コンポーネントがReactを使ってDOMを表現している場合、バックエンドはどのDOMノードを使えばいいのかをどうやって知るのでしょうか？そこで登場するのが`connectors`です。コネクタでは、あらかじめ定義された役割（ドラッグソース、ドラッグプレビュー、ドロップターゲット）のいずれかを `renderfunction` の DOM ノードに割り当てることができます。