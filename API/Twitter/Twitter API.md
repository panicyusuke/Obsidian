#Twitter #API
  
Twitter APIを叩いて遊ぶ。  
  
すべてはここから。  
  
Twitter: [@panicyusuke](https://twitter.com/panicyusuke)  
> なぜ僕のツイートが伸びないか考えた結果次の仮説が導きだされた。 ツイートは面白いのだがつぶやく時間とフォロワーのツイッターを見ている時間とマッチしていない。 つまり、みんながよく見ている時間につぶやけばバズる。 > この仮説を検証するにはフォロワーがTwitterを見ている時間がいつなのかを特定して、 > そのタイミングに合わせて投稿すればよいということ。 んじゃあそれってどうやるのさ？って話だけど > Twitter APIを使って僕のフォロワーの投稿時間、 > いいね時間を吸い出して分析すればいい。 Tweepyは4年前自然言語処理でたくさん触ったので自信はある。 さあ、検証していくう  

## Twitter API for Python  

### Setup

#### Python virtual environment  
  
```shell  
  
python3 -m venv venv  
source venv/bin/activate  
  
```  
  
#### 1. Create `requirements.txt`  
  
`requirements.txt`  
```text:requirements.txt  
  
twitter  
python-dotenv  
dash  
  
```  
  
#### 2. Create `.env` for secret twitter token  
  
`.env`  
```ENV  
  
CONSUMER_KEY=YourConsumerToken  
CONSUMER_SECRET_KEY=YourConsumerSecret  
ACCESS_TOKEN=YourAccessToken  
ACCESS_TOKEN_SECRET=YourSecretToken  
  
```  
  
### API Doc  
  
Twitter APIはいくつか種類がある。  
APIを使うにはTwitter [Developer Platform](https://developer.twitter.com/content/developer-twitter/en) でアプリケーション登録を行う必要がある。

詳しくは公式ドキュメントを参考にされたい。

1.   キーワード検索のためのSearchAPI: 
2.   アカウントプロフィールを扱うアカウントAPI
3.   リアルタイムのタイムラインを分析するリアルタイムAPI
4.   ダイレクトメッセージを扱うメッセージAPI
5.   広告機能と連携する広告API
 
Twitter社が提供するHTTP通信（REST）を使ってAPIを使用することになる。   
  
## Usage

### Get timeline  
  
タイムラインを取得する  
  
```python  

from twiiter_client import client # 自作ライブラリ  
  
```

## Reference

- [Twiiter ヘルプセンター](https://help.twitter.com/ja/rules-and-policies/twitter-api)
- [Twitter Developer Platform](https://developer.twitter.com/en/docs/twitter-api)