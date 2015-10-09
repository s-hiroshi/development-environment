# WP REST API/WP OAUTH

[WP API and OAuth - Using WordPress without WordPress](http://www.sitepoint.com/wp-api-and-oauth-using-wordpress-without-wordpress/)

ローカル環境

* OAuthサーバー  
	wordpress.local  
* OAuthクライアント  
  http://oauth-client.com:8080/index.php  
  (MAMPのバーチャルホスト設定)

## WP-API/OAuth1

[WP-API/OAuth1](https://github.com/WP-API/OAuth1)  
[WP-API/OAuth1](https://github.com/WP-API/OAuth1/blob/master/docs/spec.md)

リモートクライアントのWordPresに対するアクセス(操作)の認証と許可を制御します。

## 用語

* サイト(site)  
  WordPressがインストールされているサイト。
* クライアント  
  ユーザーへサービスを提供するためにOAuth APIでWordPressへアクセスするプログラム。
* access token  
  クライアントごとに発行するトークン。access tokenはクライアントに許可を与えるトークンです。
* request token  
  認証過程でのみ使うトークン。認証過程でのみ利用しどのような権限も与えません。
* ユーザー(user)  
  クライアントのユーザー。

## Step 0

OAUTH APIの認証用WP APIの形で提供します。

## GET

OAUTH APIはGETによる情報の取得には関係しません。それはWP APIの守備範囲です。


## Key, Secret取得

	$ cd /var/www/wordpress
	$ wp oauth1 add
		ID: 1718
		Key: gOmxpyafVeSr
		Secret: xc4OFIoz5VsWAS8wHifz6U1498FuF9fXFmfiiisRmVrbxMwG

## コールバック関数

ポート番号が80, 443, 8080以外はwp_http_validate_urlがfalseを返すのでInvalid URLのエラーとなります。