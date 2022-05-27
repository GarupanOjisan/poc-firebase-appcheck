# PoC AppCheck

Firebase AppCheckのカスタムバックエンドを検証するためのプロジェクト。

## 検証結果

検証の結果、次のような手順でAppCheckを利用することができるとわかりました。

1. カスタムトークンを生成
   1. 自身のバックエンドでユーザーを認証
   2. 認証できたら `GET /fetchAppCheckToken` でカスタムトークンを取得
2. クライアントにカスタムトークンを返す
3. クライアントは以降のリクエストで `X-Firebase-AppCheck` ヘッダーにカスタムトークンを設定
4. 自身のバックエンドでカスタムトークンを受け取り `POST /verifyAppCheckToken` をリクエスト
5. verifyAppCheckToken エンドポイントはトークンが正当であればHTTP 200を返します
6. 自身のバックエンドはverifyAppCheckTokenからHTTP 401が返ればクライアントのリクエストをエラーにします

## クライアントが必要な対応

上記の通り、backendの認証時に返ってくるカスタムトークンを
`X-Firebase-AppCheck` ヘッダーに設定してください。

## バックエンドが必要な対応

上記の通り、
認証時にfunctionsの `GET /fetchAppCheckToken` をリクエストしてカスタムトークンを取得してください。
以降のリクエストでは `POST /verifyAppCheckToken` でトークンを検証します。
