#React #TypeScript

`Redux` とは、
[[React]] のアプリケーションの状態管理を行うためのフレームワークのこと。
[[Store]] と呼ばれるReactのアプリケーションの状態が保存されている箱のようなものを操作することを責務としている。

[Reduxを分かりやすく解説してみた](https://future-architect.github.io/articles/20200429/)がとても参考になるのでそっちを見たほうがきちんと理解できると思う。

## Redux概念図
(図は参考の記事からお借りしました。)

![Redux概念図](https://future-architect.github.io/images/20200429/1.png)

**State**：アプリケーションの状態  
**Action**：ユーザーが何押したいかという情報を持つオブジェクト  
**Reducer**：Actionを元にStateを更新するメソッド  
**Store**：Stateの情報を保持している場所

## 注意点

上の図のように、ユーザーが直接コンポーネントを操作することでStore内の`State`  を変更することはできない。

## Reference

- [Reduxを分かりやすく解説してみた](https://future-architect.github.io/articles/20200429/)