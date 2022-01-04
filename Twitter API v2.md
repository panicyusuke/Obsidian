#Twitter #API
 
 
### API Doc  
  
Twitter APIはいくつか種類がある。  
APIを使うにはTwitter [Developer Platform](https://developer.twitter.com/content/developer-twitter/en) でアプリケーション登録を行う必要がある。

ここでは、最新のAPI、Twitter API v2を説明する。
PythonやJSなどでクライアントライブラリが用意されていたりするが、
ここではエンドポイントを指定し、HTTP通信（REST）を使用して、任意のエンドポイントデータを取得する。

## 取得できるデータの軸

- Tweets Lookup: ツイート起点で検索が出来る
- Manage Tweets: ツイートの制御等
- Timelines: タイムラインの内容を取得できる

## Likes

-   Twitter API v2エンドポイント：
    -   GET https://api.twitter.com/2/users/:id/liked_tweets  
        （指定されたユーザーIDによって高く評価されたツイートのリスト）
    -   GET https://api.twitter.com/2/tweets/:id/liking_users  
        （指定されたツイートIDを高く評価したユーザーのリスト）
	- 

## Usage

指定ユーザーのタイムラインを取得したい 👉 
PythonでTwitter APIを使いたい 👉 [[Twitter APIをPythonで使う]]

## Reference

- [Twiiter ヘルプセンター](https://help.twitter.com/ja/rules-and-policies/twitter-api)
- [Twitter Developer Platform](https://developer.twitter.com/en/docs/twitter-api)
- [Twitter API アップデート 何が変わって何ができるようになった？](https://www.shuttlerock.co.jp/media/detail/post-5069/)
- [Rate limits: API Call制限](https://developer.twitter.com/en/docs/twitter-api/rate-limits)
- [Twitter Authentication](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens)
- [ユーザーにAPI Call数をもらう](https://developer.twitter.com/ja/docs/authentication/oauth-1-0a/obtaining-user-access-tokens)

## News

- [TwitterのAPIがアップデートされ「いいね」などのデータにアクセス可能に](https://gigazine.net/news/20210521-twitter-api-v2-update/)