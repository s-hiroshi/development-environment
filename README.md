# 内容

AWSでWEBサービスを運用するために勉強している内容を書き留めた個人的なメモ書き。

# 目標

継続的インテグレーション可能な開発環境を構築しAWSで高品質なWEBサービスを提供する。

# 前提とする環境

* Ubuntu
* Nginx
* MySQL
* PHP

# 参考書

* [「CakePHPで学ぶ継続的インテグレーション」 渡辺 一宏, 吉羽 龍太郎, 岸田 健一郎, 穴澤 康裕, 丸山 弘詩  (編集)](http://www.amazon.co.jp/CakePHP%E3%81%A7%E5%AD%A6%E3%81%B6%E7%B6%99%E7%B6%9A%E7%9A%84%E3%82%A4%E3%83%B3%E3%83%86%E3%82%B0%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3-%E6%B8%A1%E8%BE%BA-%E4%B8%80%E5%AE%8F/dp/4844336789/ref=tmm_pap_title_0?ie=UTF8&qid=1421710653&sr=8-1)
* [「CakePHP2 実践入門」(WEB+DB PRESS plus) 安藤 祐介, 岸田 健一郎, 新原 雅司, 市川 快, 渡辺 一, 鈴木 則夫](http://www.amazon.co.jp/gp/product/4774153249/ref=pd_lpo_sbs_dp_ss_2?pf_rd_p=187205609&pf_rd_s=lpo-top-stripe&pf_rd_t=201&pf_rd_i=4839924317&pf_rd_m=AN1VRQENFRJN5&pf_rd_r=0Y02W25AP82Z83YPCYQR)
* [「Linuxサーバーセキュリティ徹底入門 ープンソースによるサーバー防衛の基本」中島 能和](http://www.amazon.co.jp/Linux%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E5%BE%B9%E5%BA%95%E5%85%A5%E9%96%80-%E3%83%BC%E3%83%97%E3%83%B3%E3%82%BD%E3%83%BC%E3%82%B9%E3%81%AB%E3%82%88%E3%82%8B%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E9%98%B2%E8%A1%9B%E3%81%AE%E5%9F%BA%E6%9C%AC-%E4%B8%AD%E5%B3%B6-%E8%83%BD%E5%92%8C/dp/4798132381/ref=tmm_jp_oversized_meta_binding_title_0?ie=UTF8&qid=1421728106&sr=1-1)

# 目次

* [パッケージ管理システム](#package)
* [HTML/CSS/JavaScript開発環境](#html_css_javascript)
* [Ubuntu+Nginx+MySQL+PHP開発環境](#ubuntu_nginx_mysql_php)
* [継続的インテグレーション](#ci)
* [AWS(Amazon Web Services)でWEBサービス運用](#aws)

# <a name="package">パッケージ管理システム</a>

## npm

node.jsパッケージ管理ツール。  
通常node.jsをインストールしたときに同時にインストールされる。

[npm Documentation](https://docs.npmjs.com/)

### node.js/npmインストール

下記サイトからpkgファイルをダウンロードしインストールした。

[node.js](http://nodejs.org/)


### バージョン確認
 
    $ npm —version
    1.3.25

### パス確認

    $ which npm
    /usr/local/bin/npm

### ヘルプ

    $ npm -h   // クイックヘルプ --helpはない
    $ npm -l   // display full usage info

### パッケージのインストール

    $ npm install パッケージ名       // カレントフォルダにインストール
    $ sudo npm install -g パッケージ名    // グローバルにインストール


### パッケージ一覧確認

    $ npm list       // カレントディレクトリにインストールしたパッケージ
    $ npm list -g    // グローバルにインストールしたパッケージ

### パッケージの更新

    $ npm update パッケージ名       // カレントフォルダにインストールしたパッケージの更新
    $ sudo npm update -g パッケージ名    // グローバルにインストールしたパッケージの更新

### npm自体の更新

    $ npm update  npm
    $ sudo npm update  npm -g



## gem

rubyパッケージ管理ツール。

### バージョン確認
 
    $ gem —version

### パス確認

    $ which gem
    /usr/bin/gem

### パッケージ一覧確認
 
    $ gem list --local # ローカルのパッケージ表示
    $ gem list --both # ローカル、リモートの両方とも表示。

デフォルトは —localが指定。

### gemヘルプ
 
    $ gem --help     // $ gem -hでもよい
    $ gem bar -h     // gem barのヘルプ
    $ gem list -h    // 例 gem listのヘルプ


### パッケージ更新

    $ sudo gem update <packagename>    // gemでインストールしたパッケージの更新

### gem自体の更新
 
    $ sudo gem update --system    // gem自体の更新
 

(例) sass, compassの更新

    $ sudo gem update sass
    $ sudo gem update compass


__gemはデフォルトでは/usr/binへインストールする？__


[そろそろ整理しておきたい、Gemコマンドの使い方 - ばくのエンジニア日誌](http://bakunyo.hatenablog.com/entry/2013/05/23/%E3%81%9D%E3%82%8D%E3%81%9D%E3%82%8D%E6%95%B4%E7%90%86%E3%81%97%E3%81%A6%E3%81%8A%E3%81%8D%E3%81%9F%E3%81%84%E3%80%81Gem%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%81%AE%E4%BD%BF%E3%81%84%E6%96%B9)


## wget

> wget とは、UNIXコマンドラインで HTTP や FTP 経由のファイル取得を行えるツールです。
Webサイトであれば、リンク先を階層で指定して一気に取得することができ、オフラインでじっくり読んだり、ミラーサイトを簡単に作ることが可能です。
また、ダウンロードが途中で止まってしまった場合は、途中からやり直すレジューム機能があり便利です。
>
> wget はGNUプロジェクトで作られているフリーソフトです。改良も配布も自由なので、是非活用しましょう。

[wget の使い方](http://tech.bayashi.net/svr/doc/wget.html)


## cURL

> cURL（カール[2]）は、さまざまなプロトコルを用いてデータを転送するライブラリとコマンドラインツールを提供するプロジェクトである。cURLプロジェクトは libcurl と cURL の2つの成果を生んでいる。

Wikipedia


## apt

Debian系のパッケージ管理システム。


## apt-get

aptを利用したパッケージ管理コマンド
追加したリポジトリの一覧表示方法は?


## rpm

> 米Red Hat社が開発したパッケージ管理システム。パッケージのインストールやアンインストールを行う。インストールの際は依存関係のあるパッケージ(動作に必要な他のパッケージ）を調べ，もしシステムにそれらパッケージが存在しない場合は，その旨をユーザーに通知する。また，アンインストールする際に，そのパッケージが他パッケージと依存関係がある(他のパッケージの動作に必要とされる)場合には，アンインストールできない旨を通告する。さらに，既にシステムにインストール済みのパッケージをチェックすることも可能。パッケージのインストール，アンインストール，アップデートには管理者権限が必要。
 
[Linuxコマンド集 - 【 rpm 】 RPMパッケージをインストール/アンインストールする：ITpro](http://itpro.nikkeibp.co.jp/article/COLUMN/20060227/230875/)


## yum

> Yellowdog Updater Modified (Yum ヤム)はLinuxのRPM Package Managerのパッケージを管理するメタパッケージ管理システムである。Yumは デューク大学のLinux@DUKEプロジェクトでセス・ヴィダル（英語版）を始めとするボランティアによって開発された。

Wikipedia

yumはパッケージの置き場であるレポジトリを登録する必要がある。


## Bundler

rubyの依存関係解決の標準パッケージ管理ツール。

## Homebrew — OS X用パッケージマネージャー

Homebrewはパッケージを/usr/local/binへインストールする。

### Homebrewのインストール

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

### バージョン確認

    $ brew --version
    0.9.5

### パス確認

    $ which brew
    /usr/local/bin/brew

### パッケージのインストール

    $ brew install パッケージ名  // パッケージのインストール

###  パッケージ一覧確認

     $ brew list        // インストールしたパッケージ
 
    ant libyaml readline sqlite wget
    ios-sim openssl ruby subversion

### パッケージの更新

    $ brew upgrade     // インストールしたパッケージを更新

### Homebrew自体の更新

    $ brew update      // Homebrew自体を更新
 



# <a name="html_css_javascript">HTML/CSS/JavaScript開発環境</a>

## 開発ツール

* CSS プリプロセッサー
    + Sass/Compass
* JavaScriptテストツール
    + QUnit + PhantomJS
* ドキュメンテーションツール
    + CSS  
      StyleDocco / SassDoc
    + JavaScript  
      YUI Doc
* コードインスペクション
    + JavaScript  
      JSLint, JSHint
* 自動化ツール Grunt


## 継続的開発

各ツールをGruntで自動化する。


## フォルダ構成の例

以下の説明は下記フォルダ構成を前提とする。

    example
    |
    |— index.html                // 公開サイトへデプロイ
    |
    |— css
         |— style.min.css        // 公開サイトへデプロイ
    |
    |—  js
         |— scripts.min.js       // 公開サイトへデプロイ
    |
    |— dev
         |— css
              |— doc            // CSS スタイルガイドフォルダ StyleDocco/SassDoc
              |— src
                   |— style.css // 圧縮してexample/css/style.mini.cssへ
         |— sass
              |— style.scss     // dev/css/src/style.cssへコンパイル
         |—  js
              |— scripts.js
              |— doc            // ドキュメンテーションフォルダ  YUI DOC
              |— test           //  JavaScriptのテスト  QUnit
                   |— qunit-1.16.0.css
                   |— qunit-1.16.0.js
                   |— scripts-test.html
                   |— scripts-test.js
         |
         |— node_modules         // Grunt本体・パッケージのインストールフォルダ
         |— package.json         // Gruntパッケージファイル
         |— Gruntfile.js         // Grunt設定ファイル
         |— contrib.rb           // Compass設定フィアル


## Grunt

[Grunt: The JavaScript Task Runner](http://gruntjs.com/)

gruntは一般的にgrunt-cliのみグローバルへインストールし、grunt本体も含めて各プラグインはプロジェクトごとのフォルダへインストールする。

### 1. node/npmのインストール

[node.js](http://nodejs.org/)

Gruntはnode.js/npmを使う。

node.jsはpkgファイルをダウンロードしインストールした。  
npmはnode.jsと一緒にインストールされた。

### 2. grunt command line interface(grunt-cli)のグローバルへのインストール

    $ sudo npm install -g grunt-cli

### 3. プロジェクトフォルダにpackage.jsonを作成

    {
      "name": "example",
      "version": "0.0.1"
    }

### 4. プロジェクトフォルダにGrunt本体とプラグインをインストール

grunt本体とプラグインはプロジェクトごとにプロジェクトフォルダへインストールする。

#### 4-1. Grunt本体のインストール

    $ cd <path>
    $ npm install grunt --save-dev

\<path\>は上記フォルダ構成の例でははdevのパス。  
–save-devをつければpackage.jsonのdevDependenciesプラグイン情報が追記される。

    {
      "name": "example",
      "version": "0.0.1",
      "devDependencies": {
      "grunt": "~0.4.1"
      }
    }

#### 4-2. プラグインインストール例

    $ npm install grunt-contrib-compass --save-dev
    $ npm install grunt-contrib-cssmin --save-dev
    $ npm install grunt-contrib-qunit --save-dev
    $ npm install grunt-contrib-uglify --save-dev
    $ npm install grunt-contrib-watch --save-dev
    $ npm install grunt-contrib-yuidoc --save-dev
    $ npm install grunt-styledocco --save-dev


    {
      "name": "example-project",
      "version": "0.0.1",
      "devDependencies": {
        "grunt": "^0.4.5",
        "grunt-contrib-compass": "^0.4.1",
        "grunt-contrib-cssmin": "^0.11.0",
        "grunt-contrib-qunit": "^0.5.2",
        "grunt-contrib-uglify": "^0.2.7",
        "grunt-contrib-watch": "^0.5.3",
        "grunt-contrib-yuidoc": "^0.5.2",
        "grunt-styledocco": "^0.2.1"
      }
    }

### 5. Gruntfile.jsの例

    module.exports = function(grunt) {
      grunt.initConfig({
        watch: {
          live: {
            files: [
              'sass/*.scss',
              'js/src/*.js'
            ],
            tasks: ['compass', 'cssmin', 'uglify' ,'styledocco', 'yuidoc']
          },
          qunit: {
            files: ['js/src/*.js'],
            tasks: ['qunit', 'yuidoc']
          }
        },
        compass: {
          dist: {
            options: {
              config: 'config.rb'
            }
          }
        },
        qunit: {
          all: ["js/test/*.html"]
        },
        cssmin: {
          compress: {
            files: {
              '../../css/style.min.css': [ 'css/src/style.css' ]
            }
          }
        },
        uglify: {
          my_target: {
            files: [{
              expand: true,
              cwd: 'js/src',
              src: '*.js',
              dest: '../../js/',
              ext: '.min.js'

            }]
          }
        },
        styledocco: {
          dist: {
            options: {
              name: 'CSS CI'
            },
            files: {
              'css/doc': 'css/src'
            }
          }
        },
        yuidoc: {
          compile: {
            name: 'JavaSrcript CI',
            description: 'JavaScriptのCIテスト',
            version: '0.0.1',
            url: 'http://wwww.example.com',
            options: {
              paths: 'js/src',
              outdir: 'js/doc/'
            }
          }
        }

      });

      grunt.loadNpmTasks( 'grunt-contrib-watch' );
      grunt.loadNpmTasks( 'grunt-contrib-compass' );
      grunt.loadNpmTasks( 'grunt-contrib-qunit' );
      grunt.loadNpmTasks( 'grunt-contrib-cssmin' );
      grunt.loadNpmTasks( 'grunt-contrib-uglify' );
      grunt.loadNpmTasks( 'grunt-styledocco' );
      grunt.loadNpmTasks( 'grunt-contrib-yuidoc' );
      grunt.registerTask( 'default', [ 'watch'] );
    };


### 6. Grunt実行

    // 上記例ではgrunt.registerTask( 'default', [ 'watch'] );なのでwatchを実行
    // watchにタスクとして他の処理を指定しているのでそれらも順番に実行
    $ grunt
    // 特定のタスクのみ実行
    $ grunt <taskname>

### package.jsonをもとにしたプラグインのインストール

    $ npm install

既存のnode_modulesフォルダがあれば削除しておく。

### プラグインのバージョンアップ

    $ npm update —save-dev

すべてのプラグインがアップデートされる。


## Sass

CSSプリプロセッサー。

[Sass: Syntactically Awesome Style Sheets](http://sass-lang.com/)

### Sassインストール

    $ sudo gem install sass

### コンパイル

style.scssをコンパイルして同じフォルダにstyle.cssを作成する例。

    $ sass style.scss style.css

### 変更を監視して自動コンパイル

    $ sass –watch style.scss:style.css

ターミナルではctrl + Cでwatchを停止する。

### バージョン確認

    $ sass —version
    3.4.9

### パス確認

    $ which sass
    /usr/bin/sass


## Compass

Sassを使ったCSS作成フレームワーク。

[Compass Home | Compass Documentation](http://compass-style.org/)
[Sass/Compass のインストールと基本的な環境設定 | Web Design Leaves](http://www.webdesignleaves.com/wp/htmlcss/652/)

### インストール

    $ sudo gem install compass
 
### バージョン確認

    $ compass —version
    1.0.1

### パス確認

    $ which compass
    /usr/bin/compass

### コンパス初期化

    $ create compass --bare

    contrib.rbとsassフォルダが作成される。

### config.rb(Compass設定ファイル)

    |—css
    |   |— a.css
    |
    |—dev
    |  |— sass
    |  |    |— a.scss
    |  |
    |  |— contrib.rb
    |
    |—index.html

contrib.rbの設定例

    // 設定
    css_dir = "../css"
    sass_dir = “sass”
    // compass専用コメントの出力を停止するとき(config.rb)
    line_comments = false

上記例ではcompassコマンドでコンパイルするとdev/sassフォルダのモジュールファイルを除いたscssファイルをコンパイルしcssディレクトリへ出力する。

### 変更を監視して自動コンパイル

    $ compass watch css/sass/main.scss


## StyoeDocco

CSS スタイルガイド。

### インストール

    $ sudo npm install -fg styledocco

オプションfは必須。

### インストール先

    /usr/local/lib/node_modules/styledocco/bin/styledocco

### パスの確認

    $ which styledocco
    /usr/local/bin/styledocco

### スタイルガイド作成

    $ cd mytheme
    $ styledocco -n "My Theme" -o docs/css style.css

### Grunt + StyleDocco

[grunt-styledocco](https://www.npmjs.com/package/grunt-styledocco)

### Gruntfile.js

    styledocco: {
      dist: {
        options: {
          name: 'ci+styleguide'
        },
        files: {
          'docs': 'css'
        }
      }
    }

    grunt.loadNpmTasks( 'grunt-styledocco' );


## Code Inspections(検査)

 * jsLint  PhpStormはデフォルトでサポート。
 * jsHint  PhpStormはデフォルトでサポート
    jsLintより緩い


## QUnit テストツール

[QUnit](qunitjs.com)

### Grunt + Qunit(+PhantomJS)

[gruntjs/grunt-contrib-qunit](https://github.com/gruntjs/grunt-contrib-qunit)

grunt-contrib-qunitはPhantomJSを含む。

### インストール

    $ npm install grunt-contrib-qunit —save-dev

Qunitファイル(js/css)をCDNから読み込むと正常にテストできなかった。
ダウンロードして配置した。


## YUI Doc - JavaScript キュメンテーションツール

[YUIDoc – Javascript Documentation Tool](http://yui.github.io/yuidoc/)
[YUIDoc Syntax Reference](http://yui.github.io/yuidoc/syntax/index.html)

### インストール

    npm -g install yuidocjs.

### コマンド

    yuidoc

### Grunt + YUI DOC

    yuidoc: {
      compile: {
        name: '<%= pkg.name %>',
        description: '<%= pkg.description %>',
        version: '<%= pkg.version %>',
        url: '<%= pkg.homepage %>',
        options: {
          paths: 'path/to/source/code/',
          themedir: 'path/to/custom/theme/',
          outdir: 'where/to/save/docs/'
        }
      }
    }

[目次 HTML/CSS/JavaScript開発環境の目次へ戻る](#html_css_javascript)




# <a name="ubuntu_nginx_mysql_php">Ubuntu+Nginx+MySQL+PHP開発環境</a>

AWS上にUbuntu + Nginx + MySQL + PHPの開発環境を構築することを目指す。

## 目次

* [Ubuntu](#ubuntu)
* [シェル](#shell)
* [Vi](#vi)
* [Nginx, MySQL, PHPインストール](#install_nginx_mysql_php)
* [Nginx](#nginx)
* [PHP](#php)
    + [PHP実行環境](#php_exe)
    + [php.ini保存場所](#php_ini)
    + [文字コード](#php_character)
    + [実行環境の例](#php_exe_sample)
    + [実行ユーザーの確認](#php_user)
    + [PECL](#php_pecl)
    + [Composer](#php_composer)
    + [単体テスト - PHPUnit](#php_test)
    + [デバッグ - Xdebug](#php_ci_debug)
    + [ドキュメンテーション - phpDocumentor](#php_documentation)
    + [コードインスペクション - PHP_CodeSniffer](#php_inspection)
    + [CakePHPのインストール](#php_cakephp)
* [MySQL](#mysql)


## 構築環境

* Ubuntu 14.04 
* Nginx  
  nginx version: nginx/1.4.6 (Ubuntu)
* MySQL  
* PHP5   
  PHP Version 5.5.9-1ubuntu4.5


## <a name="ubuntu">Ubuntu</a>

### Ubuntuログインユーザー

ユーザーubuntuでログインしていると仮定して記載する。


### ログ

    /var/log



## <a name="shell">シェル</a>

### 日本語環境インストール

    $ sudo apt-get install language-pack-ja
    $ sudo update-locale LANG=ja_JP.UTF-8

一時てきに日本語環境へ切り替える。

    // 現在の設定を確認
    $ export $LANG
    en_US.UTF-8
    // 日本語環境に設定
    $ export LANG=ja_JP.UTF-8



## <a name="vi">Vi</a>

### 文字コード確認

    // 開いているファイルの文字コード確認
    :set enc?


### 文字コード設定

    // UTF-8へ変更
    :set encoding=utf8



## <a name="install_nginx_mysql_php">Nginx + MySQL + PHPインストール</a>

Nginx, MySQL, PHP5の環境を構築するのに必要なパッケージをインストールする。

    // apt-getを最新へ更新
    $ sudo apt-get update
    // 必要パッケージインストール
    $ sudo apt-get install php5 php5-cli php5-fpm php5-mysql php-pear php5-curl php5-dev php-apc php5-xsl php5-mcrypt mysql-server-5.5 nginx



## ドキュメントルート作成

    $ sudo mkdir -p /var/www/application/current/app/webroot

## PHP実行ユーザー

* ユーザー  
  www-data
* www-dataが属するグループ  
  www-data

### PHP実行ユーザー確認

    <?php
    echo `whoami`;

以下PHP実行ユーザーはwww-dataと仮定する。


## currentディレクトリ以下の所有者とパーミション変更
 
    $ cd /var/www/application
    $ sudo chown -R www-data current
    $ sudo chmod -R 755 current



## <a name="env_nginx">Nginx</a>

### プログラム

    $ which nginx
    /usr/sbin/nginx


### 起動・停止・再起動

[ubuntuでnginxの起動と最低限のコマンド](http://joppot.info/2014/03/10/970)


### ログ

nginx.confで設定する。

    ##
    # Logging Settings
    ##

    access_log /var/log/sginx/access.log;
    error_log /var/log/nginx/error.log;

[nginxのログ出力変更 - Qiita](http://qiitj.com/hito3/items/0e539e82ee3c410cccf1u)


### 設定関連ファイル

    (1) /etc/nginx/nginx.conf  // Nginx全体の設定(今回は変更なし)
    (2) /etc/nginx/sites-available/default  // ホストの設定(変更)
    (3) /etc/nginx/sites-enabled/default   //  (2)のシンボリックリンク Nginxの起動時に読み込まれる

今回は/etc/nginx/sites-available/defaultを編集する。


### site-available/defaultファイル

* ドキュメントルート設定 rootディレクティブ
* /によるアクセス        indexディレクトリ
* FastCGIの設定          locationディレクティブ

__初期状態ではlocationがコメントアウトされておりphpファイルへアクセスするとダウンロードしてしまう。下記のようにコメントアウトを外す。__

    server {
            listen 80 default_server;
            listen [::]:80 default_server ipv6only=on;
 
            root /var/www/application/current/app/webroot;
            index index.php index.html index.htm;
 
            # Make site accessible from http://localhost/
            server_name localhost;
 
            location / {
                    # First attempt to serve request as file, then
                    # as directory, then fall back to displaying a 404.
                    try_files $uri $uri/ =404;
                    # Uncomment to enable naxsi on this location
                    # include /etc/nginx/naxsi.rules
            }
 
            # Only for nginx-naxsi used with nginx-naxsi-ui : process denied requests
            #location /RequestDenied {
            #       proxy_pass http://127.0.0.1:8080; 
            #}
 
            #error_page 404 /404.html;
 
            # redirect server error pages to the static page /50x.html
            #
            #error_page 500 502 503 504 /50x.html;
            #location = /50x.html {
            #       root /usr/share/nginx/html;
            #}
 
            # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
            #
            location ~ \.php$ {
                    fastcgi_split_path_info ^(.+\.php)(/.+)$;
            #       # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
            #
            #       # With php5-cgi alone:
                    # fastcgi_pass 127.0.0.1:9000;
                    # With php5-fpm:
                    fastcgi_pass unix:/var/run/php5-fpm.sock;
                    fastcgi_index index.php;
                    include fastcgi_params;
            }
 
            # deny access to .htaccess files, if Apache's document root
            # concurs with nginx's one
            #
            #location ~ /\.ht {
            #       deny all;
            #}
    }


### Nginx(再)起動

    $ sudo nginx -s reload



[軽量で高速なウェブサーバNginxを、Ubuntu 12.04に導入する(設定編その１) | 近藤嘉雪のプログラミング工房日誌](http://blog.kondoyoshiyuki.com/2012/12/09/setting-1-nginx-on-ubuntu-12-04/)



## <a name="php">PHPの開発環境</a>

### <a name="php_exe">PHP実行環境の分類</a>

* Apache + モジュール/CGI
    - モジュール  
      Apache + php_mod: Apacheのモジュールとして動かす。
    - CGI  
      Apach + php-cgi: ApacheからCGIとして呼び出す。
* Nginx + php-fpm  
  [PHP: FastCGI Process Manager (FPM) - Manual](http://php.net/manual/ja/install.fpm.php)
  NginxのCGIからCGIとして呼び出す。
* CLI(Command Line Interface)  
  コマンドで実行する



### <a name="php_ini">php.ini保存場所</a>

* ブラウザ  
  phpinfo関数を実行する。  
  項目「Configuration File (php.ini) ,Path/Loaded Configuration File」を確認する。
* CLI  
  $ php -r 'phpinfo();' | grep php.ini


### <a name="php_character">文字コード</a>

UTF8で運用するためphp.iniファイルの文字関連を編集する。

    default_charset = "UTF-8"                            // コメントアウトをはずす

    [mbstring]
    ; language for internal character representation.
    ; http://php.net/mbstring.language
    mbstring.language = Japanese

    ; internal/script encoding.
    ; Some encoding cannot work as internal encoding.
    ; (e.g. SJIS, BIG5, ISO-2022-*)
    ; http://php.net/mbstring.internal-encoding
    mbstring.internal_encoding = UTF-8

    ; http input encoding.
    ; http://php.net/mbstring.http-input
    mbstring.http_input = psss

    ; http output encoding. mb_output_handler must be
    ; registered as output buffer to function
    ; http://php.net/mbstring.http-output
    mbstring.http_output = pass

    ; enable automatic encoding translation according to
    ; mbstring.internal_encoding setting. Input chars are
    ; converted to internal encoding by setting this to On.
    ; Note: Do _not_ use automatic encoding translation for
    ;       portable libs/applications.
    ; http://php.net/mbstring.encoding-translation
    mbstring.encoding_translation = Off

    ; automatic encoding detection order.
    ; auto means
    ; http://php.net/mbstring.detect-order
    mbstring.detect_order = SJIS,EUC-JP,JIS,UTF-8,ASCII
    ; substitute_character used when character cannot be converted
    ; one from another
    ; http://php.net/mbstring.substitute-character
    mbstring.substitute_character = none


default_charsetにUTF-8を設定すればPHPから出力するとき下記コードが自動で出力される。

    header('Content-Type: text/html; charset:UTF-8');

上記をしていしているときも出力されるHTMLコードにはmetaタグで文字コードを指定するべき。

    <meta charset="utf-8">


### <a name="php_exe_sample">実行環境の例(phpinfo関数をブラウザで実行)</a>

| PHPh| PHP Version 5.5.9-1ubuntu4.5 |
|-----|-----|
|Configuration File (php.ini) Path|/etc/php5/fpm|
|Loaded Configuration File|/etc/php5/fpm/php.ini|
|Scan this dir for additional .ini files|/etc/php5/fpm/conf.d|
|Additional .ini files parsed|/etc/php5/fpm/conf.d/05-opcache.ini, /etc/php5/fpm/conf.d/10-pdo.ini, /etc/php5/fpm/conf.d/20-curl.ini, /etc/php5/fpm/conf.d/20-json.ini, /etc/php5/fpm/conf.d/20-mcrypt.ini, /etc/php5/fpm/conf.d/20-mysql.ini, /etc/php5/fpm/conf.d/20-mysqli.ini, /etc/php5/fpm/conf.d/20-pdo_mysql.ini, /etc/php5/fpm/conf.d/20-readline.ini, /etc/php5/fpm/conf.d/20-xdebug.ini, /etc/php5/fpm/conf.d/20-xsl.ini|
|PHP API|20121113|
|PHP Extension|20121212|
|Zend Extension|220121212|


### <a name="php_pecl">PECL :: The PHP Extension Community Library</a>

[PECL :: The PHP Extension Community Library](http://pecl.php.net/)


#### PECLの例

* [PECL :: Package :: imagick](http://pecl.php.net/package/imagick)
* [PECL :: Package :: oauth](http://pecl.php.net/package/oauth)


#### PECLライブラリのインストール

> PECLのインストール用には、PEAR同様に「pecl」コマンドが提供されている。インストール方法もほぼPEARと同じだが、インストール後に設定ファイル（php.ini）の「extension」でインストールしたモジュールを指定する必要がある点が異なる。
Wikipedia


1. peclコマンド[^pecl][^phpize][^phpbuilddir]  
  $ pecl install \<package name\>
2. php.iniでextensionでモジュールを設定(後述のAdditional .iniを参照)  

[^pecl]:peclコマンドは一般的にPHPをインストールすればpear コマンドなどと一緒にインストールされる。

[^phpize]:pecl installはソースのコンパイルでphpizeを利用する。phpizeがインストールされていない場合は次節を参考にインストールする。

[^phpbuilddir]:ビルドのワーキングディレクトリ /build/buildd/php5-5.5.9+dfsg/pear-build-download


#### phpize

> 拡張モジュールをビルドする低レベルなビルドツール。autoconfやautomake m4等のビルドツールが別途必要になる。これを使用することにより、PHPをソースから再コンパイルすることなく拡張モジュールをビルドすることができる

[phpizeとは - PHP用語 Weblio辞書](http://www.weblio.jp/content/phpize)

pecl installはソースのコンパイルでphpizeを利用する。
phpizeはDebian系のUbuntuではphp5-dev[^php-devel]に含まれている。
php5-devをインストールする。

    $ apt-get install php5-dev

[^php-devel]:RPM系はphp-develをyumでインストールする。


#### Additional .ini

通常、PECLでインストールしたライブラリはphp.iniのextension_dirで指定したフォルダへ配置しphp.iniへextension=*.soと記載して読み込む。
しかし今回構築した環境では下記のような仕組みにになっていた。


##### ライブラリ配置ディレクトリ

    /usr/lib/php5/20121212   <- どのファイルでこのディレクトリがPECLライブラリ保存先として指定しているかは不明

    curl.so  mcrypt.so  mysql.so    pdo_mysql.so  readline.so  xsl.so
    json.so  mysqli.so  opcache.so  pdo.so


##### extension=\<library name\>

php.iniに追記せず/etc/php5/fpm/conf.dディレクトリに各ライブラリごとのiniファイルがありextension=\<library name\>の記載がされている。

    vagrant@develop:/etc/php5/fpm/conf.d$ ls
    05-opcache.ini  20-curl.ini  20-mcrypt.ini  20-mysql.ini      20-readline.ini  20-xsl.ini
    10-pdo.ini      20-json.ini  20-mysqli.ini  20-pdo_mysql.ini  20-xdebug.ini

[プログラミング日誌 :: LinuxのPHPに拡張モジュールを入れる方法](http://nb-tech.doorblog.jp/archives/51670170.html)

#### extensionとzend_extension

[PHP extensionとZend extensionの違い - hnwの日記](http://d.hatena.ne.jp/hnw/20130715)


### ログの設定

ログをファイルへ保存する設定をphp.iniへ記載。

    display_errors = Off

    log_errors = On
    error_log = <path>


## <a name="php_composer">Composer</a>

PHPパッケージ管理ツール。


#### パッケージのインストール

    $ cd /var/www/application/current/app
    $ php composer install

#### Composerのインストール

    $ curl -sS https://getcomposer.org/installer | php

composer.pharがカレントディレクトリにダウンロードされる。
ダウンロードしたcomposer.pharは通常composerへ名前を変更しパスの通っているディレクトリへ移動する(例 /usr/local/bin)。

パスが通っていればコマンドは下記のように実行できる。

    $ composer --version

パスが通っていないときはcomposer(名前を変更していないときはcomposer.phar)コマンドは下記のように実行する。

    $ php /fullpath/composer --version


#### Composerの初期化

    $ composer init

composer.jsonファイルが作成される。


#### composer.jsonの例

    {
        "require": {
            "monolog/monolog": "1.0.*"
        }
    }

[Composer ドキュメント日本語訳](http://kohkimakimoto.github.io/getcompo:ser.org_doc_jp/doc/01-basic-usage.html)

ディレクトリにvendor[^conposer-cake]ディレクトリを作成しそこへ配置して管理する。

[^conposer-cake]: デフォルトはConposerはインストールしたパッケージをvendorディレクトリに配置する。しかしCakePHPではVendorディレクトリを利用するためvendorから
Vendorへ変更する。変更はcomposer.jsonのconfig.vendor-dirプロパティで設定する。


#### composer.jsonに記載されたパッケージのインストール

    $ composer install


1. パッケージをインストール
2. composer.lockファイルを作成しバージョンを固定する。


#### パッケージの追加
  
    // 通常の追加
    $ composer require <package>
    // 開発でのみ必要なパッケージの追加
    $ composer require --dev <package>
 
composer requireはパッケージを追加しcomposer.jsonへ追記する。
composer require --devは開発環境でのみ必要なパッケージを追加するときに使う。  
composer installではインストールされず--devオプションを付けてcomposer install --devでインストールされる。


#### パッケージのインストール場所

通常、composer.jsonと同階層にvendorディレクトリが作成され配置される。

上記composer.jsonの例では

    vendor/monolog/monolog

    |-- composer.json
    |
    |-- composer.lock
    |
    |-- vendor
          |
          |--monolog
              |
              |--monolog


composer.jsonで変更できる。

    "config": {
        "vendor-dir": "some",
      },


#### Composerの利点

1. 利点  
  ComposerでインストールしたパッケージはVendorディレクトリへ配置し依存関係をを管理してくれる。
2. オートロード[^composer_cake_autoload]

[^composer_cake_autoload]: 例えばCakePHPではConfig/bootstrap.phpへオートロード
を追加すればよい。require_once dirname(dirname(__FILE__)) . DS . 'Vendor' . DS . 'autoload.php';


### <a name="php_test">単体テストツール - PHPUnit</a>

[PHPUnit #x2013; The PHP Testing Framework](https://phpunit.de/)

PHPUnitのインストールの方法は幾つかある。

#### phpunit.pharをダウンロード

> ➜ wget https://phar.phpunit.de/phpunit.phar
>
>➜ chmod +x phpunit.phar
>
>➜ sudo mv phpunit.phar /usr/local/bin/phpunit
>
>➜ phpunit --version

#### Composerを使いインストール


PHPUnitの最新版(2015.01.16)は4.4だがCakePHPで利用することを考えめ3.7をインストール

    // PHPUnitをインストール
    $ composer require "phpunit/phpunit":"3.7.*"


#### phpunitコマンド

    vendor/bin/phpunit

### 簡単なテスト

    var/www/application/current/app
        |
        |…..
        |— vendor
                  |…..
        |— test
                  |— CalcTest.php
        |
        |— webroot
                  |— Calc.php


CalcTest.php

    <?php
    require_once(“/var/www/application/current/app/webroot/Calc.php”);

    class CalcTest extends PHPUnit_Framework_TestCase {
        private $calc;

        protected function setUp() {
            $this->calc = new Calc(10);
        }

        public function testAdd() {
            $this->assertEquals(15, $this->calc->add(5));
        }
    }

Calc.php

    <?php
    class Calc {
        private $value;

        public function __construct($value = 0) {
            $this->value = $value;
        }

        public function add($value) {
            return $this->value + $value;
        }
    }


### テスト実行

    $ vendor/bin/phpunit test/CalcTest


### --bootstrapオプション

    working_dir
      |
      |-- Calc.php
      |
      |-- Test
            |-- CalcTest.php
            |
            |-- bootstrap.php
      |
      |-- vendor
            |-- bin
                  |-- phpunit

#### bootstrap.php

    <?php
    require_once('Calc.php');  // workingディレクトリからの相対パス

#### 実行

    $ vendor/bin/phpunit --bootstrap Test/bootstrap.php CalcTest


### <a name="php_debug">デバッグ- Xdebug</a>

* [Xdebug - Debugger and Profiler Tool for PHP](http://xdebug.org/)


#### インストール

    $ sudo apt-get install php5-xdebug

/usr/lib/php5/20121212/xdebug.soへインストールされた。

#### 設定(/etc/php5/fpm/conf.d/20-xdebug.ini)

/etc/php5/fpm/conf.d/20-xdebug.iniへ読込み処理を記載した。

    zend_extension=/usr/lib/php5/20121212/xdebug.so


#### 再起動

    sudo service php5-fpm restart


### <a name="php_phpdocumentation">ドキュメンテーションツール - phpDocumentor</a>

[phpDocumentor](http://www.phpdoc.org/)

#### Composerを使いインストール

    $ composer require "phpdocumentor/phpdocumentor:2.*"

[Installing Using Composer](http://www.phpdoc.org/docs/latest/getting-started/installing.html#using-composer)



### <a name="php_inspection">Code Inspections(検査) - PHP_CodeSniffer</a>

#### インストール

今回はComposerを使いインストールした。

    $ composer require "squizlabs/php_codesniffer":"*"

[squizlabs/PHP_CodeSniffer · GitHub](https://github.com/squizlabs/PHP_CodeSniffer)
[squizlabs/php_codesniffer - Packagist](https://packagist.org/packages/squizlabs/php_codesniffer)

#### CakePHPの規約追加

[cakephp/cakephp-codesniffer](cakephp/CakePHP_CodeSniffer)

    $ composer require "cakephp/cakephp-codesniffer=2.*"
    $ vendor/bin/phpcs --config-set installed_paths vendor/cakephp/cakephp-codesniffer


## <a name="php_cakephp">CakePHP</a>

### インストール

Composerを使いインストールする。

    {
        "name": "sample",
        "authors": [
          {
            "name": "Name",
            "email": "Email Adress"
          }
        ],
        "require": {
          "cakephp/cakephp": "2.6.*",
          "ext-mcrypt": "*"

        },
        "config": {
            "vendor-dir": "Vendor/"
        },
        
    }


### 拡張モジュールmcryptエラー

[Mcrypt extension is missing in 14.04 server for mysql - Ask Ubuntu](http://askubuntu.com/questions/460837/mcrypt-extension-is-missing-in-14-04-server-for-mysql)

mcrsyptはapt-getでインストール済み。

    // mcryptをインストール
    $ sudo apt-get php5-mcrypt

しかしインストールで下記のエラーが発生した。

    Your requirements could not be resolved to an installable set of packages.

      Problem 1
        - The requested PHP extension ext-mcrypt * is missing from your system.
      Problem 2
        - cakephp/cakephp 2.6.1 requires ext-mcrypt * -> the requested PHP extension mcrypt is missing from your system.
        - cakephp/cakephp 2.6.0 requires ext-mcrypt * -> the requested PHP extension mcrypt is missing from your system.
        - Installation request for cakephp/cakephp 2.6.* -> satisfiable by cakephp/cakephp[2.6.0, 2.6.1].


/etc/php5/fpm/conf.dおよび/etc/php5/cli/conf.dにはcrypt用のiniファイルは存在しなかった。  
/etc/php5/fpm/conf.dの中身を確認(/etc/php/cli/conf.dも同様)。

    $ ls /etc/php5/fpm/conf.d
    05-opcache.ini  20-apcu.ini  20-json.ini    20-mysql.ini      20-readline.ini
    10-pdo.ini      20-curl.ini  20-mysqli.ini  20-pdo_mysql.ini  20-xsl.ini

/etc/php5/fpm/conf.dおよび/etc/php5/cli/conf.dのファイルは/etc/php5/mods-availableの各ファイルへのシンボリックファイルだった


    $ ls /etc/php5/mods-available
    apcu.ini  json.ini    mysqli.ini  opcache.ini  pdo_mysql.ini  xsl.ini
    curl.ini  mcrypt.ini  mysql.ini   pdo.ini      readline.ini

  

mcryptのシンボリックファイルを作成した。

    $ ln -s /etc/php5/mods-available/mcrypt.ini /etc/php5/cli/conf.d/20-mcrypt.ini
    $ ln -s /etc/php5/mods-available/mcrypt.ini /etc/php5/fpm/conf.d/20-mcrypt.ini

Nginx再起動

    $ sudo nginx -s reload


### プロジェクト作成

    ubuntu@xxx:/var/www/application/current/app$ Vendor/bin/cake bake project
    
    Welcome to CakePHP v2.6.1 Console
    ---------------------------------------------------------------
    App : app
    Path: /var/www/application/current/app/
    ---------------------------------------------------------------
    What is the path to the project you want to bake?  
    [/var/www/application/current/app/myapp] > /var/www/application/current/app
    Skel Directory: /var/www/application/current/app/Vendor/cakephp/cakephp/lib/Cake/Console/Templates/skel
    Will be copied to: /var/www/application/current/app
    ---------------------------------------------------------------
    Look okay? (y/n/q) 
    .....
    .....



## <a name="mysql">MySQL</a>

### 起動・停止・再起動

    // 起動
    $ sudo /etc/init.d/mysql start
     
    // 停止
    $ sudo /etc/init.d/mysql stop
     
    // 再起動
    $ sudo /etc/init.d/mysql restart
 

### 設定ファイル

    $ sudo find / -name my.cnf
    /etc/mysql/my.cnf


### MySQLの文字コード関連確認

    mysql> show variables like 'character_set_%';

| Variable_name            | Value                                      |
---------------------------|--------------------------------------------|
| character_set_client     | utf8                                       |
| character_set_connection | utf8                                       |
| character_set_database   | latin1                                     |
| character_set_filesystem | binary                                     |
| character_set_results    | utf8                                       |
| character_set_server     | latin1                                     |
| character_set_system     | utf8                                       |


### charcter_set_serverの文字コードをUTF8へ設定

    $ sudo vi /etc/mysql/my.cnf

    [mysqld]
    skip-character-set-client-handshake
    character-set-server = utf8
    collation-server = utf8_general_ci


### UTF8でデータベース作成

    // 新規
    CREATE DATABASE <databasename> CHARACTER SET utf8
    // 変更
    ALTER DATABASE (<databasename) CHARACTER SET utf8

### テーブル作成情報

    mysql> show create table categories;

### 文字コード(UTF8)を指定してテーブルを作成

    create table <tablename> (
        ; 定義
    ) default character set utf8;


### AWS RDS

AWSのサービスRDSからMySQLサーバー起動する。

### 接続確認 mysql_test.php

    <?php
        $url = "<endpoint>";    // RDSでMySQLのインスタンスを作成した際にインスタンスのパネルに表示
        $user = "<username>";   // RDSでMySQLのインスタンスを作成する際に設定
        $pass = "<passw0rd>";   // RDSでMySQLのインスタンスを作成する際に設定
        $db = "<dbname>";       // RDSでMySQLのインスタンスを作成する際に設定

        // 接続
        $link = mysql_connect($url,$user,$pass) or die("接続失敗。");

        // DB選択
        $sdb = mysql_select_db($db,$link) or die("DB選択失敗。");

        // クエリ送信する
        $sql = "SELECT * FROM <tablename>";  // <tablename>は作成済みでデータを何件か挿入済みとする
        $result = mysql_query($sql, $link) or die("クエリ送信失敗。");

        //行数取得
        $rows = mysql_num_rows($result);

        //結果保持用メモリ開放
        mysql_free_result($result);

        // 切断
        mysql_close($link) or die("切断失敗。");
    ?>
    
    <?php header('Content-Type: text/html; charset=utf-8');?>
    <html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf8">
        <title>接続テスト</title>
    </head>
    <body>
        <?php echo $rows; ?>
    </body>
    </html>



### Appendix 1. ユーザー、グループの変更

    sudo chown [-f|-R] username (filename|dirname)
    sudo chgrp  [-f|-R]] groupname (filename|dirname)



### Appendix 2. PHPファイル作成テスト

    <?PHP

    $file_name = 'test.txt';

    // 存在確認
    if( !file_exists($file_name) ){
        // ファイル作成
        touch( $file_name );
    }else{
        header('Content-Type: text/html; charset=utf-8');
        echo $file_name . 'は存在しています。処理を終了します';
        exit();
    }


### Appendix 3. Nginxのエラーログ

    /var/log/nginx/error.log


### Appendix 4.「CakePHP 継続的インテグレーション」ディレクトリのパーミション

* /var/www/application 
  ユーザー、グループともにroot。 パーミション755。
* /var/www/application/current以下  
    + ユーザーvagrant
    + グループwww-data
    + パーミション 775

### Appendix 5. 「CakePHP 継続的インテグレーション」のsite-available/defaultファイル

__初期状態ではlocationがコメントアウトされておりphpファイルへアクセスするとダウンロードしてしまう。
下記のようにコメントアウトを外す。__

    server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;

        root /var/www/application/current/app/webroot;
        index index.php index.html index.htm;

        server_name localhost;

        location / {
            try_files $uri $uri/ /index.php?$args;
        }

        location ~ \.php$ {
            try_files $uri =404;
            include /etc/nginx/fastcgi_params;
            fastcgi_pass unix:/var/run/php5-fpm.sock;
            fastcgi_index   index.php;
            fastcgi_intercept_errors on;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_param CAKE_ENV development;
        }
    }



### Appendix 6. SFTP

FileZilla設定方法は下記サイトを参考にして設定 
[WordPress】素人でも出来た！AWS EC2へのサーバー移行方法7 旧データをアップロード | 男子風呂（ぐ）](http://danshiblog.com/job/130128-amazon-ec2-server-setting-wordpressdata-upload.html)


* SFTPはSSHの使用ポート番号(22)を利用するのでAWSのSecurity Groupsで
別途ポートを開ける必要は無い。
* AWSはrootでは接続できないのでubuntuユーザーで接続
* PHPの実行ユーザーはwww-data
* FTPソフトでアップロードした場合のディレクトリ/ファイルの所有者はubuntu。
* 表示するだけならHTML/CSS/JavaScript/PHPの各ファイルの実行権限は004でよい
  (ディレクトリは755)。


### Appendix 7. 手順

    $ sudo apt-get update
    $ sudo apt-get install php5 php5-cli php5-fpm php5-mysql php-pear php5-curl php5-dev php-apc php5-xsl php5-mcrypt mysql-server-5.5 nginx
    $ sudo mkdir -p /var/www/application/current/app/webroot
    $ cd /var/www/application
    $ sudo chown -R www-data current
    $ sudo chmod -R 775 current
    $ sudo vi /etc/nginx/sites-available/default
    $ sudo nginx -s reload


[Ubuntu+Nginx+MySQL+PHP開発環境の目次へ戻る](#ubuntu_nginx_mysql_php)




# <a name="ci">継続的インテグレーション</a>

* [VirtualBox + Vagrant + Chef Soloを使ったCI環境](#virtualbox_vagrant_chef)
* [CI(継続的インテグレーション)](#ci_ci)
* [アジャイル](#agile)
* [BDD:振舞駆動開発 (開発手法)](#bdd)
* [CakePHP開発環境](#env_cake)
* Appendix
    + [Ruby](#appendix_ruby)
    + [Berkshelfはbundleで管理して使うとエラーがでるのでChefDKを使う](#appendix_berkshelf)
    + [behatインストールで発生したエラーへの対応](#appendix_behat)
    + [JenkinsのビルドでAPCの書き込みエラーが発生する問題](#appendix_jenkins)
    + [gitのエディタを変更](#appendix_git_editor)
    + [用語](#appendix_terms)
    + [英語](#appendix_en)


# <a name="virtualbox_vagrant_chef">VirtualBox + Vagrant + Chef Soloをで継続的CI環境構築(開発環境構築/プロビジョニング/デプロイ)</a>

[「CakePHPで学ぶ継続的インテグレーション」 渡辺 一宏, 吉羽 龍太郎, 岸田 健一郎, 穴澤 康裕, 丸山 弘詩  (編集)](http://www.amazon.co.jp/CakePHP%E3%81%A7%E5%AD%A6%E3%81%B6%E7%B6%99%E7%B6%9A%E7%9A%84%E3%82%A4%E3%83%B3%E3%83%86%E3%82%B0%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3-%E6%B8%A1%E8%BE%BA-%E4%B8%80%E5%AE%8F/dp/4844336789/ref=tmm_pap_title_0?ie=UTF8&qid=1421710653&sr=8-1)

## 仮想化ソフトウェア

* VirtualBox
* VMware

仮想化ソフトのゲストOSを開発環境として利用する。


## 仮想開発環境構築ソフトウェア

仮想化ソフトウェアの制御やゲストOSのプロビジョニングを行う。

## Vagrant(仮想開発環境構築ソフトウェア)

> Vagrant（ベイグラント）は、FLOSSの仮想開発環境構築ソフトウェア[1]。VirtualBoxをはじめとする仮想化ソフトウェアやChef（英語版）やSalt（英語版）、Puppetといった構成管理ソフトウェアのラッパーとみなすこともできる。

Wikipedia


### Chef

構成管理ツール /プロビジョニングツール


> プロビジョニング（英: Provisioning）は、本来は「準備、提供、設備」などの意味であり、現在では通常、音声通信やコンピュータなどの分野における、ユーザーや顧客へのサービス提供の仕組みを指す。

Wikipedia


### VirtualBox + Vagrant + Chef

プロビジョニングツールChef Client, Chef SoloはVagrantプラグインvagrant-omnibusで導入する。設定はVagrantfileに記載する。

### Vagrantの役割

1. 仮想サーバの起動・終了
2. ホストOSとゲストOSでディレクトリ共有  
  (例) develop.vm.synced_folder “application”, "/var/www/application/current", (Vagrantfile)
3. 仮想サーバのプロビジョニング  
  Knife-solo, berkshelf

[Command-Line Interface - Vagrant Documentation](http://docs.vagrantup.com/v2/cli/)

### Vagrantのボックス追加

    $ vagrant box add [options] <name, url, or path>
 
(例) Bentoのopscode-ubuntu-14.04の場合

    $ vagrant box add opscode-ubuntu-14.0.4 http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-14.04_chef-provisionerless.box

### 初期化

    $ vagrant init

### 起動・休止・シャットダウン・削除

    $ vagrant up                 // 起動
    $ vagrant halt               // シャットダウン
    $ vagrant destroy            // 削除


### SSHで接続

    $ vagrant ssh

### vagrant プラグインの導入

    $ vagrant plugin install sahara
    $ vagrant plugin install vagrant-omnibus
    $ vagrant plugin install vagrant-cachier

### vagrant plugin installで発生したエラーへの対応

#### エラーメッセージ

> Bundler, the underlying system Vagrant uses to install plugins,
> reported an error. The error is shown below. These errors are usually
> caused by misconfigured plugin installations or transient network
> issues. The error from Bundler is:

> An error occurred while installing nokogiri (1.6.5), and Bundler cannot continue.
> Make sure that `gem install nokogiri -v 1.6.5 succeeds before bundling.

#### 解決策

    $ gem install --install-dir ~/.vagrant.d/gems nokogiri -v '1.6.5'

[vagrant pluginをインストールしようとするとnokogiriエラーがでてインストールできない時の対策 | hypermkt blog](http://blog.hypermkt.jp/can-not-install-vagant-plugin-by-nokogiri/)


## プロビジョニング

Vagrantは仮想サーバーを起動したときや指定したタイミングでプロビジョニングを行うことができる。


### プロビジョニングツール

Chef(Chef Solo, Chef Server)
[Chef | IT automation for speed and awesomeness | Chef](https://www.chef.io/chef/)

### knife-solo

Chef Soloのリポジトリを作成する。

[knife-solo](http://matschaffer.github.io/knife-solo/)

### Berkshelf

Chefのクックブックとその依存関係を管理する。

BerkshelfをBundlerを使いインストールしたとき下記コマンドでエラーが発生した。
Bundlerを使わずChefDKを利用することで解決した。

    $ bundle exec berks vendor ./cookbooks

### ChefDk

Chef関連のツール集(Berkshelf, Knife-soloなど)

[Chef Development Kit | Chef Downloads | Chef](https://downloads.chef.io/chef-dk/)
 

Bundlerでエラーが発生した下記処理もChefDKで解決した。

    $ bundle exec knife cookbook create <name> -o site-cookbooks

#### ChefDk導入後

    $ knife cookbook create <name> -o site-cookbooks

## Vagrant, Chefを使ったプロビジョニング手順

1. site-cookbooks/recipeでプロビジョニングの内容を記述したrubyファイル(rb)作成する。
2. berks vendor cookbooksでrecipeのパッケージをcookbooksへ配置する。
3. vagrant provisionでプロビジョニングする(プロビジョニングは仮想サーバー起動時にも行われる。

    |— site-cookbooks
            |— recipe
                    |— default.rb
                    |— develop.rb
    |
    |— Vagrantfile
    |         chef_run_list = %w [
                  recipe[apt]
                  recipe[phpenv::default.rb]
                  recipe[phpenv::develop]
              ]
 

1. $ berks vendor cookbooks
2. $ vagrant up
3. $ vagrant provision


### Capistrano3

開発環境からデプロイ環境にデプロイするツール。


## 継続的インテグレーションサーバー

### Jenkins

GitHubリポジトリ

Jenkinsの設定画面でエラーがでる。下記を行う


stderr: Host key verification failed.が出たら、git ls-remoteを叩く！

[Jenkinsを導入してGithub, Bitbucketから自動ビルドを可能にするまで](http://tsukaby.com/tech_blog/archives/250)


パスフレーズなしでキーを作成した。こちらで解決した可能性が高い。

[Google グループjenkinsからgithubへのssh接続](https://groups.google.com/forum/#!topic/jenkinsci-ja/JkjRAyQyOKE)




# <a name="ci_ci">CI(継続的インテグレーション)</a>


主に下記書籍の読書メモ

[「CakePHPで学ぶ継続的インテグレーション」 渡辺 一宏, 吉羽 龍太郎, 岸田 健一郎, 穴澤 康裕, 丸山 弘詩  (編集)](http://www.amazon.co.jp/CakePHP%E3%81%A7%E5%AD%A6%E3%81%B6%E7%B6%99%E7%B6%9A%E7%9A%84%E3%82%A4%E3%83%B3%E3%83%86%E3%82%B0%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3-%E6%B8%A1%E8%BE%BA-%E4%B8%80%E5%AE%8F/dp/4844336789/ref=tmm_pap_title_0?ie=UTF8&qid=1421710653&sr=8-1)

__引用でページ番号のあるものは本書から引用したもの。__

### 本記事サーバー構成

サーバー 3台

* 開発サーバー
  192.168.33.10
* CIサーバー
  192.168.33.100
* 公開サーバー
  192.168.33.200

### ディレクトリ構造

develop.vm.synced_folder "application", "/var/www/application/current"

    current
      |--app  // Cakeアプリケーション
          |-- Config    // CakePHP コンフィグレーション
          |-- Console
          |-- Controller
          |-- Test   // CakePHPテスト用ディレクトリ
          |.....
          |-- Vendor // Composerパッケージ用ディレクトリ
          |-- composer.json
          |-- phpunit.xml
      |--deploy // デプロイ Capistorano
      |--features // BDD
      |-- build.xml



### CIの流れ

1. 開発サーバーのソースをGitHubへPushする。
2. CIサーバーはGitHubからソースをPullしテストを行う(自動処理)。
  本記事ではCIサーバーの自動処理をJenkinsで管理する。
3. 開発サーバーでデプロイ処理を行う。デプロイは自動処理される。
  本記事ではCapistranoでデプロイサーバーへ自動でプロイする。

[Welcome to Jenkins CI! | Jenkins CI](http://jenkins-ci.org/)
[capistrano/capistrano](https://github.com/capistrano/capistrano)


### CIツール

* 継続的インテグレーションツール
    + Jenkins
* (JavaScript/HTML/CSS)自動化ツール
    + Grunt
* テストツール
    + Behat(BDD駆動開発用テストツール)
    + PHPUnit(単体テストツール)
* コードインスペクション(コード検査ツール)
    + PHP_CodeSniffer
* ドキュメンテーションツール
    + phpDocumentor

### 開発手法

* アジャイル
* BDD:振舞駆動開発 (開発手法)



## <a name="agile">アジャイル</a>

> 短い期間でリリース可能なソフトウェアを繰り返し提供することに重きを置きます
(p.007)

* アジャイル
    + Scrum(スクラム)
      アジャイル開発手法の１つ。
        - スプリント
          設計-開発-テスト-レビューの単位。
 
 
#### Scrum(スクラム)

> 2週間から1か月程度の短時間で、設計から開発、テストなどの一連の作業を繰り返し、ソフトウェアを仕上げます。
(p.007)
 
#### スプリント

> 各スプリントでは最終的に実際に動作するソフトウェアの提供が求められます。
(p.007)



## <a name="bdd">BDD:振舞駆動開発 (開発手法)</a>

振舞駆動開発(BDD: Behavior Driven Develop)。

> BDDはユーザーストーリーと、そこから導きだされるビジネスロジックをテストすることに注目しています。
(p.178)

> ユーザーストーリーから具体的な利用シーンを抽出します。これを「シナリオ」と呼びます。
> BDDのフレームワークを使う場合は、ドメイン特化型言語(DSL domain-specific language)と呼ばれる記法で記述します。
> 代表的なDSLは「Gherkin」と呼ばれ、記述したシナリオは、受け入れテストとして「Cucumber 」や「Behat」などのテスティングフレー> > ムワークによって自動実行できます。
(p.181)


### BDDの流れ

->失敗する受け入れテスト -> __| 失敗する単体テスト-> 実装してテスト成功させる->
リファクタリング |__ -> ステップの定義 -> 「ステップを実行してシナリオを成功させ
る」(p.180) -> リファクタリング ->

#### ユーザーストーリー

> ユーザーストーリーとは、時間軸に沿ってユーザーが得られる体験や価値、出来事を、筋書きとして記述します。ユーザーストーリーはそれ単独でリリース可能であり、ビジネス要件やスケジュール、見積などが含まれています。

##### ユーザーストーリーの例

> 会員として、新しい記事を投稿できる、なぜなら会員は自分の言葉で発信したいからだ
(p.178)

#### シナリオ

シナリオはユーザーストーリーから定義される。

> シナリオは連続する出来事を表現するもので、よりアプリケーションが実現することに注目していると言えます。
(p.181)


#### フィーチャ


>フィーチャとは、顧客の視点から「アプリケーションに必要となる機能や特徴」のことで、
> 最初の2行目に書かれている通り、ユーザーストーリー記法で記述する事を推奨しています。
(p.182)

* フィーチャの記法
Gerkin記法
* フィーチャの拡張子
.feature


#### フィーチャの例

「CakePHPで学ぶ継続的インテグレーション」 渡辺 一宏, 吉羽 龍太郎, 岸田 健一郎, 穴澤 康裕, 丸山 弘詩  (編集)

##### ディレクトリ構造


    current
       |— app
            |— Config
                |— bootstrap
                    |— environments
                         |— ci.php
                         |— development.php
                         |— test.php
                    |— environments.php
                |— …
            |
            |— Console
            |— Controller
            |— …
            |— composer.json
            |— index.php
            |— phpunit.xml
        |
        |— feature
                |— boostrap
                |— …
                |— blog_new_post.feature


##### フィーチャファイル(.feature)

「CakePHPで学ぶ継続的インテグレーション」 渡辺 一宏, 吉羽 龍太郎, 岸田 健一郎, 穴澤 康裕, 丸山 弘詩  (編集)


    # language: ja
    フィーチャ: 会員として新しい記事を投稿できる、なぜなら会員は自分の言葉で発信したいからだ
    背景:
    前提 "会員" としてログインしている
    シナリオ: 新しい記事を投稿できる
    もし 以下の内容で記事を投稿する
    | タイトル | はじめてのブログ |
    | 本文 | はじめまして |
    ならば 新しい記事が登録されていること
    シナリオ: タイトルなしでは新しい記事は投稿できない
    もし 以下の内容で記事を投稿する
    | タイトル | |
    | 本文 | はじめまして |
    ならば 新しい記事が登録できないこと


#### フィーチャの実行

 __フィーチャファイル(.feature)はBehatを使って実行する。__


### Behat/Mink (BDD用テスティングフレームワーク)

BDDのためのPHPで作成した(ユーザー)ストーリーベースのテスティングフレームワーク。

### Bddプラグイン BehatのCakePHP2用プラグイン (プラグイン)

> BDD(Behavior Driven Development) integration plugin for CakePHP2

[sizuhiko/Bdd](https://github.com/sizuhiko/Bdd)

 Bddプラグインはフィーチャファイルを実行する。
 BddプラグインはCake標準テストケースとPHPUnitを使い実行される。

#### インストール

* Composerを使いインストール
* composer.jsonにプラグインのリポジトリを登録[^composer_repo]

[^composer_repo]:>Packagistから所得できないパッケージがある場合は、
composer.jsonの[repositories]に記述する事で利用できます。(p.187)

composer.json

    "repositories": [
      {
        "type": "vcs",
        "urrl: "git://github.com/sizuhiko/Bdd.git
      }
    ],

#### フィーチャーファイルのテスト

    $ Console/cake Bdd.story

### BDDと単体テスト

__フィーチャは最終的に単体テストの集まりを実行する。__


## Jenkins

継続的インテグレーション用



## <a name="env_cakephp">CakePHP開発環境</a>

* PHPUnit ユニットテスト  
  CakePHPはユニットテストをPHPUnitで行う。
* CakeDC Migration Plugin  
  データベースマイグレーション
* Behat  
  > Behat is an open source behavior-driven development framework for PHP
  [Behat Documentation &mdash; Behat 2.5.3 documentation](http://docs.behat.org/en/v2.5/)
* sizuhiko/Bdd  
  CakePHP2用のプラグイン
* behat/mink-goutte-driver  
  JavaScriptを使わずBehatを利用するプラグイン。

### デバッグレベル

app/Config/core.php

    Configure::write('debug', 2);



## <a name="appendix_ruby">Appendix Ruby</a>

    $ brew install ruby

brewをつかい最新版をインストールした(2.2.0)。
バージョンの確認をすると下記のようにMac OSに標準でインストールされているバージョンが表示される。

    $ ruby --version
    ruby 2.0.0p481 (2014-05-08 revision 45883) [universal.x86_64-darwin13]

rubyのバージョンを完全にアップデートする方法は解らなかった。
下記をためしたが解決はしていない。


### rbenv/ruby-buildでインストール

rbenvはシェルによりrubyを切り替える。


    $ brew update
    $ brew install rbenv ruby-build

    $ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

    $ source ~/.bash_profile
    $ rbenv --version
    rbenv 0.4.0


[rbenv を利用した Ruby 環境の構築 ｜ Developers.IO](http://dev.classmethod.jp/server-side/language/build-ruby-environment-by-rbenv/)


    $ ruby —version
    ruby 2.0.0p481 (2014-05-08 revision 45883) [universal.x86_64-darwin13]

やはりMac OS標準のRubyが使われる。



## <a name="appendix_berkshelf">Appendix Berkshelfはbundleで管理して使うとエラーがで
るのでChefDKを使う</a>

[ChefDk の berkshelf で cookbook をダウンロードする - Qiita](http://qiita.com/shin1x1/items/872cf5b9396516068892)
[Chef Development Kit | Chef Downloads | Chef](https://downloads.chef.io/chef-dk/mac/#/)


## <a name="appendix_behat">Appendix behatインストールで発生したエラーへの対応</a>

### エラー

Could not fetch https://api.github.com/repos/sizuhiko/Bdd/git/refs/heads?per_page=100, enter your GitHub credentials to go over the API rate limit
The credentials will be swapped for an OAuth token stored in /home/vagrant/.composer/auth.json, your password will not be stored
To revoke access to this token you can visit https://github.com/settings/applications
Username: xxxxx
Password:xxxxx
Token successfully created

Could not fetch https://api.github.com/authorizations, enter your GitHub credentials to go over the API rate limit
The credentials will be swapped for an OAuth token stored in /home/vagrant/.composer/auth.json, your password will not be stored
To revoke access to this token you can visit https://github.com/settings/applications
Username:xxxxx
Password:xxxxx
An existing OAuth token for Composer is present and will be reused

### 対応

下記ファイルに記載されているトークン(Token)をcomposer.jsonファイルのconfigへ追記する。
[Composer Update Fails due to Github Authorization · Issue #3542 · composer/composer](https://github.com/composer/composer/issues/3542)

    /home/vagrant/.composer/auth.json

composer.json

    {
        …..
        "config": {
            "vendor-dir": "Vendor",
            "github-oauth": {
                “github.com”: <Token>
            }
        },
        …..
    }



## <a name="appendix_jenkins">Appendix JenkinsのビルドでAPCの書き込みエラーが発生
する問題</a>

PHPUnitでの単体テストがうまく行っていない


    vagrant@ci:/var/lib/jenkins/jobs/blogapp/workspace/app$  Console/cake test app AllTests
    PHP Warning:  _cake_core_ cache was unable to write 'cake_dev_eng' to File cache in /var/lib/jenkins/jobs/blogapp/workspace/app/Vendor/cakephp/cakephp/lib/Cake/Cache/Cache.php on line 323

[シェルからCakePHPを使うとAPC書き込みに失敗する問題 - 創作メモ帳](http://sousaku-memo.net/php-system/952)

### 1. /etc/php5/cli/conf.dディレクトリ20-apc.iniを作成。


/etc/php5/cli/conf.d/20-apc.iniの内容

    ;apc extension module を有効にします。
    ;extension = apc.so
    ;apc.enabled=1

    ;# apc extension module のオプションを設定します。

    ;## コンパイラキャッシュのために割り当てる共有メモリセグメントの数。
    apc.shm_segments=1

    ;## 個々の共有メモリセグメントの大きさを MB 単位で指定します。
    ;#apc.shm_size=32
    apc.shm_size=128M

    ;## 最適化レベル。ゼロは最適化を無効にし、 値を大きくするほど最適化のレベルが高くなります。
    apc.optimization=0

    ;## キャッシュされているエントリが、 他のエントリに割り当てられるまでスロットに残っていることの可能な秒数。
    ;#apc.ttl=7200
    apc.ttl=86400
    ;#apc.user_ttl=7200
    apc.user_ttl=86400

    ;## Web サーバで読み込まれるソースファイルの総数についての 「ヒント」。よくわからない場合はゼロを指定するか、 単に無視してください。
    apc.num_files_hint=1024

    ;## --enable-mmap を用いて MMAP サポートつきでコンパイルされている場合、ここで mktemp 形式のファイルマスクを指定します。
    apc.mmap_file_mask=/tmp/apc.XXXXXX

    ;## たいていは、テストやデバッグ用に使用します。これを設定すると CLI バージョンの PHP で APC を有効にします。
    apc.enable_cli=1

    ;## デフォルトで On です。しかし、これを Off にして + で始まる apc.filters  とともに使用することで、 フィルタに一致したファイルのみをキャッシュすることが可能です。
    apc.cache_by_default=1

### 1を記載したら下記のエラーがでるようになった。

    PHP Warning:  PHP Startup: Unable to load dynamic library '/usr/lib/php5/20121212/apc.so' - /usr/lib/php5/20121212/apc.so: cannot open shared object file: No such file or directory in Unknown on line 0

1. apc.soをインストールしたい。
2. sudo pecl install APCではエラー出てインストールできなかった。
3. apc.soはphp-develに含まれるらしい。
4. php-develをインストールするためにyumをインストールした。
  sudo apt-get install yum
5. yum install php-develではエラー



## <a name="appendix_git_editor">Appendix gitのエディタを変更</a>

Ubuntuでnanoをviへ変更

git config --global core.editor "vi"

[gitのエディタをnanoから他へ変更する | J-Linuxer](http://jlinuxer.dip.jp/?p=645)


## <a name="appendix_terms">Appendix 用語</a>

> .soファイル 【 shared object file 】 .so形式 / .soフォーマット

> soファイル / so形式
> UNIX系OSの共有ライブラリのファイル形式。拡張子が「.so」であることからこのように呼ばれる。実行可能形式のプログラムが格納されているが、単体で起動することはできず、他のプログラムにリンクしてその機能を呼び出すようになっている。
>この形式のファイルはプログラムの実行時にリンクされる動的リンク(ダイナミックリンク)ライブラリで、ビルド時にリンクされる静的リンク(スタティックリンク)ライブラリの場合は「.a」(archiveの略)という拡張子になる。

[.soファイルとは 〔 soファイル 〕 【 shared object file 】 - 意味/解説/説明/定義 ： IT用語辞典](http://e-words.jp/w/2EsoE38395E382A1E382A4E383AB.html)





# <a name="aws">AWS(Amazon Web Services)でWEBサービス運用</a>

## 目次

* [Linuxセキュリティ](#aws_security)
* [EC2](#aws_ec2)
* [RDS](#aws_rds)
* [Route 53](#aws_route53)
    + 独自ドメイン運用
* [S3](#aws_s3)
* [Ubuntu + Postfixでメールを運用](#aws_postfix) 
* [課金](#aws_bills)
* [Linuxコマンド(Ubuntu)](#aws_cmd_ubuntu)
* [AWS 用語](#aws_aws_terms)



## <a name="#aws_security">Linuxセキュリティ</a>

### 目的

AWSでUbuntuを安全に運用する。

1. サーバーOSへのログイン    
  AWSはSSHを使いログインする。初期設定はrootのログインを禁止している。
2. サービスへの不正アクセス  
  各サービスはiptableを使いポート番号を開閉を制御しパケット通信をコントロールする。


### Ubuntuへログイン

rootログインを禁止する。  
AWSではrootではログインできずubuntuユーザーでログインする。


### コンソールからrootのログイン禁止

    $ su root
    $ echo > /etc/securetty


### SSHでrootでログイン禁止 およびSSHポートの変更

#### 設定ファイル変更

    /etc/ssh/sshd_config

    PermitRootLogin no
　　Port 22222

#### SSHサーバー再起動

SSHサーバー再起動

　　/etc/init.d/ssh restart

__AWSではうまく行かなかった。上記の変更をしても22番でログインできた。__

### ユーザー

適切にユーザーを管理する。

#### rootユーザーにパスワード設定

    $ sudo passwd root
    nter new UNIX password: 
    Retype new UNIX password: 
    passwd: password updated successfully

#### suの利用制限

デフォルトではすべてのユーザーがsuを使いroot権限を利用できる。  
下記設定を行いsuコマンドを実行できるユーザーをwheelグループのユーザーに制限する。

/etc/pad.d/suファイルを編集


    // コメントアウトを解除
    # auth       required   pam_wheel.so
    auth       required   pam_wheel.so

su rootができなくなる。

    ubuntu@xxx:~$ su root
    Password: 
    su: Permission denied

Ubuntuはwheelユーザーがないので作成しubuntuをwheelグループへ追加する。

    ubuntu@xxx:~$ sudo addgroup --gid 11 wheel
    Adding group `wheel' (GID 11) ...
    Done.

    // wheelグループへubuntuを追加
    ubuntu@xxx:~$ sudo usermod -G wheel ubuntu
    

su rootができる。

    ubuntu@xxx:~$ su root
    Password: 
    root@xxx: #

#### ユーザー一覧

    // ユーザー情報表示
    $ cat /etc/passwd
    // ユーザー一覧表示
    $ cut -d: -f1 /etc/passwd

#### 個別のユーザー情報

    // パスワードやホームディレクトリなど
    $ cat /etc/passwd | grep <username>

    // 所属グループなどの情報
    $ id <username>

#### グループ一覧

   $ cut -d: -f1 /etc/group

#### ユーザーをグループに追加/削除

    $ sudo gpasswd -a <username> <groupname>

__aオプションを指定しないと既存グループが削除される。__

   $ sudo gpasswd -d <username> <groupname>


### サービス

不要なサービスは停止する。

#### サービス一覧

    // RPM系のchkconfigと同様の機能を持つsysv-rc-confインストール
    $ sudo apt-get sysv-rc-conf
    // 起動サービス一覧表示
    $ sudo sysv-rc-conf --list

#### サービス停止

    $ sudo sysv-rc-conf <service> Off

### ネットワークセキュリティ

#### ポートの確認

    $ netstat -atun


### OSの更新


### パッケージの更新

    // 最新のパッケージリスト取得
    $ sudo apt-get update
    // インストール済みパッケージをすべて更新
    $ sudo apt-get upgrade

特定のパッケージのみ更新する場合は下記の記事が参考になった。

[apt / aptitude で特定のパッケージのみアップグレードする方法 - Devslog](http://devslog.com/article/20120216085533.html)


### ポートの制御(iptables)

利用するポートInbound/Outboundを設定。最小限にする。
yumやapt-getはポート番号80を使う。

[そうかyumってhttpポート（80番）を使うのか・・・](http://app.m-cocolog.jp/t/typecast/691311/578213/73514519)



## <a name="aws_ec2">EC2</a>

Amazon Linux AMI 2014.09.1 (HVM) - ami-4985b048

* [SSH](#aws_ec2_ssh)
* [Security Groups](#aws_ec2_secuity)
* [Elastic IPs](#aws_ec2_ip)
* [RPM系 Apache、MySQL, PHPインストール](#aws_ec2_rpm_app)
* [Debian系Nginx, MySQL,PHP](#aws_ec2_debian_app)
    + /var/www/application/current/app/webrootでPHPを動作

### <a name="aws_ec2_ssh">SSH</a>

* EC2
    + NETWORK & SECURITY
        - Key Pairs

    $ ssh -i <private_key_file> <destination>


### <a name="aws_ec2_security">Security Groups</a>

* EC2
    + NETWORK & SECURITY
        - Security Groups


どのポートを空けるか(どのサービスをりようするか)を各インスタンスごとにセキュリティグループとして設定する。  
どのポートをどこから許可するか？  


### <a name="aws_ec2_ip">Elastic IPs</a>

* EC2
    + NETWORK & SECURITY
        - Elastic IPs

__T2 instances are VPC-only. Your T2 instance will launch into your VPC. Learn more about T2 and VPC.__

インスタンスの起動・再起動で固定IPは再度割り当てられ値が変わる。
再起動ごも固定にするにはElastic IPが必要?。


### <a name="aws_ec2_rpm_app">RPM系 Apache、MySQL, PHPインストー</a>

    // 初回接続時にはyumを更新
    $ sudo yum update
    // Apache, MySQL, PHPをインストール
    $ yum install httpd mysql php
    // ApacheをOS起動時に立ち上げる
    $ sudo chkconfig httpd on
    // Apache起動
    $ sudo service httpd start


### <a name="aws_ec2_debian_app">Debian系Nginx, MySQL,PHP]<a>

    // apt-getを利用する前に最新の状態へ
    $ sudo apt-get update

#### パッケージインストール

    apt-get install php5 php5-cli php5-fpm php5-mysql php-pear php5-curl php5-dev php-apc php5-xsl php5-mcrypt mysql-server-5.5 nginx git

[PHP: Debian GNU/Linux へのインストール - Manual](http://php.net/manual/ja/install.unix.debian.php)

#### PHP動作確認

Evernoteの「2015.01.16 AWS UbuntuでPHPを動作させる」を参照。


## <a name="aws_rds">RDS</a>

* MySQL
* 課金

### MySQL

1. セキュリティグループでMySQLのポート(3306)を開ける
2. RDSのインスタンスへDB Security Groupsを設定する

[Amazon RDS ～EC2インスタンスからDBインスタンスへの接続～　|ec2 db インスタンス　接続　方法 | ナレコムAWSレシピ](http://recipe.kc-cloud.jp/archives/397)

    $ mysql -h <Endpoint> -u <username> -p

AWSではパスワードなしでrootでログインすることはできない。



### 課金

[よくある質問 - Amazon RDS（リレーショナルデータベースサービス Amazon Relational Database Service） | アマゾン ウェブ サービス（AWS 日本語）](http://aws.amazon.com/jp/rds/faqs/)

上記ページの請求に記載。


## <a name="aws_route53">Route 53</a>

> DNS サービスです。このサービスを利用して独自ドメインとEC2 インスタンスを紐付けます。
[初心者のボクが「AWS + WordPress」でブログを作る！ Vol.2 〜独自ドメインで運用しよう〜 | ひげメガネのブログ](http://blog.higemegane.com/2014/01/wp_with_aws2/)

### 利用サービス

* EC2
    + EIastic IPs 
      固定IP取得しEC2インスタンスと結びつける。
* Route 53
     独自ドメインと固定IPを結びつける

DNSサーバー正しく設定されているかを確認する。

    $ nslookup <dns> <domain>



### 確認

    $ host example.com
    example.com has address xxx.xxx.xxx.xxx

xxx.xxx.xxx.xxxがElastic IPsで取得したIPアドレスのならば処理が正常に行われている。


[AWS Developer Forums: メールの送受信方法について …](https://forums.aws.amazon.com/thread.jspa?messageID=307586)




## <a name="aws_postfix">Ubuntu + Postfixでメールを運用</a>


* [Debian(Ubuntu)で postfix を使ってみる | レンタルサーバー・自宅サーバー設定・構築のヒント](http://server-setting.info/debian/debian-postfix-setting.html)
* [AWS Developer Forums: メールの送受信方法について …](https://forums.aws.amazon.com/thread.jspa?messageID=307586)

## 送信環境構築

1. AWS EC2 > Security Groupで送信用ポート設定。
2. AWS Route 53でMXレコード設定
3. Postfixをインストール
4. Postfix設定(/etc/postfix/main.cf)
5. 送信テスト mailコマンド

## Route 53でMXレコード追加

### 前提

Aレコードにexample.comを設定している。


### MX Main exchangeレコード追加

* Name
  mail.example.com
* Type  
  Mail Exchange
* Value  
  10 mail.example.com



## Postfix

### インストール

    $ sudo apt-get install postfix
        // バージョン確認
    $ /usr/sbin/postconf | grep mail_version
    mail_version = 2.11.0

[Postfixのバージョンを確認する | CoDE4U](http://blog.code4u.org/archives/1135)


### 設定ファイル

    /etc/postfix/main.cf

下記ページを参考に設定。


* [Debian(Ubuntu)で postfix を使ってみる | レンタルサーバー・自宅サーバー設定・構築のヒント](http://server-setting.info/debian/debian-postfix-setting.html)
* [AWS Developer Forums: メールの送受信方法について …](https://forums.aws.amazon.com/thread.jspa?messageID=307586)
*[Postfix+Dovecotによるメールサーバ構築 ｜ Developers.IO](http://dev.classmethod.jp/cloud/aws/mail_server_with_postfix_and_dovecot/)

## postfix 再起動

    $ sudo /etc/init.d/postfix restart



## mailコマンドで送信確認

### インストール

    $ sudo apt-get install mailutils


試した環境ではmailコマンドの終了は.ではなくエンター + Ctrl + D。


### メールのログ

    /var/log/mail.log
    /var/log/mail.err



## <a name="aws_bills">課金</a>

* Billing & Cost Management
    + Preferences
        - Receive Billing Alerts
           Manage Billing Alerts



## <a name="aws_cmd_ubuntu">Linuxコマンド(Ubuntu)</a> 

* ポートの確認
  $ netstat -tlnp
* インストール済みパッケージの確認
  $ dpkg -l
  $ aptitude search "~i"
* プロセス
  ps -ef|grep postfix
* DNS確認
  host domain
* cat
  標準出力へ出力

## <a name="aws_terms">用語</a>

### Amazon Virtual Private Cloud VPC


### オンプレミス

> オンプレミス (英語 on-premises)[注 1] とは、情報システムを使用者（通常は企業）自身が管理する設備内に導入、設置して運用することをいう。元来は普通に見られる運用形態であったが、2005年ころからインターネットに接続されたサーバファームやSaaS、クラウドコンピューティングなど、外部のリソースをオンデマンドで活用する新たな運用形態が浸透するにつれて、従来の形態と区別するためにレトロニムとして「オンプレミス」の語が使われるようになった。自社運用（型）とも訳される[1]。

Wikipedia

### MTA 【 Message Transfer Agent 】

> ネットワーク上でメールを転送・配送するソフトウェア。いわゆるメールサーバの中心的な役割の一つで、利用者がメールソフト(MUA：Mail User Agent)などで送信したメールを受け取り、宛先に基づいて振り分け、相手方のMTAなどに転送する。
>
> MTAはメールを受け取って次の適切な配信先を決定するのが主な役割で、配信先への送信・転送はMDA(Mail Delivery Agent)が行うが、実際のメールサーバの実装としてはMTAとMDAが一体となっていることが多く、MDAの役割まで含めてMTAということが多い。利用者とのメールの送受信や、MTA間のメールの送受信にはSMTP(Simple Mail Transfer Protocol)という通信プロトコルが標準的に用いられる。
>
>これに対し、メールサーバの役割のうち、受信側で送られてきたメールを受け取って保管し、利用者のメールソフトなどに引き渡すソフトウェアはMRA(Mail Retreival Agent)と呼ばれ、MTAとは独立して実装・提供されることが多い。一般的にはメールサーバを構成する際はMTA(MDAを含む)ソフトとMRAソフトを組み合わせて導入する。

[MTAとは 【 Message Transfer Agent 】 - 意味/解説/説明/定義 ： IT用語辞典](http://e-words.jp/w/MTA.html)


### iptables

> iptablesは、Linuxに実装されたIPv4用のパケットフィルタリングおよびネットワークアドレス変換 (NAT) 機能（複数のNetfilterモジュールとして実装されている）の設定を操作するコマンドのこと。Netfilterは、いわゆるファイヤウォールやルータとしての役割を果たす。IPv6 用の実装は ip6tables である。

Wikipedia

# Appendix 1. Ubuntu + Apache + MySQL + PHP

    $ sudo apt-get update

    $ sudo apt-get install apache2
    $ sudo apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql
    $ sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt

### Apache設定ファイル

/etc/apache2/apache2.conf


# Appendix 2.

とりあえずUbuntu + Nginxで動作した設定

    server {
            listen 80 default_server;
            listen [::]:80 default_server ipv6only=on;

            root /var/www/application/current/app/webroot;
            index index.php index.html index.htm;

            # Make site accessible from http://localhost/
            server_name localhost;

            location / {
                    try_files $uri $uri?$args $uri/ /index.php?$uri&$args /index.php?$args;
            }
            
            location ~ \.php$ {
                    try_files       $uri =404;
                    fastcgi_pass unix:/var/run/php5-fpm.sock;
                    #fastcgi_pass    127.0.0.1:9000;
                    fastcgi_index   index.php;
                    fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
                    include         fastcgi_params;
            }
     }
