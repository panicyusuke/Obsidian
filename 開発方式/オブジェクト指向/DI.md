DIとはデザインパターンの1つで、DI = Dependency Injection（訳: 依存性の注入） である。
まず、日本語に直訳して理解することはお勧めしない。

英語の説明は以下である。
> A dependency is an object that can be used (a service).
上記より、注入するのはオブジェクトであるということがわかります。

では、もう少し具体的に考えてみよう。
「依存性注入とは、コンピュータプログラムの[デザインパターン](https://e-words.jp/w/%E3%83%87%E3%82%B6%E3%82%A4%E3%83%B3%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3.html "デザインパターン")の一つで、[オブジェクト](https://e-words.jp/w/%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88.html "オブジェクト")などの間に生じる[依存関係](https://e-words.jp/w/%E4%BE%9D%E5%AD%98%E9%96%A2%E4%BF%82.html "依存関係")をオブジェクト内のコードに直接記述せず、外部から何らかの形で与えるようにする手法」とある。

①AとBの依存関係が固定的で、外部から分かりにくい例
```Javascript
function createPentagon() {
	const intArr = [1,2,3,4,5,6,7,8,9,10]
	
	const pentagon = intArr.splice(0,5)
	
	console.log(pentagon)
}
```

②依存性注入を利用して、Aの内部にBを直接記述しない例
```Javascript
let intArr = null
let polygon = null

function createIntArr(intArr) {
	intArr = [1,2,3,4,5,6,7,8,9,10]
}

function createPolygon(sides, polygon, intArr) {
	switch(sides) {
		case: 3
			polygon = intArr.splice(0,3)
			break
		case: 5
			polygon = intArr.splice(0.5)
			break
	}
}
```