# VCCWで開発

## VCCW

* phpunit
* phpcs

## 参考書籍

サイトの拡張性を飛躍的に高める WordPressプラグイン開発のバイブル

## 開発パス

	/var/www/wordpress

## WP-CLI実行フォルダ

wp-config.phpのフォルダで実行します。

### インストールの確認

	$ wp --info

### 実行例

プラグインの有効化・無効化

	$ wp plugin activate <plugin-name>
	$ wp plugin deactivate <plugin-name>

プラグインの土台作成

	$ wp scaffold plugin <plugin-name>

単体テスト雛形作成

	wp scaffold plugin-tests <plugin-name>

## 準備

* sudo gem gemコマンドが見つからない 解決  
[sudo「コマンドが見つかりません」PATHが初期化されているときの対処法](http://blog.thingslabo.com/archives/000395.html)
* compassをインストールしてもcompass not found 未解決  
[compassをインストールしたのにcommand not foundになる – blog.hereticsintheworld](http://blog.hereticsintheworld.com/4181.html)

## Grunt

Gruntはローカルで実行でします。
CompassがVCCWでは正常にインストールできませんでした。
