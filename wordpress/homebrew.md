# Homebrew

HomebrewでPHPとMySQLをインストールしWordPressを動かす。

[Homebrew — The missing package manager for OS X](http://brew.sh/)

## PHPインストール

	$ brew install php56

シェル(Bash)がMacにでフォルトでインストール済みのPHPではなく
Homebrewで新規にインストールしたPHPを参照するよう.bash_profileのパスを設定する。  

	$ export PATH=$(brew --prefix homebrew/php/php56)/bin:$PATH

### 確認

	// バージョン
	$ php --version
	// インストール先
	$ which php
	// php.iniのパス
	$ php --info | grep Configuration

### PHPビルトインサーバー起動

	$ php -S localhost:8080
	
### Xdebugインストール

	$ brew install php56-xdebug

xdebug.iniでxdebug.soの読み込みを記載していた。

xdebug.ini

	[xdebug]
	zend_extension="/usr/local/opt/php56-xdebug/xdebug.so"

xdebug.iniは/usr/local/etc/php/5.6/conf.d/ext-xdebug.iniへインストールされたていた。  

phpinfo関数でScan this dir for additionalを確認すると/usr/local/etc/php/5.6/conf.d/が設定されていた。
そのためXdebugのインストールでphp.iniを変更する必要ななかった。

## MySQLインストール

	$ brew install mysql

rootユーザーへのパスワード設定などのセキュリティに関する処理を行う。

	$ mysql_secure_installation

### 確認

	// バージョン
	$ mysql --version
	// インストール先
	$ which mysql

### MySQL起動

	$ mysql.server start
