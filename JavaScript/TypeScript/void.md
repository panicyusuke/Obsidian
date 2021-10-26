#JavaScript #TypeScript 

# Void

最後の`console.log({data})` でレスポンスを返すとき、戻り値がない場合がある。
まあログとかかなり限定的な気がするが...
そんなときは`Void` 型を使いましょう。

```typescript

function returnNoting(): void {  
    console.log("I don't return anything")  
}  
  
console.log(returnNoting())

```