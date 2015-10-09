# ローカル開発環境

| 種類 | プログラム | バージョン | 設定ファイル | IPアドレス | ポート番号 |
|-----|----------|----------|-----------|-----------|---------|
| Mac | /usr/sbin/httpd | Apache/2.2.26 (Unix) | /etc/apache2/httpd.conf | 192.168.12.88[^1] | 80[^2] |
| VCCW | /usr/sbin/httpd[^3] | Apache/2.2.15 (Unix) | /etc/httpd/conf/httpd.conf, /etc/httpd/sites-enabled/wordpress.conf[^4], /etc/httpd/sites-available/wordpress.conf | 192.168.33.10 | 80 |
| XAMPP | /Applications/MAMP/Library/bin/httpd | Apache 2.2.29 | /Applications/MAMP/conf/apache/httpd.conf | 192.168.12.88 | 8888 |
| PHPビルトインサーバー[^5] | | | | | |


# HTTP ResponseヘッダーへWEBサーバーの情報を表示  
  ServerTokens OS
  ServerSignature On  
  (公開サーバーではサーバー情報は非表示にする)


[^1]:IPアドレス  
Mac(ホスト)のIPアドレスはシステム環境設定 > ネットワークまたはifconfigで確認する。  
VirtualBox(ゲスト)のIPアドレスはホストOSでipconfigで確認する。  
VCCWのホスト(vccw.dev)のIPアドレスはVagrantへSSH接続後にifconfigで確認する。  
(/path/to/vagrant/provision/default.ymlの値をもとにプロビジョニングされる。)

[^2]:待ち受けポート番号    
httpd.confのLitenディレクティブで設定する。  
VCCWは/etc/httpd/ports.confで設定する(port.confはhttpd.confから読み込む)

[^3]:Vagrantへログインして確認。  
($ which httpd)

[^4]: wordpr/etc/httpd/sites-enabled/wordpress.confはhttpd.confで読み込まれる。  
wordess.confは/etc/httpd/sites-available/wordpress.conf へのシンボリックリンク

[^5]: PHPビルドインサーバーは下記コマンドで起動する(ポート番号8080を使う例)  
$ php -S localhost:8080

