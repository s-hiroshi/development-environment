# テーマ・プラグイン開発

## 環境構築

### 仮想環境

[VCCW - A WordPress development environment.](http://vccw.cc/)

### Vagrant起動・SSHログイン

	$ vagrant up
	$ vagrant ssh

## ホーム

	/home/vagrant

### 開発パス

	/var/www/wordpress

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

### ユニットテスト

* テストデータ  
  https://wpcom-themes.svn.automattic.com/demo/theme-unit-test-data.xml
  






## Appendix

* sudo gem gemコマンドが見つからない 解決  
[sudo「コマンドが見つかりません」PATHが初期化されているときの対処法](http://blog.thingslabo.com/archives/000395.html)
* compassをインストールしてもcompass not found 未解決  
[compassをインストールしたのにcommand not foundになる – blog.hereticsintheworld](http://blog.hereticsintheworld.com/4181.html)

