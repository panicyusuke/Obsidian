#JavaScript #TypeScript 

# AnyとInterfaceとType

未知のAPIを叩いたとき、エディタがレスポンスの型を推測することは不可能。
だって未知なんだから。
でも`TypeScript` の流儀は型付け。
そこで未知のレスポンスに対しては `Any` というすべてを許容する型を与える。
また、エディタには推測不可能でもコーダーにとって未知でない場合は`Interface` を積極採用すべし。
Interfaceは継承可能で、再定義しても定義済みのInterface情報を初期化せずに追加する。
つまり、後からInterfaceの型を変更しようとしても対応できる。

```typescript

import axios from "axios"  
let url: string = "https://udemy-utils.herokuapp.com/api/v1/articles?token=token123"  
  
axios.get(url).then(function (response) {  
        interface Article {  
            id: number  
			title: string  
			description: string  
 		}  
  
        let data: Article[]  
        data = [
			{  
				id: 1,  
				title: "aa",  
				description: "as"  
 			}
		]  
        console.log({data})  
    }  
)

```


しかし、後から追加できるというのには賛否両論がある。
Interfaceによる継承を使用すると、コード記述量が増えたときにわからなくなってしまう可能性があることが指摘されている。
そのため、別のTypeという型定義の方法がある。
公式は`Type`ではなく、`Interface`を推奨しているが、ここではTypeについても紹介する。

```typescript
type Profile = {  
	first_name: string  
	last_name: string  
	age: number  
}
```

この方式を使うと、気安く再定義ができない。（初期化されてしまう）
そういう思想でコーティングしたいときにはこちらを使う。

また、複数の型定義を再利用したい場合は、[[Intersection]] を使用すると良い。