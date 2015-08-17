# テーマ・プラグイン開発

## 仮想環境

[VCCW - A WordPress development environment.](http://vccw.cc/)

## 参考書籍

サイトの拡張性を飛躍的に高める WordPressプラグイン開発のバイブル

## Vagrant起動・SSHログイン

	$ vagrant up
	$ vagrant ssh

## 開発パス

	/var/www/wordpress

## Composer パッケージ

* PHP_CodeSniffer  
  VCCWはデフォルトでインストール済です。
* [phpDocumentor](http://www.phpdoc.org/)
  
## ComposerへphpDocumentorを追加
  
	$ cd  /home/vagrant/.composer

	$ vi /home/vagrant/.composer.json
    
    {
        "require": {
            "squizlabs/php_codesniffer": "*",
            "phpdocumentor/phpdocumentor": "2.*"
        }
    }
    
    $ composer update

## WP-CLI

VCCWはデフォルトでインストール済みです。

### WP-CLI実行フォルダ

wp-config.phpのフォルダで実行します。

### インストールの確認

	$ wp --info

### 実行例1 プラグインの有効化・無効化

	$ wp plugin activate <plugin-name>
	$ wp plugin deactivate <plugin-name>

### 実行例2 プラグインの土台作成

	$ wp scaffold plugin <plugin-name>

### 実行例3 単体テスト雛形作成

	wp scaffold plugin-tests <plugin-name>

## Grunt

Gruntはローカルで実行でします。


## Appendix

* sudo gem gemコマンドが見つからない 解決  
[sudo「コマンドが見つかりません」PATHが初期化されているときの対処法](http://blog.thingslabo.com/archives/000395.html)
* compassをインストールしてもcompass not found 未解決  
[compassをインストールしたのにcommand not foundになる – blog.hereticsintheworld](http://blog.hereticsintheworld.com/4181.html)
