# テーマ・プラグイン開発

## 環境構築

### 仮想環境

* [VCCW - A WordPress development environment.](http://vccw.cc/)  
  自作テーマやプラグイン開発用の仮想環境です。
* [Varying-Vagrant-Vagrants/VVV](https://github.com/Varying-Vagrant-Vagrants/VVV)  
  Coreの開発用仮想環境です。

## VVV

### 開発パス

	/srv/www/wordpress-develop

## VCCW

### Vagrant起動・SSHログイン

	$ vagrant up
	$ vagrant ssh

## ホーム

	/home/vagrant

### 開発パス

	/var/www/wordpress

### PHP(VCCW)

#### PHP実行ユーザー

vagrant

実行ユーザーは下記スクリプトで確認できます。

	<?php
        echo `whoami`;

#### PHPプログラムパス

	/usr/bin/php

#### PHP設定ファイルパス(php.ini)

	/etc/php.ini

#### エラー制御

PHPのエラー制御はphp.iniで設定します。  
php.iniで設定した値はini_set関数でPHPファイルから上書きできます。  
(よってWordPressのwp-config.phpの設定で上書きされます。)

##### エラー表示制御

php.iniで下記設定を行うとエラーは画面に表示されません。
(php.iniの設定は公開サイトではこの設定が推奨です。)

	error_reporting 
	display_errors = Off

php.iniの設定はPHPファイルのini_set関数で上書き出来ます。  
WordPressはwp_config.phpで下記設定を行うことによりphp.iniを上書きしエラーを表示します。

	define( 'WP_DEBUG', true );

##### エラーログ設定

php.iniで下記設定を行うとエラーをファイルへ出力します。

	log_errors = On
	error_log = /var/log/php_errors.log

PHPの実行ユーザーはvagrantなのでphp_errors.logファイルの実行権限に注意してください。


### Apach2(VCCW)

デーモン

	/usr/sbin/httpd

設定ファイル

	/etc/httpd/conf/httpd.conf

ログ

httpd.confのログ設定

	ErrorLog /var/log/httpd/error.log

ログファイル

	/var/log/httpd/error.log

### XDebug

VCCWはXDebugがインストール済みです。

	<?php
	phoinfo();

XDebug設定ファイル xdebug.ini

	/etc/php.d/xdebug.ini

下記項目をxdebug.iniへ追加

	xdebug.collect_vars=on
    xdebug.collect_params=4
    xdebug.dump_globals=on
    xdebug.dump.GET=*
    xdebug.dump.POST=*
    xdebug.show_local_vars=on

### Composer

#### パス

	/home/vagrant/.composer

* PHP_CodeSniffer  
  VCCWはデフォルトでインストール済です。
* [phpDocumentor](http://www.phpdoc.org/)  
  クラス図の描画に必要なgraphvizを上手くインストールできていません。
  
### ComposerへphpDocumentorを追加
  
	$ cd  /home/vagrant/.composer

composer.jsonへphpdocumentor追加します。

    {
        "require": {
            "squizlabs/php_codesniffer": "*",
            "phpdocumentor/phpdocumentor": "2.*"
        }
    }

phpdocumentorをインストールします。
    
    $ composer update

composer.jsonのあるディレクトリで上記コマンドを実行するとcomposer.jsonの内容をもとにアップデートします。

### WP-CLI

VCCWはデフォルトでインストール済みです。

#### WP-CLI実行ディレクトリ

wp-config.phpが配置されているディレクトリで実行します。

#### インストールの確認

	$ wp --info

#### 実行例 

	//プラグインの有効化・無効化
	$ wp plugin activate <plugin-name>
	$ wp plugin deactivate <plugin-name>

	// プラグインの土台作成
	$ wp scaffold plugin <plugin-name>

	// 単体テスト雛形作成
	wp scaffold plugin-tests <plugin-name>

## MySQL


## root

管理者ユーザーです。  
VCCWにデフォルトで設定されています。

ユーザー名: root  
パスワード: wordpress

## wordpress

テーマ、プラグイン開発用のMySQLユーザーです。  
VCCWにデフォルトで設定されています。
操作できるデータベースはwordpressのみです。

ユーザー名: wordpress  
パスワード: wordpress  
データベース: wordpress


## テーマ開発

### ディレクトリ構成

	my-theme
	|
	|-- css
		|-- vendor
			|--bootstrap
			|--..
		|
		|-- my-theme-admin.css // (必要であれば)管理画面用CSS
	|
	|-- images
		|-- default.png 	// header imageのデフォルト画像
	|
	|-- js	// Gruntでtheme1.js .. theme2.jsを結合しmy-theme/js/theme.jsを作成
		|-- theme1.js
		|-- ..
		|-- themeN.js
	|
	|-- dev // 開発ディレクトリ(Grunt実行)
		|-- css
			|-- some.scss	// @importでstyle.scssへインポート
			|-- ..			// @importでstyle.scssへインポート
			|-- someN.scss	// @importでstyle.scssへインポート
			|-- ..			// @importでstyle.scssへインポート
			|-- style.scss  // コンパイルしてmy-theme/style.cssを作成
		|
		|-- js
			|-- vendor
					|-- some.js
			|
			|-- theme.js	// Grunt my-theme/dev/js/theme1.js..themeN.jsを結合し作成
		|
		|-- config.rb		// Compass設定ファイル
		|-- Gruntfile.js	// Grunt設定ファイル
		|-- package.json	// Grunt依存関係管理ファイル
	|
	|-- languages
		|-- default.po		// 配布
		|-- default.pot
		|-- default.mo
		|-- ja.po			// 配布
		|-- ja.mo			// 配布
	|
	|-- inc
		|-- my-theme-xxx.php
		|-- ...
	|
	|-- functions.php
	|-- ..
	|-- index.php
	|-- ..
	|-- style.css 			// Gruntでmy-theme/dev/css/style.scssをコンパイルし作成
	|-- readme.txt			// http://test.wordpress.org/plugins/about/validator/でチェック
	|-- screenshot.png

### ビルド

#### 翻訳

	$ cd <kanagata-theme-directory>
	$ find . -iname "*.php" > ./languages/tmp/phplist.txt
	$ xgettext --language=php --keyword=__ --keyword=_e --keyword=_n:1,2 --keyword=_x -f ./languages/tmp/phplist.txt -o ./languages/default.pot

Poeditを使いja.po, ja.moを作成します。

##### 配布

* default.po
* ja.mo
* ja.po

#### Grunt

	$ cd /path/to/my-theme/dev
	$ grunt build

1. /dev/buildディレクトリへコピー
2. /dev/buildディレクトリから不必要なファイルを削除

#### style.cssの@charsetを削除

style.cssの先頭に@charset "UTF-8"があるときは削除します。


### Customize と Theme Options
 
どちらもwp_optionsテーブルに格納されます。

|種類|option_name|値の設定|値の取得|
|---|---|---|---|
|Customize|theme\_mods\_テーマ名|Theme Customization API|Theme Customization API<br>get_theme_mod|
|Theme Options|register_settingsで任意に設定|add_options関数|get_options関数|

### テーマユニットテスト

* テストデータ  
  https://wpcom-themes.svn.automattic.com/demo/theme-unit-test-data.xml


## プラグイン開発


# プラグイン作成

## セキュリティー

nonce(number used once)を使いセキュリティーを確保します。

### 送信元

フォーム

	<form>
	<?php echo wp_nonce_field( 'example_action', '_wpnonce' ); ?>
	....

リンク

	wp_nonce_url(
		add_query_arg( array( 'action' => 'example' ), $url ),
		'example_action'
	);

送信先プログラム

	// 送信元が管理画面の場合
	if ( isset( $_POST['_wpnonce'] ) && $_POST['_wpnonce'] ) {
		if ( check_admin_referer( 'example_action', '_wpnonce' ) {
			// 処理
		}
	}

	// 送信元が管理画面では無い場合
	$nonce = isset( $_POST['_wpnonce'] ) ? $_POST['_wpnonce'] : null;
	if ( wp_verify_nonce( $nonce, 'example_action' ) )  {
		// 処理
	}


## 構成

* オプション Setting API  
	update_option/get_option/delete_option
* カスタム投稿タイプ  
  register_post_type
	+ カスタムフィールド  
	  add_meta_box WordPressはカスタムフィールドをwp_postmetaへ保存するのでadd_meta_boxを使います。


### Subversion

[https://wordpress.org/plugins/about/svn/](https://wordpress.org/plugins/about/svn/)

#### 最初のコミット

	$ cd wp-content/plugins
	$ svn co http://plugins.svn.wordpress.org/category-monthly-archives/ my-plugin-dir
	
	$ cd my-plugin-dir
	$ svn add trunk/*
	$ svn ci -m 'Adding first version of my plugin'

#### バージョンアップ(タグ付け)

WordPressはタグを付けるとバージョンアップになります。下記例はバージョン0.0.2を公開する例です。

	$ cd my-plugin-dir
	$ svn cp trunk tags/0.0.2
	$ svn ci -m "tagging version 0.0.2"


## Core

### 環境準備

	$ vagrant ssh
	// Vagrantへログイン
	$ cd /var/www/wordpress
	// wordpress-developディレクトリ作成
	$ mkdir wordpress-develop
	// WordPress.orgより開発ソースチェックアウト
    $ svn co https://develop.svn.wordpress.org/trunk/ wordpress-develop
    // Mysql rootユーザーでwordpress_testデータベース作成
    $ mysql -u root -p wordpress
    mysql > CREATE DATABASE wordpress_test;
    // wordpress_testデータベースを操作出来るMySQLユーザー(develop)作成
    mysql > grant all privileges on *.* to develop@localhost identified by 'wordpress';
    // wp-tests-config-sample.phpをwp-tests-config.phpへ変更しデータベース情報設定

### テスト実行

	$cd /var/www/wordpress/wordpress-develop
	// 単一ファイル
	$ phpunit tests/phpunit/tests/post/getPostClass.php
	// グループ wordpress-develop/tests/phpunit/tests/postの配下@postアノテーションのテスト実行
	$ phpunit --group post


## 案件

### ツール

* CSS プリプロセッサー
    + Sass/Compass
* ドキュメンテーションツール
    + CSS  
      hologram
    + JavaScript  
      YUI Doc
* コードインスペクション
	+ WordPress  
	  PHP_CodeSniffer
    + JavaScript  
      JSLint, JSHint
* デプロイ
    + grunt-sftp-deploy
* 自動化ツール Grunt


### フォルダ構成の例

	mytheme
	|
	|— css						// サードパーティーCSS
	|
	|- dev
		|
		|— css					// コンパイルして mytheme/style.cssを作成
			 |— _style1.scss
			 |- _style2.scss
			 |..
			 |- _styleN.scss
			 |..
			 |- _style.scss
		
		|— js					// grunt-contrib-concatで結合してmytheme/js/mytheme.js作成
			 |— script1.js
			 |- script2.js
			 |..
			 |- scriptN.js
		|
		|- jsdocs				// YUIDocフォルダ grunt-contrib-yuidocでJavaScrptドキュメンテーション作成
		|
		|- node_modules
		|
		|- phpcs				// PHP_CodeSniffer
		|
		|- styledocs            // hologramフォルダ grunt-hologramでスタイルガイド作成
		|
		|- wpcs					// WordPress-Coding-Standardsフォルダ
		|
		|- ftppass				// grunt-sftp-deployのFTP情報ファイル
		|
		|- config.rg			// Compass設定ファイル           
		|
		|— Gruntfile.js         // Grunt設定ファイル
		|
		|- hologram_config.yml	// hologram設定ファイル
		|
		|— package.json         // Gruntパッケージファイル
	|
	|- images					// テーマの画像 
	|
	|- js						//  mytheme.jsとサードパーティーのスクリプト
	|
	|- index.php
	|
	|- style.css


### Sass

CSSプリプロセッサー。

[Sass: Syntactically Awesome Style Sheets](http://sass-lang.com/)

#### Sassインストール

    $ sudo gem install sass

#### コンパイル

style.scssをコンパイルし同フォルダにstyle.cssを作成する例。

    $ sass style.scss style.css

#### 変更を監視して自動コンパイル

    $ sass --watch style.scss:style.css

ctrl + Cで監視を停止する。

### Compass

Sassを使ったCSS作成フレームワーク。

[Compass Home | Compass Documentation](http://compass-style.org/)  
[Sass/Compass のインストールと基本的な環境設定 | Web Design Leaves](http://www.webdesignleaves.com/wp/htmlcss/652/)

#### インストール

    $ sudo gem install compass
 
#### 初期化

    $ compass create --bare
    
contrib.rbとsassフォルダが作成される。

#### config.rb(Compass設定ファイル)

    $ cd /path/to/mytheme/dev
    $ compass create --bare

	mytheme
    |
    |—dev
    |  |— css
    |  |    |— style.scss
    |  |
    |  |— contrib.rb
    |
    |—index.html
    |
    |- style.css

contrib.rbの設定例

    // 設定
    css_dir = ".."
    sass_dir = “css”
    // compass専用コメントの出力を停止
    line_comments = false

上記例ではcompassコマンドでコンパイルするとdev/cssフォルダのモジュールファイルを除いたscssファイルをコンパイルしmythemeディレクトリへ出力する。

#### 変更を監視して自動コンパイル

    $ compass watch css/style.scss

### Grunt

[Grunt: The JavaScript Task Runner](http://gruntjs.com/)

Gruntはgrunt-cliのみグローバルへインストールしGrunt本体も含めて各パッケージはプロジェクトごとのフォルダへインストールする。

#### 1. node/npmのインストール

[node.js](http://nodejs.org/)

Gruntはnode.js/npmを使う。

node.jsはサイトからpkgファイルをダウンロードしインストールする。  
npmはnode.jsと一緒にインストールされる。

#### 2. grunt command line interface(grunt-cli)をグローバルへインストール

	$ sudo npm install -g grunt-cli

#### 3. プロジェクトフォルダにpackage.jsonを作成

	{
	  "name": "example",
	  "version": "0.0.1"
	}

#### 4. プロジェクトフォルダにGrunt本体とプラグインをインストール

grunt本体とプラグインはプロジェクトごとにプロジェクトフォルダへインストールする。

#### 5. Grunt本体のインストール

	$ cd /path/to/project
	$ npm install grunt --save-dev

/path/to/projectは上記フォルダ構成の例でははdevのパス。  
–save-devをつければpackage.jsonのdevDependenciesへプラグイン情報が追記される。

	{
	  "name": "example",
	  "version": "0.0.1",
	  "devDependencies": {
	  "grunt": "~0.4.1"
	  }
	}

### hologram

スタイルガイド。

### インストール

	$ sudo gem install hologram

### Gruntへ導入

	$ npm install grunt-hologram --save-dev

### hologram init

Gruntfile.jsがあるフォルダで下記コマンド実行します。

	$ hologram init

設定フィルhologram_config.ymlが作成されます。

### Gruntfile.js

Gruntfileへ下記を記載します。

	grunt.loadNpmTasks('grunt-hologram');
	
	hologram: {
		generate: {
			options: {
				config: 'hologram_config.yml'
			},
		},
	},

## YUI Doc - JavaScript キュメンテーションツール

[YUIDoc – Javascript Documentation Tool](http://yui.github.io/yuidoc/)  
[YUIDoc Syntax Reference](http://yui.github.io/yuidoc/syntax/index.html)

### インストール

	npm -g install yuidocjs.

### コマンド

	yuidoc

### Grunt + YUI DOC

	yuidoc: {
		compile: {
			name: 'mytheme',
			description: 'mytheme documentation.',
			version: '0.0.1',
			url: 'http://www.example.com',
			options: {
				paths: "js",
				outdir: "jsdocs"
			}
		}
	}

### SFTPを使ったデプロイ

[thrashr888/grunt-sftp-deploy · GitHub](https://github.com/thrashr888/grunt-sftp-deploy)


### PHP_CodeSnifferとWordPress-Coding-Standards

下記リンクにPHP_CodeSnifferをスタンドアローンでインストールする方法が記載してある。

[WordPress-Coding-Standards/WordPress-Coding-Standards](https://github.com/WordPress-Coding-Standards/WordPress-Coding-Standards)

mytheme/dev/phpcs、mytheme/dev/wpcs

	cd /path/to/mytheme/dev
	git clone https://github.com/squizlabs/PHP_CodeSniffer.git phpcs
	git clone -b master https://github.com/WordPress-Coding-Standards/WordPress-Coding-Standards.git wpcs
	cd phpcs
	./scripts/phpcs --config-set installed_paths ../wpcs

#### Gruntへの設定

	phpcs: {
		application: {
			src: ["../*.php"]
		},
		options: {
			bin: './phpcs/scripts/phpcs',
			standard: "WordPress"
		}
	},

## Appendix

* sudo gem gemコマンドが見つからない 解決  
[sudo「コマンドが見つかりません」PATHが初期化されているときの対処法](http://blog.thingslabo.com/archives/000395.html)
* compassをインストールしてもcompass not found 未解決  
[compassをインストールしたのにcommand not foundになる – blog.hereticsintheworld](http://blog.hereticsintheworld.com/4181.html)

