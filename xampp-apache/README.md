# XAMPP 環境構築

[XAMPP Installers and Downloads for Apache Friends](https://www.apachefriends.org/jp/index.html)

## 目標

XAMPPにバーチャルホストを設定する。

## 目次

* [XAMPPインストール](#install)
* [バーチャルホスト](#virtual-host)
    + [設定ファイル](#setting)
    + [サブドメイン](#sub-domain)
    + [ポート番号](#port)



## <a name="install">インストール</a>

XAMPP OS X版 5.6.3-0

    /Applications/XAMPP



## <a name="virtual-host">バーチャルホスト</a>

バーチャルホストは2通りの方法がある。

1. サブドメインを使う。
2. ポート番号を使う。


### <a name="setting">設定ファイル</a>

* httpd.conf
  /Applications/XAMPP/xamppfiles/etc/httpd.conf
* httpd-vhosts.conf
  /Applications/XAMPP/xamppfiles/etc/extra/httpd-vhosts.conf
* hosts
  /private/etc/hosts



### <a name="sub-domain">サブドメイン使用</a>

example.localhostを例として記載する。


#### URL

URLは下記のようになる。

    http://example.localhost

 
#### httpd.conf

	# Virtual hosts
	Include /Applications/XAMPP/etc/extra/httpd-vhosts.conf  # コメントアウト


#### httpd-vhosts.conf

##### http://localhostで表示するホストの設定

    NameVirtualHost *:80

    <VirtualHost *:80>
    ServerName localhost
    DocumentRoot "/Applications/XAMPP/xamppfiles/htdocs"
    </VirtualHost>

##### バーチャルホストexample.localhostの設定

    <VirtualHost *:80>
        ServerAdmin webmaster@example.com
        DocumentRoot "/Applications/XAMPP/xamppfiles/docs/example"
        ServerName example.localhost
        ErrorLog "logs/example.localhost-error_log"
        CustomLog "logs/example.localhost-access_log" common
    </VirtualHost>
    
    <Directory "/Applications/XAMPP/xamppfiles/docs/example">
    AllowOverride All
    Require all granted
    </Directory>

__ Apache 2.4以降、アクセス制御を以下のように記載しても正常に動作しなくなった。__

	order deny,allow
	allow from ALL


#### hosts

	.....
	127.0.0.1 example.localhost
	.....


### <a name="port">ポート番号を使う方法(例はポート番号50000番)</a>

#### URL

	http://localhost:50000

#### httpd.conf

	# Virtual hosts
	Include /Applications/XAMPP/etc/extra/httpd-vhosts.conf  #コメントアウト

	Listen: 80
	Listen: 50000    # 追加

#### httpd-vhosts.conf

    NameVirtualHost *:50000

    <VirtualHost *:50000>
    ServerName localhost
    DocumentRoot "/Applications/XAMPP/xamppfiles/docs/example
    </VirtualHost>

#### hosts

	.....
	127.0.0.1 localhost:50000
	.....

### 参考

httpd.confの末尾に下記を追加しないとうまく動作しないこともあるようだが当環境では記述が無くてもうまくいった。

	AcceptMutex flock

&raquo; [\[Apache\] XAMPP for Mac でバーチャルホストの設定をする &laquo; きんくまデザイン](http://www.kuma-de.com/blog/2011-02-09/2535)
