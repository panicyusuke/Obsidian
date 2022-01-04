TypeScriptの世界は色々と統一されていない書き方が多すぎるのでここで開発ルールを統制する。

# ルール

1. `type` は使わない。`interface` を代わりに使う！
2. `function` で関数定義しない。

## `type` は使わない。`interface` を代わりに使う！
理由:
 
 ## `function` で関数定義しない。

```TypeScript
const function_name:type_of_function_name = (arg1:type1, ...):型 =>{
	return ()
}
```

## 命名規則

### camelCase

- 変数
- 関数

## PascalCase

- クラス
- インターフェース
- 名前空間

## Enumはつかわない

## 予約語っぽいやつには気をつけよう

idとか...

## named Exportsを使うべし





