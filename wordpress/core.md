# Core

## 環境準備

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

## テスト実行

	$cd /var/www/wordpress/wordpress-develop
	// 単一ファイル
	$ phpunit tests/phpunit/tests/post/getPostClass.php
	// グループ wordpress-develop/tests/phpunit/tests/postの配下@postアノテーションのテスト実行
	$ phpunit --group post