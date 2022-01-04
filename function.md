## function

```TypeScript:function.ts

function 関数名(引数リスト): 返り値の型 {
  return 返り値
}

```

## allow_function

`return` 文を書かずに返り値を返すことができる。
`TypeScript` の流儀でいくと引数と返り値に型を指定する必要がある。


関数の定義方法の一つ。

```TypeScript:allow_function.ts

export {}  
  
let bmi: (height: number, weight: number) => number = (  
    height: number,  
 	weight: number  
): number => weight / (height * height);  
  
console.log(bmi(171, 65))

```

## optional

引数が不定の場合の関数。
引数の指定部分に`?`をいれてやることでオプション引数にすることができる。

```TypeScript: Optianl

let bmi: (height: number, weight: number, printable?: boolean) => number = (  
    height: number,  
 weight: number,  
 printable: boolean  
): number => {  
    const bmi: number = weight / (height * height)  
    if (printable) {  
        return -1  
 } else {  
        return bmi  
    }  
}  
  
let bmi_true: number = bmi(1.71, 65, true)  
let bmi_false: number = bmi(1.71, 65, false)  
  
console.log('bmi true: ', bmi_true, 'bmi false: ', bmi_false)
>> bmi true:  -1 bmi false:  22.229061933586404

```

## default

関数の引数は定義しているが、指定されなかった場合、デフォルト値が格納される。
`default`と `optional` は型か値かの違い。

 ```TypeScript:default値

 const caluclate_tax = (price:number, tax_rate = 1.05) => {
 	return price * tax_rate
 }

 ```
 
 Pythonと同じ雰囲気で書くことができる。

## Rest（残余引数）

引数が不定の場合に用いる。
可変長引数とも呼ばれる。

```TypeScript:Rest(残余引数)

function iTakeItAll(first, second, ...allOthers) {
	console.log(allOthers);
}
iTakeItAll('foo', 'bar'); // []
iTakeItAll('foo', 'bar', 'bas', 'qux'); // ['bas','qux']
```

## まとめ

```TypeScript

// 昔からあるfunctionの引数に型付け。書く引数の後ろに型を書く。
// 返り値は引数リストの () の後に書く。
function checkFlag(flag: boolean): string {
  console.log(flag);
  return "check done";
}

// アロー関数も同様
const normalize = (input: string): string => {
  return input.toLowerCase();
}

```


## Disscussion

- [JavaScript: 通常の関数とアロー関数の違いは「書き方だけ」ではない。異なる性質が10個ほどある。](https://qiita.com/suin/items/a44825d253d023e31e4d)
- [【JavaScript】function()ではなくアロー関数を使うメリット](https://keita-blog.com/programming/arrow-function)

