#TBD 

いいねに関するAPI エンドポイントは4つある。

1. [GET /2/tweets/:id/liking_users](https://developer.twitter.com/en/docs/twitter-api/tweets/likes/api-reference/get-tweets-id-liking_users): ツイートに紐づく`いいね`を取得する。
2. [GET /2/users/:id/liked_tweets](https://developer.twitter.com/en/docs/twitter-api/tweets/likes/api-reference/get-users-id-liked_tweets): ユーザーの`いいね`を取得する
3. [POST /2/users/:id/likes](https://developer.twitter.com/en/docs/twitter-api/tweets/likes/api-reference/post-users-id-likes): ツイートに`いいね` をする
4. [DELETE /2/users/:id/likes/:tweet_id](https://developer.twitter.com/en/docs/twitter-api/tweets/likes/api-reference/delete-users-id-likes-tweet_id): `いいね`を取り消す

いずれもHTTPメソッドとリソースの指定することで必要な処理を実現する。


## Reference

- [Likes](https://developer.twitter.com/en/docs/twitter-api/tweets/likes/api-reference/get-users-id-liked_tweets)