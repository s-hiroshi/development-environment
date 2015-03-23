# WordPress

* [ローカル環境構築\(XAMPP\)](#local)
* [バックアップ・復元](#backup)
* [バージョンアッップ](#version-up)
* [プラグイン](#plugin)
* [Nucleusからの移行](#nucleus)


# ローカル環境構築(XAMPP)

XAMPPでローカル環境を構築する手順を記載する。  
バーチャルホストのアドレスは下記の規則にする。

    example.com -> example.localhost

バーチャルホストとWordPressのカスタムパーマリンクを併用するときの注意点。

1. mod_rewriteを有効にする(デフォルトは無効)

    LoadModule rewrite_module modules/mod_rewrite.so

2. AllowOverrideを有効化する(デフォルトは無効)

    <Directory />
        Options FollowSymLinks
        AllowOverride All
        Order deny,allow
        Deny from all
    </Directory>

3. 管理画面のパーマリンク設定を更新
  1, 2の設定後に管理画面「パーマリンク設定」の「変更の更新」を行う。

__htaccessの記載に誤りがなくても[変更の更新]処理を行わないと反映されない場合ある。__


# <a name="backup"> データバックアップと復元

## .htaccess

1. サーバーの.htaccessをローカルに作成したserver.htaccessフォルダへバックアップする。
2. ローカル用のhtaccessを作成する。

## wp-config.php

1. サーバーのwp-config.phpをローカルに作成したserver.wp-configフォルダへバックアップする。
2. ローカル用のwp-config.phpを作成する。

## 1 SQLダンプ

### phpMyAdminでダンプする場合

[データベースのバックアップ - WordPress Codex 日本語版](http://wpdocs.sourceforge.jp/%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E3%81%AE%E3%83%90%E3%83%83%E3%82%AF%E3%82%A2%E3%83%83%E3%83%97)

### コマンドラインを使う

    $ mysqldump -u <user name> -p <database name > <dump file>


## 2 ローカル用のダンプファイル修正

ドメインを書き換える。example.comをexample.localhostで構築する例を記載する。

1. www.example.comをexample.localhostへ置換する。
2. example.comをexample.localhostへ置換する。

同時に行う。

    (www)?example.com example.localhost

## 3 ローカル(XAMPP)で復元

SQLのインポートはDatabaseを作成してすぐ行う。

1. Databaseの作成  
   CREATE DATABASE `example` DEFAULT CHARACTER SET utf8;  
   __DEFAULT CARACTER SET utf8を指定する。__
2. SQLファイルのインポート
    1. phpMyAdminのインポートを使う
    2. コマンドを使う


    $ mysql -u <user name> -p <database name> < <dump file>

## 4 wordpress構築

### wp-config.php

    define('DB_NAME', '<database name>');
    /** MySQL データベースのユーザー名 */
    define('DB_USER', '<user name>');
    /** MySQL データベースのパスワード */
    define('DB_PASSWORD', '<password>');
    /** MySQL のホスト名 */
    define('DB_HOST', 'localhost');
    /** データベースのテーブルを作成する際のデータベースのキャラクターセット */
    define('DB_CHARSET', 'utf8');
    /** データベースの照合順序 (ほとんどの場合変更する必要はありません) */
    define('DB_COLLATE', '');

### WordPress管理画面でパーマリンクを変更

   /%category%/%post_id%.html

__設定後に管理画面「パーマリンク設定」で「変更の更新」を行う。上記処理を行わないと.htaccessを正しく記載していても反映されない場合ある。__

### DocumentRoot直下でWordPressを動かす

1. htaccessを作成して下記のように記載する。

```php
    # BEGIN WordPress
    <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
    </IfModule>
    # END WordPress
```

2. index.phpをドキュメントルートへ移動

3. 内容を変更する。

    変更前
        require('./wp-blog-header.php');
    変更後
        require('./wordpress/wp-blog-header.php');


## クライアント確認サーバー(ヘテムル)

http://www.mysite.com/client/exampleでWorPressを表示するときの.htaccess。

__設定後に管理画面「パーマリンク設定」で「変更の更新」を行う。上記処理を行わないと.htaccessを正しく記載していても反映されない場合ある。__

	# BEGIN heteml
	# 注意:
	# このブロックはヘテムルのコントロールパネルによって作成されました。
	# # BEGIN heteml 〜 # END heteml は編集を行わないようにご注意ください。
	
	AuthUserFile /home/sites/heteml/users/s/-/h/<user name>/web/mysite/client/example/.htpasswd
	AuthGroupFile /dev/null
	AuthName "example"
	AuthType Basic
	require valid-user
	# END heteml

	# BEGIN WordPress
	<IfModule mod_rewrite.c>
	RewriteEngine On
	RewriteBase /client/example/
	RewriteRule ^index\.php$ - [L]
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteRule . /client/example/index.php [L]
	</IfModule>

	# END WordPress

# <a name="version-up"> WordPressバージョンアップ

## 正式手順

1. 古いWordPressをローカルにバックアップ
2. 古いWordPressのSQLダンプ(下記3, 4が失敗したときに復元するため)
3. 本番環境のWordPressを最新板へバージョンアップ
4. プラグインをバージョンアップ

## プラグイン

事前にプラグインの対応状況を確認する。  

## 注意

古いバージョンと新しいバージョンでデータベース構造に変更があるとき、
古いバージョンのSQLダンプは最新板で復元できない。

例えば3.3.2から3.5.1へのバージョンアップはデータベースの構造に変更が伴う。
そのため3.3.2のSQLダンプを3.5.1へインポートするだけでは復元できない(インポートのさいにエラーが発生する)。



# NucleusからWordPressへの移行

Nucleus => MT形式 => WorPress

    移行前 http://www.example.com
    移行後 http://www.example.net

## 移行の注意

記載方法では画像はメディアライブラリに表示されない(単純に投稿内容のパスを書き換えているだけ)。

## Nucleus -> MT形式 -> WordPress

### Nucleus -> MT形式

&raquo; <a href="http://japan.nucleuscms.org/wiki/plugins:impexp">plugins:impexp [Nucleus CMS Japan Wiki]</a>

MTファイル形式でバックアップが作成される。


## ダンプファイルの編集

### 1. ドメイン編集

    置換前 http://example.com
    置換後 http://example.net

### 2. 画像フォルダパスの編集

    置換前 /media
    置換後 /wordpress/wp-content/uploads/media

### 3. リンク先画像パス編集

置換対象文字列

    <a href="/nucleus/plugins/impexp/index.php?imagepopup=example/19700101-example.gif&このあといろいろな属性が続く"><img src="http://www.example.com/media/thumbnail/example1_19700101-example.gif" いろいろな属性 /></a>

    パターン <a href="/nucleus/plugins/impexp/index.php\?imagepopup=([^&]+)[^>]+>
    置換文字列 <a href="http://example.net/wordpress/wp-content/uploads/media/$1">
