## WP REST API/WP OAUTH

ローカル環境

* OAuthサーバー
	+ WordPress  
	  vccw.dev
* OAuthクライアント
	+ MAMPP  
	  http://oauth-client.com:8888/index.php

## MAMP

VCCWのバーチャルホスト設定が上手くできなかったのでMAMPで作業を行います。

### 準備

Macのローカルのphpのバーションを上げました。

	$ curl -s http://php-osx.liip.ch/install.sh | bash -s 5.6

	$ cd ~
	$ vi .bash_profile

exportを下記のように修正しました。

	export PATH=/usr/local/php5/bin:$PATH

&raquo; [macのphpをアップデート - わすれっぽいきみえ](http://kimikimi714.hatenablog.com/entry/2013/07/06/233518)

## The WordPress OAuth Authentication API ("OAuth API")

[WP-API/OAuth1](https://github.com/WP-API/OAuth1/blob/master/docs/spec.md)

リモートクライアントのWordPresに対するアクセス(操作)の認証と許可を制御します。

## 用語

* サイト(site) WordPressがインストールされているサイト。
* クライアント ユーザーへサービスを提供するためにOAuth APIでWordPressへアクセスするプログラム。
* access token  
  クライアントごとに発行するトークン。access tokenはクライアントに許可を与えるトークンです。
* request token  
  認証過程でのみ使うトークン。認証過程でのみ利用しどのような権限も与えません。
* ユーザー(user) クライアントのユーザー。

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