#JavaScript #TypeScript 

# Intersection

`type` や`interface`で定義した型を組み合わせて新たな型を定義したいとする。（ユースケースはまだイメージできていないが、有名なので多分結構使うのだろう）
その際、次のようにして定義済みの型を組み合わせることができる。
型の再利用性が高まることが期待できるので有用そうではある...

```typescript

type Pitchre = {  
    throwingSpeed: number  
}

type Batter = {  
    battingAverage: number  
}

type TwoWayPlayer = Pitchre1 & Batter1

const superPlayer:TwoWayPlayer = {
	throwingSpeed: 165,  
	battingAverage: 0.286
}

```


このようにして、既存の型を`&` でつなぎ、再定義することができる。