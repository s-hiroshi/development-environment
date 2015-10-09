## 仮想環境

* [VCCW - A WordPress development environment.](http://vccw.cc/)  
  自作テーマやプラグイン開発用の仮想環境です。
* [Varying-Vagrant-Vagrants/VVV](https://github.com/Varying-Vagrant-Vagrants/VVV)  
  Coreの開発用仮想環境です。

### Vagrant起動・SSHログイン

	$ vagrant up
	$ vagrant ssh

## VVV

### 開発パス

	/srv/www/wordpress-develop

## VCCW

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

	error_reporting = E_ALL
	display_errors = Off

php.iniの設定はPHPファイルのini_set関数で上書きできます。  
WordPressはwp_config.phpのWP_DEBUG定数でphp.iniを上書きしエラーを表示します。

	define( 'WP_DEBUG', true );

##### エラーログ設定

php.iniの設定でエラーログをファイルへ出力します。

	log_errors = On
	error_log = /var/log/php_errors.log

PHPの実行ユーザーはvagrantです。php_errors.logファイルの実行権限に注意してください。  
(/var/logディレクトリの所有者はrootです。php_error.logは  
vagrantユーザーが読み書きできるよう設定してください。)


### Apache2(VCCW)

#### デーモン

	/usr/sbin/httpd

#### 設定ファイル

	/etc/httpd/conf/httpd.conf

#### ログ

ログはhttpd.confで設定されています

	ErrorLog /var/log/httpd/error.log

#### バーチャルホスト(未)

下記設定では上手く表示できませんでした。  
(other.devを追加しようと思いました。)

#### バーチャルドメイン設定ファイル作成(sites-available/other.conf)

/etc/httpd/sites-availableへバーチャルドメイン用ファイル作成しました。
(既存のwordpress.confをコピーしother.confを作成・修正)

	$ cd /etc/httpd/sites-available
	$ sudo cp wordpress.conf other.dev

#### sites-enableへシンボリックリンク作成

	$ cd /etc/httpd/sites-enable
	$ sudo ln -s /etc/httpd/sites-available/other.conf other.conf

#### hostsファイルへ追加

/etc/hostsへother.devを追加しました。

	$ cd /etc

	127.0.0.1   wordpress.local other.dev wordpress localhost localhost.localdomain localhost4 localhost4.localdomain4
	::1         localhost localhost.localdomain localhost6 localhost6.localdomain6 other.dev

##### Apache再起動

	$ sudo service httpd restart

### Xdebug

VCCWはXdebugがインストール済みです。  
インストールは下記スクリプトで確認できます。

	<?php
	phoinfo();

#### Xdebug設定ファイル(xdebug.ini)

	/etc/php.d/xdebug.ini

	; Enable xdebug extension module
	zend_extension=/usr/lib/php/modules/xdebug.so

### PhpStormリモートデバッグ

VirtualBoxのIPアドレスはifconfigコマンドで調べることができます。
(以下192.168.33.1と仮定します。)

	$ ifconfig

	vboxnet0: ...
		ether ...
		inet 192.168.33.1 netmask 0xffffff00 broadcast 192.168.33.255

#### Xdebugの設定(xdebug.ini)

vagrantへログインします。

	$ vagrant ssh
	$ sudo vi /etc/php.d/xdebug.ini

xdebug.iniへ下記設定を追加します。

	xdebug.remote_enable=1
	xdebug.remote_autostart=1
	xdebug.remote_host="192.168.33.1"
	xdebug.remote_port=9000
	xdebug.profiler_enable=1
	xdebug.profiler_output_dir="/tmp"
	xdebug.max_nesting_level=1000
	xdebug.idekey = "PHPSTORM"

xdebug.remote_hostはifconfigで調べた値を入力します。  
xdebug.remote_portはPhpStromのLanguages & Frameworks > PHP > Debug > Xdebugに記載されているポート番号を設定します。  
xdebug.idekeyはPHPSTORMを設定します。下記アドレスで確認できます。  
(https://www.jetbrains.com/phpstorm/marklets/)

#### httpdの再起動

	$ sudo service httpd restart

#### PhpStormサーバー設定

PhpStormの電話マークをクリックしListenの状態にします。  
Listenの状態でブラウザでvccw.devへアクセスするとIncoming Connection from Xdebugダイアログが表示されます。  
Acceptを選択すると下記サーバー項目が設定されます。  
Languages & Frameworks > PHP > Serversへvccw.devが設定されています。  
(Languages & Frameworks > PHP > Serversの設定を最初に行うこともできます。)

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