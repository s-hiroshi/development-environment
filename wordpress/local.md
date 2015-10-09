# ローカル開発環境



| 種類 | プログラム | バージョン | 設定ファイル | IPアドレス | ポート番号 |
|-----|----------|----------|-----------|-----------|---------|
| Mac | /usr/sbin/httpd | Apache/2.2.26 (Unix) | /etc/apache2/httpd.conf | 192.168.12.88[^1] | 80 |
| VCCW | /usr/sbin/httpd[^2] | Apache/2.2.15 (Unix) | /etc/httpd/conf/httpd.conf | 192.168.33.10[^3] | 80[^4] |
| XAMPP | /Applications/MAMP/Library/bin/httpd | Apache 2.2.29 | /Applications/MAMP/conf/apache/httpd.conf | 192.168.12.88 | 8888 |
| PHPビルトインサーバー | | | | | |




[^1]:IPアドレス確認  
Mac(ホスト)のIPアドレスはシステム環境設定 > ネットワークまたはifconfigで確認する。  
VirtualBox(ゲスト)のIPアドレスゲストはipconfigで確認する。
VagrantのIPアドレスはVagrantへSSH接続後にifconfigで確認する。

[^2]:Vagrantへログインして確認。

[^3]:VirtualBoxのIPアドレスは192.168.33.1(ipconfigで確認)

[^4]:待ち受けポート番号    
httpd.confのLitenディレクティブで設定する。
VCCWは/etc/httpd/ports.confで設定する(port.confはhttpd.confから読み込む)


# Q&A

* 現在起動しているhttpdの確認
* ブラウザでhttpdの表示ページのhttpdを知る方法

