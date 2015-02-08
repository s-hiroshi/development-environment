
# XAMPPと一緒にインストールされたMySQL/phpMyAdmin

MySQLはMacへデフォルトで入っていない。
今回はXAMPPのインストールと一緒にインストールされるMySQLについて記載する。

## mysqld

/Applications/XAMPP/xamppfiles/sbin/mysqld --skip-grant-tables --user=root

## mysqlコマンド

    /Applications/XAMPP/xamppfiles/bin/mysql
    
## rootへパスワードを設定

    set password for hoge@localhost = password('pass');
    
__ 下記コマンドでパスワード設定したときはエラーが発生してログインできなかった。__

    UPDATE mysql.user SET Password=PASSWORD('pass') WHERE User='root' AND Host='localhost';

## ユーザーの確認

   mysql> use mysql
   Database changed
   mysql> select user, host, password from user;
   +------+-----------+-------------------------------------------+
   | user | host      | password                                  |
   +------+-----------+-------------------------------------------+
   | root | localhost | *320947A24DF806709E4A32F86C9A97A8E56B3BF1 |
   | root | linux     |                                           |
   |      | localhost |                                           |
   |      | linux     |                                           |
   | pma  | localhost |                                           |
   +------+-----------+-------------------------------------------+
   5 rows in set (0.01 sec)

## プログラムフォルダー

    /Applications/XAMPP/xamppfiles/var/mysql

## my.cnf(MySQL設定ファイル)

    /Applications/XAMPP/xamppfiles/etc

## パスワードを忘れたときの対処

### パスワードなしでのログイン

下記設定のいづれかをでmysqlにパスワードなしでログインできる。

#### 1. my.cnfに設定

my.cnfの[mysqld]にskip-grant-tablesを追加

    [mysqld]
    skip-grant-tables

#### 2. mysqldコマンドを --skip-grant-tablesオプションを付けて実行

    /Applications/XAMPP/xamppfiles/sbin/mysqld --skip-grant-tables --user=root


### MySQLの環境を確認

    mysql> status;

### ユーザーの権限の確認

    SHOW GRANTS FOR username

### 権限を与える

rootはデフォルトでALL PRIVILEGES ON *.*

    grant all privileges on *.* to 'root'@'localhost' IDENTIFIED BY 'pass' WITH GRANT OPTION;



