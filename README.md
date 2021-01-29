# VercelでLaravelを動かす

一応動くけど現実的ではない。  

## 動かし方
必要なファイルは`vercel.json`と`api/index.php`

これを参考にすればいいけど`vercel.json`が少し違う。  
https://github.com/juicyfx/vercel-examples/tree/master/php-laravel

動くVIEW_COMPILED_PATHはこれ。
```
"VIEW_COMPILED_PATH": "/tmp"
```

東京リージョンで使うには
```
"regions": ["hnd1"],
```

## Databases
https://vercel.com/docs/solutions/databases

AWSのRDSなどが使えるけどここはサンプルなのでなし。

## Authentication
https://vercel.com/docs/solutions/authentication

DBが使えるならLaravelの認証が使えるはず。

## Realtime
https://vercel.com/docs/solutions/realtime

Pusherが使えそうだけどVercelで動かすLaravelではやらないことかも。

## Cron
https://vercel.com/docs/solutions/cron-jobs

VercelのドキュメントではGitHub Actionsで定期的にリクエストを送るとか書いてるけど
GitHub Actions使うならGitHub Actionsでartisanコマンドを実行すればいいだけなのでLaravelでは関係ない話。

## Email
https://vercel.com/docs/solutions/email
SESなどが使える。

## File Storage & Uploads
https://vercel.com/docs/solutions/file-storage
S3などが使えるはず。

Vercelで難しそうなのはキューワーカーやHorizonをずっと動かし続ける部分？  
個人的にはそこが一番重要。

## デプロイ時のartisanコマンド
composer scriptsに`vercel`を作ればデプロイ時に実行されるのでキャッシュやマイグレーションはこれでできそう。

```
    "scripts": {
        "vercel": [
            "@php artisan config:cache",
            "@php artisan route:cache",
            "@php artisan view:cache"
        ]
    }
```

## セッション
databaseかcookieを使う。

## TrustProxies
url()やroute()を使った時に`http://`になる場合、TrustProxiesミドルウェアで$proxiesを設定。
```
protected $proxies = '*';
```
