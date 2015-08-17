# テーマ・プラグイン開発

## 環境構築

### 仮想環境

[VCCW - A WordPress development environment.](http://vccw.cc/)

### Vagrant起動・SSHログイン

	$ vagrant up
	$ vagrant ssh

### 開発パス

	/var/www/wordpress

### Composer パッケージ

* PHP_CodeSniffer  
  VCCWはデフォルトでインストール済です。
* [phpDocumentor](http://www.phpdoc.org/)
  
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

### WP-CLI

VCCWはデフォルトでインストール済みです。

#### WP-CLI実行フォルダ

wp-config.phpが配置されているフォルダで実行します。

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


## Appendix

* sudo gem gemコマンドが見つからない 解決  
[sudo「コマンドが見つかりません」PATHが初期化されているときの対処法](http://blog.thingslabo.com/archives/000395.html)
* compassをインストールしてもcompass not found 未解決  
[compassをインストールしたのにcommand not foundになる – blog.hereticsintheworld](http://blog.hereticsintheworld.com/4181.html)


## テーマ開発

### フォルダ構成

	my-theme
	|
	|-- css
		|-- my-theme-admin.css <-- 管理画面用CSS
	|
	|-- images
	|
	|-- js
		|-- my-theme.js
	|
	|-- dev
	|
	|-- languages
	|
	|-- index.php
	|
	|-- style.css <-- 管理画面を除いた見た目はstyle.cssへ集約
	|
	|-- inc
		|
		|-- my-theme-init.php
		|
		|-- my-theme-customize.php
		|
		|-- ...
	|..

### ビルド

#### 翻訳

	$ cd <kanagata-theme-directory>
	$ find . -iname "*.php" > ./languages/tmp/phplist.txt
	$ xgettext --language=php --keyword=__ --keyword=_e --keyword=_n:1,2 --keyword=_x -f ./languages/tmp/phplist.txt -o ./languages/default.pot

Poeditを使いja.po, ja.moを作成します。

#### Grunt

	$ cd <my-theme>/dev
	$ grunt build

1. /dev/buildディレクトリへコピー
2. /dev/buildディレクトリから不必要なファイルを削除

#### style.cssの@charsetを削除

style.cssの先頭に@charset "UTF-8"があるときは削除します。


### Customize と Theme Options
 
どちらもwp_optionsテーブルに格納されます。

|種類|option_name|option_value|値の設定|値の取得|
|---|---|---|---|---|
|Customize|theme\_mods\_テーマ名| |Theme Customization API|Theme Customization API<br>get_theme_mod|
|Theme Options|register_settingsで任意に設定| |add_options関数|get_options関数|

### ユニットテスト

* テストデータ  
  https://wpcom-themes.svn.automattic.com/demo/theme-unit-test-data.xml
