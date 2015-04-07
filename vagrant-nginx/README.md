# 内容

WEBサービスをAWSで運用するために勉強していることを書き留めた個人的なメモ書き。

# 目標

継続的インテグレーション可能な開発環境を構築しAWSで高品質なWEBサービスを提供する。

# 前提とする環境

* Ubuntu  
  14.04
* Nginx  
  nginx version: nginx/1.4.6 (Ubuntu)
* MySQL  
  5.5.40-0ubuntu0.14.04.1
* PHP  
  PHP Version 5.5.9-1ubuntu4.5
	- CakePHP 2.6.1
* JavaScript
	- jQuery v1.8.2

# 参考書

* [「CakePHPで学ぶ継続的インテグレーション」 渡辺 一宏, 吉羽 龍太郎, 岸田 健一郎, 穴澤 康裕, 丸山 弘詩  (編集)](http://www.amazon.co.jp/CakePHP%E3%81%A7%E5%AD%A6%E3%81%B6%E7%B6%99%E7%B6%9A%E7%9A%84%E3%82%A4%E3%83%B3%E3%83%86%E3%82%B0%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3-%E6%B8%A1%E8%BE%BA-%E4%B8%80%E5%AE%8F/dp/4844336789/ref=tmm_pap_title_0?ie=UTF8&qid=1421710653&sr=8-1)
* [「CakePHP2 実践入門」(WEB+DB PRESS plus) 安藤 祐介, 岸田 健一郎, 新原 雅司, 市川 快, 渡辺 一, 鈴木 則夫](http://www.amazon.co.jp/gp/product/4774153249/ref=pd_lpo_sbs_dp_ss_2?pf_rd_p=187205609&pf_rd_s=lpo-top-stripe&pf_rd_t=201&pf_rd_i=4839924317&pf_rd_m=AN1VRQENFRJN5&pf_rd_r=0Y02W25AP82Z83YPCYQR)
* [「Linuxサーバーセキュリティ徹底入門 ープンソースによるサーバー防衛の基本」中島 能和](http://www.amazon.co.jp/Linux%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E5%BE%B9%E5%BA%95%E5%85%A5%E9%96%80-%E3%83%BC%E3%83%97%E3%83%B3%E3%82%BD%E3%83%BC%E3%82%B9%E3%81%AB%E3%82%88%E3%82%8B%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E9%98%B2%E8%A1%9B%E3%81%AE%E5%9F%BA%E6%9C%AC-%E4%B8%AD%E5%B3%B6-%E8%83%BD%E5%92%8C/dp/4798132381/ref=tmm_jp_oversized_meta_binding_title_0?ie=UTF8&qid=1421728106&sr=1-1)
* [「Amazon Web Services クラウドデザインパターン実装ガイド」 大澤 文孝, 玉川 憲, 片山 暁雄, 鈴木 宏康](http://www.amazon.co.jp/gp/product/4822211983/ref=pd_lpo_sbs_dp_ss_2?pf_rd_p=187205609&pf_rd_s=lpo-top-stripe&pf_rd_t=201&pf_rd_i=4822211967&pf_rd_m=AN1VRQENFRJN5&pf_rd_r=1NXM6E1R7GBQEBM40WFP)  
* [現場で使える MySQL (DB Magazine SELECTION) 単行本 – 2006/3/17 松信 嘉範 ](http://www.amazon.co.jp/%E7%8F%BE%E5%A0%B4%E3%81%A7%E4%BD%BF%E3%81%88%E3%82%8B-MySQL-DB-Magazine-SELECTION/dp/4798111139/ref=sr_1_1?s=books&ie=UTF8&qid=1428203520&sr=1-1&keywords=%E7%8F%BE%E5%A0%B4%E3%81%A7%E4%BD%BF%E3%81%88%E3%82%8B+MySQL+%28DB+Magazine+SELECTION%29)

# <a name="index">目次</a>

* [ファイル取得コマンド](#get)
* [パッケージ管理システム](#package)
* [HTML/CSS/JavaScript開発環境](#html_css_javascript)
* [Ubuntu+Nginx+MySQL+PHP開発環境](#ubuntu_nginx_mysql_php)
    + [Ubuntu](#ubuntu)
    + [Nginx](#nginx)
    + [PHP](#php)
    + [MySQL](#mysql)
    + [Postfix](#postfix)
    + [Git](#git)
* [継続的インテグレーション](#ci)
	+ [環境構築](#ci_dev)
		- [Virtualbox 仮想化ソフト](#ci_virtualbox)
		- [Vagrant 仮想開発環境構築ソフトウェア](#ci_vagrant)
		- [Chef プロビジョニングツール](#ci_chef)
		- [Vagrant, Chefを使ったプロビジョニング手順](#ci_vagrant_chef)
	* [Jenkins CIサーバー](#ci_jenkins)
	* [CircleCi クラウド型CIサーバー](#ci_circleci)
	* [デプロイ処理](#ci_deploy)
	* [CakePHP開発環境](#env_cakephp)
	* [開発手法](#ci_process)
		- [アジャイル](#agile)
		- [BDD:振舞駆動開発 (開発手法)](#bdd)
* [AWS(Amazon Web Services)でWEBサービス運用](#aws)
* [Linuxコマンド](#cmd)
* [用語](#terms)
* [Appendix](#appendix)
    + [ディレクトリ/ファイル所有者・グループ変更](#appendix_owner)
    + [PHPのファイル作成雛形](#appendix_php_file)
    + [動作したNginx設定ファイルsite-available/default](#appendix_nginx_default)
    + [書籍「CakePHP 継続的インテグレーション」のNginx設定ファイルsite-available/default](#appendix_nginx_default_book)
    + [php.iniの設定を反映されないときの対処](#appendix_php_ini)
    + [zipインストール](#appendix_zip)
    + [参考書籍 CakePHPで学ぶ継続的インテグレーションのChefでMySQLをインストールした場合のパスワード](#appendix_chef_mysql)
    + [FileZillaを使ったAWS EC2へのSFTP接続](#appendix_aws_sftp)
    + [Ubuntu + Apache + MySQL + PHP](#appendix_ubuntu_apache_mysql_php)
    + [Berkshelfはbundleで管理して使うとエラーがでるのでChefDKを使う](#appendix_berkshelf)
    + [behatインストールで発生したエラーへの対応](#appendix_behat)
    + [Jenkins設定画面でエラー](#appendix_jenkins_display)
    + [Appendix cuisineモジュール](appendix_cuisine)
    + [JenkinsのビルドでAPCの書き込みエラーが発生する問題](#appendix_jenkins)
    + [soファイル](#appendix_so)
    + [Ruby](#appendix_ruby)
    + [AWS構築手順](#appendix_recipe)
    + [Appendix NginxでBasic認証](#appendix_basic)


# <a name="get">ファイル取得コマンド</a>

## wget

> wget とは、UNIXコマンドラインで HTTP や FTP 経由のファイル取得を行えるツールです。
Webサイトであれば、リンク先を階層で指定して一気に取得することができ、オフラインでじっくり読んだり、ミラーサイトを簡単に作ることが可能です。
また、ダウンロードが途中で止まってしまった場合は、途中からやり直すレジューム機能があり便利です。
>
> wget はGNUプロジェクトで作られているフリーソフトです。改良も配布も自由なので、是非活用しましょう。

[wget の使い方](http://tech.bayashi.net/svr/doc/wget.html)

(例)

	wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -

-q -Oオプションが指定されているので出力せずaptキー管理ユーティリティー apt-keyを使いjenkins-ci.org.keyを/etc/apt/trusted.gpgへ書き込む。

[Man page of APT-KEY](http://jnug.net/po4a/apt-key.ja.8.html)


## cURL

> cURL（カール[2]）は、さまざまなプロトコルを用いてデータを転送するライブラリとコマンドラインツールを提供するプロジェクトである。cURLプロジェクトは libcurl と cURL の2つの成果を生んでいる。

Wikipedia



# <a name="package">パッケージ管理システム</a>

## npm

node.jsパッケージ管理ツール。  
通常node.jsと一緒にインストールされる。

[npm Documentation](https://docs.npmjs.com/)

### node.js/npmインストール

下記サイトからpkgファイルをダウンロードする。

[node.js](http://nodejs.org/)

### バージョン確認
 
    $ npm —version
    1.3.25

### パス確認

    $ which npm
    /usr/local/bin/npm

### ヘルプ

    $ npm -h   // クイックヘルプ --helpというオプションはない
    $ npm -l   // display full usage info

### パッケージのインストール

    $ npm install <package name>            // カレントフォルダにインストール
    $ sudo npm install -g <package name>    // グローバルにインストール

### パッケージ一覧確認

    $ npm list       // カレントフォルダにインストールしたパッケージ
    $ npm list -g    // グローバルにインストールしたパッケージ

### パッケージの更新

    $ npm update <package name>            // カレントフォルダにインストールしたパッケージの更新
    $ sudo npm update -g <package name>    // グローバルにインストールしたパッケージの更新

### npm自体の更新

    $ npm update npm
    $ sudo npm update  npm -g


## gem

rubyパッケージ管理ツール。

### バージョン確認
 
    $ gem —version

### パス確認

    $ which gem
    /usr/bin/gem

### パッケージ一覧確認
 
    $ gem list --local  // ローカルのパッケージ表示
    $ gem list --both   // ローカル、リモート双方のパッケージを表示。

デフォルトは —localが指定。

### gemヘルプ
 
    $ gem --help     // $ gem -hでもよい
    $ gem bar -h     // 例 gem barのヘルプ
    $ gem list -h    // 例 gem listのヘルプ


### パッケージ更新

    $ sudo gem update <package name>  // gemでインストールしたパッケージの更新

### gem自体の更新
 
    $ sudo gem update --system        // gem自体の更新
 
(例) sass, compassの更新

    $ sudo gem update sass
    $ sudo gem update compass

__gemはパッケージをデフォルトでは/usr/binへインストールする？__

[そろそろ整理しておきたい、Gemコマンドの使い方 - ばくのエンジニア日誌](http://bakunyo.hatenablog.com/entry/2013/05/23/%E3%81%9D%E3%82%8D%E3%81%9D%E3%82%8D%E6%95%B4%E7%90%86%E3%81%97%E3%81%A6%E3%81%8A%E3%81%8D%E3%81%9F%E3%81%84%E3%80%81Gem%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%81%AE%E4%BD%BF%E3%81%84%E6%96%B9)


## apt

Debian系パッケージ管理システム。

## apt-get

aptを利用したパッケージ管理ユーティリティー。

## dpkg
 
    $ dpkg -l                  // Debian系インストール済パッケージ一覧を表示
    $ dpkg -L <package name>   // 指定したパッケージがインストールされたパスを表示
    
## deb

## rpm

> 米Red Hat社が開発したパッケージ管理システム。パッケージのインストールやアンインストールを行う。インストールの際は依存関係のあるパッケージ(動作に必要な他のパッケージ）を調べ，もしシステムにそれらパッケージが存在しない場合は，その旨をユーザーに通知する。また，アンインストールする際に，そのパッケージが他パッケージと依存関係がある(他のパッケージの動作に必要とされる)場合には，アンインストールできない旨を通告する。さらに，既にシステムにインストール済みのパッケージをチェックすることも可能。パッケージのインストール，アンインストール，アップデートには管理者権限が必要。
 
[Linuxコマンド集 - 【 rpm 】 RPMパッケージをインストール/アンインストールする：ITpro](http://itpro.nikkeibp.co.jp/article/COLUMN/20060227/230875/)


## yum

> Yellowdog Updater Modified (Yum ヤム)はLinuxのRPM Package Managerのパッケージを管理するメタパッケージ管理システムである。Yumは デューク大学のLinux@DUKEプロジェクトでセス・ヴィダル（英語版）を始めとするボランティアによって開発された。

Wikipedia

yumはパッケージの置き場であるリポジトリを登録する必要がある。


## Bundler

rubyの依存関係管理パッケージ管理ツール。

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

## ツール

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
* デプロイ
    + grunt-sftp-deploy
    + CircleCI
* 自動化ツール Grunt

## 継続的開発

各ツールをGruntで自動化する。

## フォルダ構成の例

以下の説明では下記フォルダ構成を前提とする。

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
         |
         |— package.json         // Gruntパッケージファイル
         |
         |— Gruntfile.js         // Grunt設定ファイル
         |
         |— contrib.rb           // Compass設定フィアル



## Sass

CSSプリプロセッサー。

[Sass: Syntactically Awesome Style Sheets](http://sass-lang.com/)

### Sassインストール

    $ sudo gem install sass

### コンパイル

style.scssをコンパイルし同フォルダにstyle.cssを作成する例。

    $ sass style.scss style.css

### 変更を監視して自動コンパイル

    $ sass --watch style.scss:style.css

ctrl + Cで監視を停止する。

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

### 初期化

    $ compass create --bare
    
contrib.rbとsassフォルダが作成される。


### config.rb(Compass設定ファイル)

    $ cd /path/to/dev
    $ compass create --bare


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


## SassDoc

[SassDoc](http://sassdoc.com/)

### インストール

    $ sudo npm install sassdoc -g
    
__npmのバージョン2.5.1で下記コマンドを実行するとsudo: npm: command not foundというメッセージが表示され  
それ以降、npmコマンドが使えなくなりnode.jsを再ストールした。__

    $ sudo npm install npm@latest -g



## Grunt

[Grunt: The JavaScript Task Runner](http://gruntjs.com/)

gruntは一般的にgrunt-cliのみグローバルへインストールし、grunt本体も含めて各プラグインはプロジェクトごとのフォルダへインストールする。

### 1. node/npmのインストール

[node.js](http://nodejs.org/)

Gruntはnode.js/npmを使う。

node.jsはサイトからpkgファイルをダウンロードしインストールする。  
npmはnode.jsと一緒にインストールされる。

### 2. grunt command line interface(grunt-cli)をグローバルへインストール

    $ sudo npm install -g grunt-cli

### 3. プロジェクトフォルダにpackage.jsonを作成

    {
      "name": "example",
      "version": "0.0.1"
    }

### 4. プロジェクトフォルダにGrunt本体とプラグインをインストール

grunt本体とプラグインはプロジェクトごとにプロジェクトフォルダへインストールする。

#### 4-1. Grunt本体のインストール

    $ cd /path/to/project
    $ npm install grunt --save-dev

/path/to/projectは上記フォルダ構成の例でははdevのパス。  
–save-devをつければpackage.jsonのdevDependenciesへプラグイン情報が追記される。

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
    $ npm install grunt-sassdoc --save-dev 

    {
      "name": "example",
      "version": "0.0.1",
      "devDependencies": {
        "grunt": "^0.4.5",
        "grunt-contrib-compass": "^0.4.1",
        "grunt-contrib-cssmin": "^0.11.0",
        "grunt-contrib-qunit": "^0.5.2",
        "grunt-contrib-uglify": "^0.2.7",
        "grunt-contrib-watch": "^0.5.3",
        "grunt-contrib-yuidoc": "^0.5.2",
        "grunt-styledocco": "^0.2.1",
        "grunt-sassdoc": "^2.0.0"
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
    $ grunt taskname


### package.jsonをもとにしたプラグインのインストール

    $ npm install

既存のnode_modulesフォルダがあれば削除してする。


### プラグインのバージョンアップ

    $ npm update —save-dev

すべてのプラグインがアップデートされる。



## StyleDocco

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

### SFTPを使ったデプロイ

[thrashr888/grunt-sftp-deploy · GitHub](https://github.com/thrashr888/grunt-sftp-deploy)

[目次へ戻る](#index)




# <a name="ubuntu_nginx_mysql_php">Ubuntu+Nginx+MySQL+PHP開発環境</a>

AWS上にUbuntu + Nginx + MySQL + PHPの開発環境を構築することを目指す。

## 目次

* [Ubuntu](#ubuntu)
    + [シェル](#shell)
    + [Vi](#vi)
    + [ユーザーとパーミション](#user)
    + [ドキュメントルート](#documentroot)
    + [Nginx, MySQL, PHPインストール](#install_nginx_mysql_php)
* [Nginx](#nginx)
* [PHP](#php)
    + [PHP実行環境の分類](#php_exe)
    + [php.ini保存場所の確認](#php_ini)
    + [文字コード](#php_character)
    + [実行環境の例](#php_exe_sample)
    + [実行ユーザーの確認](#php_user)
    + [PECL](#php_pecl)
    + [Composer](#php_composer)
    + [単体テスト - PHPUnit](#php_test)
    + [デバッグ - Xdebug](#php_debug)
    + [ドキュメンテーション - phpDocumentor](#php_documentation)
    + [コードインスペクション - PHP_CodeSniffer](#php_inspection)
    + [CakePHPのインストール](#php_cakephp)
* [MySQL](#mysql)
* [Postfixでメール送信](#postfix)
* [Git](#git)

## 構築環境

* Ubuntu 14.04 
* Nginx  
  nginx version: nginx/1.4.6 (Ubuntu)
* MySQL  
  5.5.40-0ubuntu0.14.04.1
* PHP5   
  PHP Version 5.5.9-1ubuntu4.5


## <a name="ubuntu">Ubuntu</a>

### Ubuntuログインユーザー

ユーザーubuntuでログインしていると仮定して記載する。


### ログ

    /var/log



## <a name="shell">シェル</a>

### 使用シェルを確認

    $ echo $SHELL

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

### GitのエディタをnanoからViへ変更

    $ git config --global core.editor "vi"

[gitのエディタをnanoから他へ変更する | J-Linuxer](http://jlinuxer.dip.jp/?p=645)



## <a name="install_nginx_mysql_php">Nginx + MySQL + PHPインストール</a>

Nginx, MySQL, PHP5の環境を構築するのに必要なパッケージをインストールする。

    // apt-getを最新へ更新
    $ sudo apt-get update
    // 必要パッケージインストール
    $ sudo apt-get install php5 php5-cli php5-fpm php5-mysql php-pear php5-curl php5-dev php-apc php5-xsl php5-mcrypt mysql-server-5.5 nginx



## <a name="documentroot">ドキュメントルート作成</a>

    $ sudo mkdir -p /var/www/application/current/app/webroot

## <a name="user">ユーザーとパーミション</a>

### PHP実行ユーザー

以下PHP実行ユーザー/グループはwww-dataとして記載する。

* ユーザー  
  www-data
* www-dataが属するグループ  
  www-data

### PHP実行ユーザー確認

    <?php
    echo `whoami`;

以下PHP実行ユーザーはwww-dataと仮定する。


### currentディレクトリ以下の所有者/グループ/パーミション変更
 
    $ cd /var/www/application
    $ sudo chown -R www-data current
    $ sudo chgrp  -R www-data current
    $ sudo chmod -R 775 current
    
### currentディレクトリ以下のグループを変更


### ubuntuユーザーをwww-dataグループへ追加

    $ sudo usermod -G www-data ubuntu


## <a name="env_nginx">Nginx</a>

### プログラム

    $ which nginx
    /usr/sbin/nginx


### 起動・停止・再起動

    $ sudo service nginx start
    $ sudo service nginx stop
    $ sudo service nginx restart

またserviceコマンドを使わない方法が下記で紹介されている。

[ubuntuでnginxの起動と最低限のコマンド](http://joppot.info/2014/03/10/970)



### 設定関連ファイル

    (1) /etc/nginx/nginx.conf              // Nginx全体の設定
    (2) /etc/nginx/sites-available/default // ホストの設定(変更)
    (3) /etc/nginx/sites-enabled/default   // (2)のシンボリックリンク Nginxの起動時に読み込まれる

今回は/etc/nginx/sites-available/defaultを編集する。


### 設定ファイルの記載法

Nginxはディレクティブで設定を行う。  
ディレクティブは1行のものと数行にわたるブロックの2種種類に分けることができる。  
ディレクティブは入れ子にできる。

    # 1行のディレクティブの例
    sendfile on;

    # ブロックディレクティブの例
    http {
        ..... 
        sendfile off; # 入れ子
        .....
        server {
            listen 80 default_server;
            ......
        
        }
        .....
     }

[Beginner’s Guide](http://nginx.org/en/docs/beginners_guide.html)


### site-available/defaultファイル

* ドキュメントルート設定  
  rootディレクティブ
* /でアクセスしたときの制御  
  indexディレクティブ
* FastCGI設定  
  locationディレクティブ

    server {
            listen 80 default_server;
            listen [::]:80 default_server ipv6only=on;
 
            root /var/www/application/current/app/webroot;
            index index.php index.html index.htm;
 
            # Make site accessible from http://localhost/
            server_name example.com;
 
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

### sendfileディレクティブ

Vagrantのconfig.vm.synced_folderに設定したフォルダーのファイルを直接開いてJavaScriptを実行するとエラーは発生しないが、
IPアドレスでアクセスし実行すると下記のようなエラーが発生した。

    uncaught syntaxerror: unexpected end of input
    Uncaught SyntaxError: Unexpected token ILLEGAL

確認するとブラウザはキャッシュを行わない設定にしているが最新のJavaScriptを読み込んでいなかった。

Nginxのsendfile(/etc/nginx/nginx.conf)ディレクティブをoffにしたら解決した。

sndfileディレクティブがonのときNginxはsendfile()APIを使いカーネルにキャッシュしているデータを送信する。


[VagrantでCSSの更新が反映されない場合の対処法 - Qiita](view-source:http://qiita.com/shoyan/items/12389d5beaa8695b1a53)  
[Vagrant上のjavascriptで「Uncaught SyntaxError: Unexpected token ILLEGAL」 - Qiita](http://qiita.com/shibukk/items/07bb794981fe2d0daf7c)  
[nginx連載3回目: nginxの設定、その1 - インフラエンジニアway - Powered by HEARTBEATS](http://heartbeats.jp/hbblog/2012/02/nginx03.html)


### Nginx(再)起動

    $ sudo nginx -s reload

[軽量で高速なウェブサーバNginxを、Ubuntu 12.04に導入する(設定編その１) | 近藤嘉雪のプログラミング工房日誌](http://blog.kondoyoshiyuki.com/2012/12/09/setting-1-nginx-on-ubuntu-12-04/)


### ログ

nginx.confで設定する。

    ##
    # Logging Settings
    ##

    access_log /var/log/sginx/access.log;
    error_log /var/log/nginx/error.log;

[nginxのログ出力変更 - Qiita](http://qiitj.com/hito3/items/0e539e82ee3c410cccf1u)



## <a name="php">PHPの開発環境</a>

### <a name="php_exe">実行環境の分類</a>

* Apache + モジュール/CGI
    - モジュール  
      Apache + php_mod: Apacheのモジュールとして動かす。
    - CGI  
      Apach + php-cgi: ApacheからCGIとして呼び出す。
* Nginx + php-fpm(以下この構成を前提として記載する)  
  [PHP: FastCGI Process Manager (FPM) - Manual](http://php.net/manual/ja/install.fpm.php)  
  NginxからCGIとして呼び出す。
* CLI(Command Line Interface)  
  コマンドで実行する。


### <a name="php_ini">php.ini保存場所確認</a>

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
    error_log = /path/to/logfile


### <a name="php_composer">Composer</a>

PHPパッケージ管理ツール。


#### Composerインストール

    $ curl -sS https://getcomposer.org/installer | php

composer.pharがカレントディレクトリにダウンロードされる。
ダウンロードしたcomposer.pharは通常composerへ名前を変更しパスの通っているディレクトリへ移動する(例 /usr/local/bin)。

パスが通っていればコマンドは下記のように実行できる。

    $ composer --version

パスが通っていないときはcomposer(名前を変更していないときはcomposer.phar)コマンドは下記のように実行する。

    $ php /fullpath/composer --version

#### Composer初期化

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

#### パッケージ追加
  
    // 通常の追加
    $ composer require packagename
    // 開発でのみ必要なパッケージの追加
    $ composer require --dev packagename
 
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

#### テスト実行

    $ vendor/bin/phpunit test/CalcTest

#### --bootstrapオプション

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


### <a name="php_documentation">ドキュメンテーションツール - phpDocumentor</a>

[phpDocumentor](http://www.phpdoc.org/)

#### Composerを使いインストール

    $ composer require "phpdocumentor/phpdocumentor:2.*"

[Installing Using Composer](http://www.phpdoc.org/docs/latest/getting-started/installing.html#using-composer)


    $ phpdoc -d src -t output


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


### <a name="php_cakephp">CakePHP</a>

#### インストール

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

#### 拡張モジュールmcryptエラー

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

#### プロジェクト作成

    ubuntu@xxx:/var/www/application/current/app$ Vendor/bin/cake bake project
    
    Welcome to CakePHP v2.6.1 Console
    ---------------------------------------------------------------
    App : app
    Path: /var/www/application/current/app/
    ---------------------------------------------------------------
    What is the path to the project you want to bake?  
    [/var/www/application/current/app/myapp] > /var/www/application/current/app
    Skel Dir/ectory: /var/www/application/current/app/Vendor/cakephp/cakephp/lib/Cake/Console/Templates/skel
    Will be copied to: /var/www/application/current/app
    ---------------------------------------------------------------
    Look okay? (y/n/q) 
    .....
    .....


## <a name="mysql">MySQL</a>

UbuntuにMySQLをインストールしている前提で記載する。

MySQLでは識別子(テーブル名やカラム名)が予約語を含むときはバッククォートで囲む必要がある。

    EXPLAIN hoge    // hogeは予約語でないのでバッククォートは必要ない。
    EXPLAIN `table` // 仮にtableテーブルを作成しているときの例。

### 起動・停止・再起動

    // 起動
    $ sudo /etc/init.d/mysql start
     
    // 停止
    $ sudo /etc/init.d/mysql stop
     
    // 再起動
    $ sudo /etc/init.d/mysql restart

    // 起動確認
    $ mysqladmin -u <username> -p ping
    Enter Password // パスワード入力
    mysqld alive
    
    // プロセス確認
    $ ps aux | grep mysqld

[MySQL サーバが起動中であるかを確認する - MySQL 逆引きリファレンス](http://mysql.javarou.com/dat/000409.html)
 

### バージョン

    $ mysqld --version
    mysql> SELECT version();
    

### mysqlクライアントへログイン

mysqlクライアントはオプションの指定が方法がいくつかある。  
ユーザー名 foo, パスワードbarの場合を記載する。

    $ mysql -u foo -p
    // Enter Password    <-- barを入力
    $ mysql -ufoo -pbar
    $ mysql --user=foo --password=bar

### 設定ファイル(my.cnf)

my.cnfのパスは通常/etc/mysql/my.cnfとなる。

    $ sudo find / -name my.cnf
    /etc/mysql/my.cnf

my.cnyの確認

   $ cat /etc/mysql/my.cnf

### mysql.sock, mysql.pidファイル

5.5.40-0ubuntu0.14.04.1では下記パスになる(my.cnfに記載されている)。

	var/run/mysqld/mysqld.pid
	var/run/mysqld/msyql.sock

### ユーザー管理

#### ユーザー作成(ユーザーmyuser)

    mysql> GRANT ALL PRIVILEGES ON *.* TO myuser IDENTIFIED BY 'passw0rd';

#### パスワード設定

    // MySQLログインユーザーのパスワード設定
    mysql> SET PASSWORD = PASSWORD('passw0rd');
    // 他ユーザーのパスワード変更
    SET PASSWORD FOR 'other'@'localhost' = PASSWORD('passw0rd');

#### ユーザーの確認

    mysql> USE mysql;
    mysql> SELECT user, host FROM user;

| user             | host             |
|------------------|------------------|
| myuser           | %                |
| root             | 127.0.0.1        |
| root             | ::1              |
| root             | ip-xx-xxx-xxx-xx |
| debian-sys-maint | localhost        |
| root             | localhost        |

myuserはホスト名を指定せずに作成した。  
ホスト名の指定がないときはすべてのホストからアクセス可能であるワイルドカード%が指定される。

    mysql> GRANT ALL PRIVILEGES ON *.* TO myuser IDENTIFIED BY 'passw0rd';
    mysql> GRANT ALL PRIVILEGES ON *.* TO `myuser`@`%` IDENTIFIED BY 'passw0rd';

#### ユーザーの削除

    mysql> DROP USER username@hostname

[Ubuntu+Nginx+MySQL+PHP開発環境の目次へ戻る](#ubuntu_nginx_mysql_php)


### MySQLの文字コード関連確認

    mysql> show variables like 'char%';

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
    ..........
    character-set-server = utf8
    ..........
    # 上記を誤ってdefault-character-set = utf8と記載してmysqldが起動しなくて困った。

    [mysqldump]
    default-character-set = utf8

    [mysql]
    default-character-set = utf8


文字コード確認。

    mysql> SHOW VARIABLES LIKE 'char%';

| Variable_name            | Value                                      |
---------------------------|--------------------------------------------|
| character_set_client     | utf8                                       |
| character_set_connection | utf8                                       |
| character_set_database   | utf8                                       |
| character_set_filesystem | binary                                     |
| character_set_results    | utf8                                       |
| character_set_server     | utf8                                       |
| character_set_system     | utf8                                       |
   

以上の設定にすると個別のデータベースの作成で CHARACTER SET utf8を付けなくてもデフォルトでUTF8になる。

### UTF8でデータベース作成

    // 新規
    CREATE DATABASE <database name> CHARACTER SET utf8
    // 変更
    ALTER DATABASE <database name> CHARACTER SET utf8
    
### データベースの文字コード確認

    mysql> SHOW CREATE DATABASE <database name>;

### テーブル作成情報

    mysql> SHOW CREATE TABLE <table name>;

### 文字コード(UTF8)を指定してテーブルを作成

    CREATE TABLE tablename (
        ; 定義
    ) default character set utf8;

### テーブル定義表示

    SHOW CREATE TABLE <table name>;

tablenameの定義が表示される。ALTER TABLEで変更した内容も反省されている。

### テーブル情報

    DESC <table name>        // またはSELECTなのど文
    EXPLAIN <table name>     // 

### ダンプ

    $ mysqldump -u <user name> -p <database name> > /path/to/dumpfile.sql
    // タイムスタンプを利用する場合
    $ mysqldump -u <user name> -p <database name> > /path/to/$(date "+%Y%m%d%H%M%S").sql
    // 20150101125959.sql 2015年1月1日12時59分59秒

### インポート

    $ mysql -u username -p --database=<database name> --host=<host name> < /path/to/dumpfile.sql
    
### 問題点と解決

#### 問題

__CakePHPで上述のようにデータベース、テーブルをUTF8で作成したがブラウザ確認は文字化けはせずmysqlクライアントで確認すると文字化けが発生した。__  


#### 解決 

CakePHPのデータベース設定の問題だった。

	class DATABASE_CONFIG {
	
		public $default = array(
			'datasource' => 'Database/Mysql',
			'persistent' => false,
			'host' => '127.0.0.1',
			'login' => 'example',
			'password' => 'passw0rd',
			'database' => 'example',
			'prefix' => '',
			'encoding' => 'utf8',       // この記載を追加したらUTF8で保存できるようになった。
		);
	
		public $test = array(
			'datasource' => 'Database/Mysql',
			'persistent' => false,
			'host' => '127.0.0.1',
			'login' => 'example',
			'password' => 'passw0rd',
			'database' => 'example_test',
			'prefix' => '',
			'encoding' => 'utf8',      // この記載を追加したらUTF8で保存できるようになった。
		);
	}



### AWS RDS

AWSはEC2インスタンスへMySQLをインストールし利用することができる。  
またAWS RDSを使いMySQLサーバーを独立して構築することもできる。

### 接続確認 mysql_test.php

    <?php
        $url = "endpoint";    // RDSでMySQLのインスタンスを作成した際にインスタンスのパネルに表示
        $user = "username";   // RDSでMySQLのインスタンスを作成する際に設定
        $pass = "passw0rd";   // RDSでMySQLのインスタンスを作成する際に設定
        $db = "dbname";       // RDSでMySQLのインスタンスを作成する際に設定

        // 接続
        $link = mysql_connect($url,$user,$pass) or die("接続失敗。");

        // DB選択
        $sdb = mysql_select_db($db,$link) or die("DB選択失敗。");

        // クエリ送信する
        $sql = "SELECT * FROM tablename";  // tablenameは作成済みでデータを何件か挿入済みとする
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


### MySQLが起動できない問題と対応

下記エラーが発生した。

    mysql -u username -p
    Enter password: 
    ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)

MySQLサーバーを下記の通り停止・起動したが起動できなかった。


    $ sudo /etc/init.d/mysql stop           [OK]
    $ sudo /etc/init.d/mysql start          [fail]

ログファイル(/var/log/mysql/error.log)に以下のように記載されていた。
    
    InnoDB: Unable to lock ./ibdata1, error: 11
    InnoDB: Check that you do not already have another mysqld process

killコマンドでmysqlプロセスを終了したら起動できた。

    $ ps aux | grep mysql
    mysql    14065  0.0 11.8 623916 44216 ?        Ssl  05:00   0:01 /usr/sbin/mysqld
    $ sudo kill -9 14065

    $ sudo /etc/init.d/mysql start         [OK]
     
[mysqlの起動に失敗（MySQL Daemon failed to start）](http://www.crossl.net/blog/mysql_failed_start/)



## <a name="postfix">Postfixでメール送信</a>

### Postfixインストール

    $ sudo apt-get install postfix
    // バージョン確認
    $ /usr/sbin/postconf | grep mail_version
    mail_version = 2.11.0

[Postfixのバージョンを確認する | CoDE4U](http://blog.code4u.org/archives/1135)


### 設定ファイル(/etc/postfix/main.cf)

下記記事を参考にして契約プロバイダーのSMTPを経由して送信する設定を行った。  
[『Varying Vagrant Vagrants』から外部へのメール送信を設定したよ | 鉄王](http://www.tecking.org/archives/3663)

    relayhost = [mail.example.com]:587                         # リレー先
    smtp_sasl_auth_enable = yes                                # 追加
    smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd    # 追加
    smtp_sasl_mechanism_filter = CRAM-MD5                      # 追加

/etc/postfixにsasl_passwdを作成し下記内容を記載。

    mail.example.com account:password

sasl_passwdのパーミションを変更を変更する。

    $ sudo chmod 600 sasl_passwd

postmapコマンドでdbファイルを作成する。

    $ sudo postmap /etc/postfix/sasl_passwd

sasl_passwd.dbが作成される。  
Postfixを再起動する。
    

### postfix 再起動

    $ sudo /etc/init.d/postfix restart
    // または
    $ sudo service postfix reload


### mailコマンドで送信確認

#### インストール

    $ sudo apt-get install mailutils

試した環境ではmailコマンドの終了は.ではなくエンター + Ctrl + D。


### メールログ

    /var/log/mail.log
    /var/log/mail.err



## <a name="git">Git</a>

GitをインストールしGitHubのソースを取得する。

1. UbuntuへGitをインストールする。
2. 公開鍵/秘密鍵を作成する。
3. GitHubへ公開鍵を設置する。
4. GitHubのリポジトリからソースを取得する。

### 1 Gitインストール

    $ sudo apt-get install git

### 2. 公開鍵/秘密鍵作成

ホームディレクトリでssh-keygenコマンドを実行する。

    $ cd /path/to/home
    $ ssh-keygen
    // パスフレーズは入力しない。

ホームディレクトリ直下の.sshディレクトリに下記ファイルが作成される。

    authorized_keys
    id_rsa
    id_rsa.pub

### 3. 公開鍵をGitHubへ設置

GitHub > Settings > SSH keys > Add SSH key

### 4. リポジトリ取得テスト

    $ mkdir gittest
    $ cd gittest
    $ git init
    $ git remote add origin git@github.com:xxx/yyy.git
    $ git pull origin master
    // 初回は下記メッセージが表示
    The authenticity of host 'github.com (xxx.xxx.xxx.xxx)' can't be established.
    RSA key fingerprint is xxx.xxx
    Are you sure you want to continue connecting (yes/no)? 
    yes


[目次へ戻る](#index)



# <a name="ci">継続的インテグレーション</a>

* [環境構築](#ci_dev)
	+ [Virtualbox 仮想化ソフト](#ci_virtualbox)
	+ [Vagrant 仮想開発環境構築ソフトウェア](#ci_vagrant)
	+ [Chef プロビジョニングツール](#ci_chef)
	+ [Vagrant, Chefを使ったプロビジョニング手順](#ci_vagrant_chef)
* [CI(継続的インテグレーション)](#ci_ci)
* [Jenkins CIサーバー](#ci_jenkins)
* [CircleCi クラウド型CIサーバー](#ci_circleci)
* [デプロイ処理](#ci_deploy)
* [CakePHP開発環境](#env_cakephp)
* [開発手法](#ci_process)
	* [アジャイル](#agile)
	* [BDD:振舞駆動開発 (開発手法)](#bdd)


以下、次の書籍の読書メモを記載している。

[「CakePHPで学ぶ継続的インテグレーション」 渡辺 一宏, 吉羽 龍太郎, 岸田 健一郎, 穴澤 康裕, 丸山 弘詩  (編集)](http://www.amazon.co.jp/CakePHP%E3%81%A7%E5%AD%A6%E3%81%B6%E7%B6%99%E7%B6%9A%E7%9A%84%E3%82%A4%E3%83%B3%E3%83%86%E3%82%B0%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3-%E6%B8%A1%E8%BE%BA-%E4%B8%80%E5%AE%8F/dp/4844336789/ref=tmm_pap_title_0?ie=UTF8&qid=1421710653&sr=8-1)

__引用のページ番号は上記書籍の該当ページを指す。__



## <a name="ci_dev">環境構築</a>

下記ソフトを使いCIのための環境を構築する。

* VirtualBox
* Vagrant
* Chef


### <a name="ci_virtualbox">VirtualBox 仮想化ソフトウェア</a>

代表的な仮想化ソフトウェア。

* VirtualBox
* VMware

仮想化ソフトのゲストOSを開発環境として利用する。


### <a name="ci_vagrant">Vagrant 仮想開発環境構築ソフトウェア</a>

仮想化ソフトウェアの制御やゲストOSのプロビジョニングを行う。  
代表的な仮想開発環境構築ソフトウェアであるVagrantについて記載する。

> Vagrant（ベイグラント）は、FLOSSの仮想開発環境構築ソフトウェア[1]。VirtualBoxをはじめとする仮想化ソフトウェアやChef（英語版）やSalt（英語版）、Puppetといった構成管理ソフトウェアのラッパーとみなすこともできる。

Wikipedia

#### Vagrantの役割

1. 仮想サーバの起動・終了
2. ホストOSとゲストOSでディレクトリ共有  
  (例) develop.vm.synced_folder “application”, "/var/www/application/current", (Vagrantfile)
3. 仮想サーバのプロビジョニング  
  Knife-solo, berkshelf

[Command-Line Interface - Vagrant Documentation](http://docs.vagrantup.com/v2/cli/)

#### Vagrantのボックス追加

    $ vagrant box add [options] <name, url, or path>
 
(例) Bentoのopscode-ubuntu-14.04の場合

    $ vagrant box add opscode-ubuntu-14.0.4 http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-14.04_chef-provisionerless.box

#### Vagrant初期化

    $ vagrant init

#### Vagrant 起動・休止・シャットダウン・削除

    $ vagrant up                 // 起動
    $ vagrant halt               // シャットダウン
    $ vagrant destroy            // 削除
    $ vagrant reload             // 再起動
    $ vagrant provision          // プロビジョンのみ行う

#### SSH接続

    $ vagrant ssh

#### vagrant プラグインの導入

    $ vagrant plugin install sahara
    $ vagrant plugin install vagrant-omnibus
    $ vagrant plugin install vagrant-cachier

#### vagrant plugin installで発生したエラーへの対応

##### エラーメッセージ

> Bundler, the underlying system Vagrant uses to install plugins,
> reported an error. The error is shown below. These errors are usually
> caused by misconfigured plugin installations or transient network
> issues. The error from Bundler is:

> An error occurred while installing nokogiri (1.6.5), and Bundler cannot continue.
> Make sure that `gem install nokogiri -v 1.6.5 succeeds before bundling.

##### 解決策

    $ gem install --install-dir ~/.vagrant.d/gems nokogiri -v '1.6.5'

[vagrant pluginをインストールしようとするとnokogiriエラーがでてインストールできない時の対策 | hypermkt blog](http://blog.hypermkt.jp/can-not-install-vagant-plugin-by-nokogiri/)

#### Vagrantでプロビジョニングが実行されるタイミング

Vagrantは仮想サーバーを起動したときや指定したタイミングでプロビジョニングを行うことができる。


### <a name="ci_chef">Chef プロビジョニングツール</a>

Chefは構成管理ツール/プロビジョニングツール。

> プロビジョニング（英: Provisioning）は、本来は「準備、提供、設備」などの意味であり、現在では通常、音声通信やコンピュータなどの分野における、ユーザーや顧客へのサービス提供の仕組みを指す。

Wikipedia

今回、Chef Client, Chef SoloをVagrantプラグインvagrant-omnibusを使って導入する。  
設定はVagrantfileに記載する。

#### プロビジョニングツール

* Chef(Chef Solo, Chef Server)  
  [Chef | IT automation for speed and awesomeness | Chef](https://www.chef.io/chef/)
* knife-solo  
  Chef Soloのリポジトリを作成する。  
  [knife-solo](http://matschaffer.github.io/knife-solo/)
* Berkshelf  
  Chefのクックブックとその依存関係を管理する。  
  BerkshelfをBundlerを使いインストールしたとき下記コマンドでエラーが発生した。  
  Bundlerを使わずChefDKを利用することで解決した。  
  $ bundle exec berks vendor ./cookbooks
* ChefDk  
  Chef関連のツール集(Berkshelf, Knife-soloなど)。  
  [Chef Development Kit | Chef Downloads | Chef](https://downloads.chef.io/chef-dk/)
 

Bundlerでエラーが発生した下記処理もChefDKで解決した。

    $ bundle exec knife cookbook create name -o site-cookbooks

#### ChefDk導入後

    $ knife cookbook create name -o site-cookbooks


### <a name="ci_vagrant_chef">Vagrant, Chefを使ったプロビジョニング手順</a>

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



## <a name="ci_ci">CI(継続的インテグレーション)</a>


継続的インテグレーションでは開発、CI、デプロイ(公開)サーバーを使う。    
バージョン管理システム(Git)を使い開発、CI、デプロイで同じコードを共有する。

CIサーバーは主にユニットテスト、ビルドを自動化する。


### <a name="ci_ci_server">本記事サーバー構成</a>

標準的なCIは3台のサーバーを使う。

* 開発サーバー   
  192.168.33.10
* CIサーバー  
  192.168.33.100
* デプロイサーバー  
  192.168.33.200


### ディレクトリ構造

    // Vagrantfile
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
      |-- deploy // デプロイ Capistorano
      |-- features // BDD
      |-- build.xml



### CIの流れ

1. 開発サーバーのソースコードをGitHubで管理する。
2. CIサーバーはGitHubからソースコードを定期的にPULLし自動テストを行う。  
  自動化処理はJenkinsを使う。
3. CIサーバーでデプロイ処理を行う。  
  デプロイツールで有名なものにFabric/Capistranoがある。


### CIツール

* 継続的インテグレーションツール
    + Jenkins
* JavaScript/HTML/CSS自動化ツール
    + Grunt
* テストツール
    + PHPUnit(単体テストツール)
    + Behat(BDD駆動開発用テストツール)
* コードインスペクション(コード検査ツール)
    + PHP_CodeSniffer
* ドキュメンテーションツール
    + phpDocumentor




## <a name="ci_jenkins">Jenkins CIサーバー</a>

[Welcome to Jenkins CI! | Jenkins CI](http://jenkins-ci.org/)  
[capistrano/capistrano](https://github.com/capistrano/capistrano)

### インストール

[Installing Jenkins on Ubuntu - Jenkins - Jenkins Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu)

    $ wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
    $ sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
    $ sudo apt-get update
    $ sudo apt-get install jenkins

最初は[Debian Repository for Jenkins](http://pkg.jenkins-ci.org/debian/)ページに記載してある方法でインストールしたが下記エラーがでた。

    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    Package jenkins is not available, but is referred to by another package.
    This may mean that the package is missing, has been obsoleted, or
    is only available from another source

ページ内のリンク「See Wiki for more information, including notes regarding upgrade from Hudson.」から[Installing Jenkins on Ubuntu - Jenkins - Jenkins Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu)を開き、記載してある方法を実行したら正常にインストールできた。


### Linuxユーザー jenkins

Jenkinsをインストールするとjenkinsユーザーが作成される。パスワードを設定する。

    $ sudo passwd jenkins
    Enter new UNIX password: 
    Retype new UNIX password


### CI用の秘密鍵、公開鍵を作成し公開鍵をGitHubへ設置

    $ sudo su - jenkins
    $ ssh-keygen


/var/lib/jenkins/.ssh/にid_rsa、id_rsa.pubが作成される。公開鍵であるid_rsa.pubをGitHubへ設定する: 
    

### GIT Pluginをインストール

JenkinsのWEBインターフェース > Jenkinsの管理 > プラグインの管理からGIT Pluginをインストールする。


### GitのPULL設定

プロジェクト > 設定 > ソースコード管理でGitを選択しリポジトリの設定を行う。

#### Add Credentials

|設定項目|設定|
|-----|-----|
|kind|SSHユーザー名と秘密鍵を選択する|
|スコープ|グローバル|
|ユーザー|jenkins|
|秘密鍵|Jenkinsマスター上の~/.sshから|


##### ビルド・トリガ

SCMをポーリングを選択しスケジュールに下記を記載する(15分毎にGitHubへポーリングする)。

     H/15 * * * *



### ソースコードがPULLされる場所

プロジェクト名がexampleの場合、デフォルトではソースコードは下記ディレクトリにcloneされる。

	/var/lib/jenkins/jobs/example/workspac



### Git Pluginの導入

Jenkinsの設定 > プラグイン

### Jenkinsで単体テストを行う

1. Phingで設定ファイルbuild.xmlにテストの自動化を記載
2. Jenkinsのビルド設定

### Phing

JenkinsでPHPの自動テストなどのビルドを行うツール。

    $ cd /var/www/application/current/app
    $ composer require --dev "phing/phing:~2.8"
    
ビルドの設定は/application/build.xmlに記載する。

### Jenkinsの設定

プロジェクト > 設定 > ビルド > シェルの実行

	cd ${WORKSPACE}/application/app
	composer install --dev
	cd ${WORKSPACE}/application
	app/Vendor/bin/phing -logger phing.listener.DefaultLogger



## <a name="ci_circleci">CircleCI CIサーバー(クラウド型)</a>

CircleCIは下記機能を提供するクラウド型CI・デプロイサーバー。

* 自動テスト  
  デフォルトはmasterブランチへプッシュすると自動テストを実行する。
* 自動デプロイ  
  テスト成功をフックに自動デプロイを実行する。
  

### 設定ファイル

circle.ymlにPushしたとき実行する処理をYMLで記載しプロジェクト直下に設置する。  
circle.ymlの記載は下記リンクを参考にする。

[Continuous Integration and Deployment](https://circleci.com/docs/configuration)  
[CircleCI経由でRailsアプリをデプロイ - Qiita](http://qiita.com/ysk_1031/items/f584a0599791bdba132a)


### 自動テスト

#### 躓いた点1 ComposerのインストールでGitHubトークンが必要になった点

	A token will be created and stored in "/home/ubuntu/.composer/auth.json", your password will never be stored
	To revoke access to this token you can visit https://github.com/settings/applications
	Username:

GitHub > Settings > Applications > Personal access tokensでトークンを取得する。  
取得トークンを記載したauth.jsonをcomposer.jsonと同じディレクトリに作成する。

	{
	  "github-oauth": {
		"github.com": "<token>"
	  }
	}

#### 躓いた点2. Console/cake testでDBへ接続できなかった点

    Database connection "Mysql" is missing, or could not be created.
    
Config/database.phpのhostをlocalhostから127.0.0.1へ変更した。

#### 躓いた点3 テスト用テーブル

    Table products for model <modelname> was not found in datasource default.

ExampleFixture.phpファイルの記載をテーブル定義、レコードが記載されたファイルへ変更した。

    // 変更前
    public $import = array('model' => 'Example', 'records' => true);
    
    // 変更後
    public $fields = array(
    		'id' => array('type' => 'integer', 'null' => false, 'default' => null, 'unsigned' => false, 'key' => 'primary'),
    		'title' => array('type' => 'string', 'null' => false, 'default' => null, 'collate' => 'utf8_unicode_ci', 'charset' => 'utf8'),
    		'context' => array('type' => 'text', 'null' => true, 'default' => null, 'collate' => 'utf8_unicode_ci', 'charset' => 'utf8'),
    		.....
	    
    public $records = array(
    		array(
    			'id' => '1',
    			.....

#### テストまでのcircle.yml

##### circle.yml

	machine:
	  php:
		version: 5.5.9
	database:
	  override:
		- mysql -u ubuntu < circle-database-setup.sql
	dependencies: 
	  override:
		- bash ./circle-composer-setup.sh
	test:
	  override:
		- ./application/app/Console/cake test app Model/Example

##### circle-database-setup.sql

	GRANT ALL PRIVILEGES ON *.* TO <user> IDENTIFIED BY '<passowrd>';
	CREATE DATABASE <databasename> CHARACTER SET utf8;
	CREATE DATABASE <databasename>_test CHARACTER SET utf8;

##### circle-composer-setup.sh

composer.jsonでパッケージ管理をしパッケージの実態はGitでトラッキングしていないときは、  
毎回composer本体とomposer.jsonに従いパッケージをインストールする。

1. composerをインストール
2. パッケージをインストール


		#!/bin/bash
		
		# Move to CakePHP app directory
		cd ./application/app
		
		# Install composer
		curl -sS https://getcomposer.org/installer | php
		
		# Install package
		php composer.phar install


### 自動ドキュメンテーション作成

* PhpDocumentor
* YUIDoc


## <a name="ci_deploy">デプロイ処理</a>

有名なデプロイツールとして下記がある。  

* Fabric
* Capistrano

今回はFabricを使う(CircleCIはFabricがデフォルトでインストール済み)。

### Fabricインストール

    $ pip install fabric

[Welcome to Fabric! &mdash; Fabric  documentation](http://www.fabfile.org/index.html)

### Fabricドキュメント

[Welcome to Fabric’s documentation! &mdash; Fabric  documentation](http://docs.fabfile.org/en/1.10/index.html)

### <a name="ci_deploy_to_ec2">AWS EC2へデプロイ</a>

#### circle.yml

* cuisineモジュールインストール
* fabfile.pyでデプロイ処理定義


	.....
	dependencies: 
	  post:
		- pip install cuisine
	.....
	deployment:
	  production:
		branch: master
		commands:
		  - fab -f ./fabfile.py bootstrap deploy

#### fabfile.py

	from __future__ import with_statement
	from fabric.api import *
	from fabric.contrib.console import confirm
	import cuisine
	 
	def bootstrap():
		env.hosts = ['xxx.xxx.xxx.xxx']
		env.user = "ubuntu"
		env.key_filename = "aws_ssh_key.pem" 
		
	def deploy():
		deploy_dir = '/var'
		www_dir = '/var/www'
		application_dir  = '/var/www/application'
		if not cuisine.dir_exists(application_dir):
			with cd(deploy_dir):
				run("git clone git@github.com:s-hiroshi/minker.git www")
				sudo("chown -R www-data www")
				sudo("chgrp  -R www-data www")
				sudo("chmod -R 775 www")
				
		with cd(www_dir):
			run("git pull")



#### 現在のフロー

* デプロイ EC2
  ソフトウェアを手動でインストール
* アプリケーション
  + リモートリポジトリ GitHub
  + 開発マシン ローカル(Mac)
  + CIサーバー CircleCI
  	- テスト
  	- デプロイ
  		1. 初回 GitHubのリポジトリをclone
  		2. クローン済み git pull
  		
##### 残り

* Fabric 条件分岐
* GitHubのブランチから特定ディレクトリのみ取得する
* アプリケーション初期設定
	- データベースマイグレーション
    
### デプロイサーバーのDebugレベル

Config/core.php

	if (isset($_SERVER['WEB_APP_ENV']) && $_SERVER['WEB_APP_ENV'] == 'production') {
	  // 本番サーバー
	  Configure::write('debug', 0);
	} elseif (isset($_SERVER['WEB_APP_ENV']) && $_SERVER['WEB_APP_ENV'] == 'test') {
	  // テストサーバー
	  Configure::write('debug', 2);
	} else {
	  // ローカル
	  Configure::write('debug', 2);
	}
	Configure::write('cake_env', $cake_env);

[\[cakephp\]環境変数で本番環境と開発環境を設定する | WEBPAPRIKA](http://webpaprika.com/1636.html)


## <a name="env_cakephp">CakePHP開発環境</a>

* ユニットテスト - PHPUnit  
  CakePHPのユニットテストはPHPUnitで行う。
* データベースマイグレーション - CakeDC Migration Plugin


## PHP実行ユーザー確認

	<?php
    echo `whoami`;
    
## app/tmpディレクトリ

CakePHPが作成するキャッシュ等やセッションの一時ファイルなどapp/tmpディレクトリに作成されるファイル/ディレクトリは全てPHP実行ユーザーで作成される。

### CakePHP テスト環境

以下、本番環境のIPアドレスは192.168.33.200、テスト環境のIPアドレスは192.168.33.10とする。
またNginxのドキュメントルート(root)はwebroot設定済みとする。

* [テスト &mdash; CakePHP Cookbook 2.x ドキュメント](http://book.cakephp.org/2.0/ja/development/testing.html)
* [「CakePHPで学ぶ継続的インテグレーション」 渡辺 一宏, 吉羽 龍太郎, 岸田 健一郎, 穴澤 康裕, 丸山 弘詩  (編集)](http://www.amazon.co.jp/CakePHP%E3%81%A7%E5%AD%A6%E3%81%B6%E7%B6%99%E7%B6%9A%E7%9A%84%E3%82%A4%E3%83%B3%E3%83%86%E3%82%B0%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3-%E6%B8%A1%E8%BE%BA-%E4%B8%80%E5%AE%8F/dp/4844336789/ref=tmm_pap_title_0?ie=UTF8&qid=1421710653&sr=8-1)


#### デバッグレベル設定

app/Config/core.php

    Configure::write('debug', 2);


#### テスト用データベース設定

Config/database.phpで本番用とテスト用データベースを設定する。

(設定例)

	class DATABASE_CONFIG {
	
		public $default = array(
			'datasource' => 'Database/Mysql',
			'persistent' => false,
			'host' => 'localhost',
			'login' => 'example',
			'password' => 'passw0rd!',
			'database' => 'example',
			'prefix' => '',
			'encoding' => 'utf8',
		);
	
		public $test = array(
			'datasource' => 'Database/Mysql',
			'persistent' => false,
			'host' => 'localhost',
			'login' => 'example',
			'password' => 'passw0rd!',
			'database' => 'example_test',
			'prefix' => '',
			'encoding' => 'utf8',
		);
		
		public $production = array(
		'datasource' => 'Database/Mysql',
			'persistent' => false,
			'host' => 'localhost',
			'login' => 'example',
			'password' => 'passw0rd!',
			'database' => 'example',
			'prefix' => '',
        	'encoding' => 'utf8',
		);
		
	}

#### 本番用/テスト用のデータベース切替方法

1. サーバー環境変数で切替
2. ClassRegistry::initで切替


#### 環境変数の定義

環境変数WEB_APP_ENVを定義しその値に応じて開発用DB、テスト用DB、本番用DBを切り替える。  
(192.168.33.10をテスト用ホスト、192.168.33.200を本番環境のホストと仮定する。)

##### Nginx

Nginxは後述するApacheのようにサーバー変数ではなくFastCGIの変数として定義する。  
FastCGIの設定はnginx設定ファイル(今回は/etc/nginx/sites-available/defaulで指定)のlocationディレクティブでfastcgi_paramを指定する。


テスト環境

	location ~ \.php$ {
		.....
    	fastcgi_param WEB_APP_ENV test;
		.....
    }

本番環境

	location ~ \.php$ {
        .....
		fastcgi_param WEB_APP_ENV  production;
		.....
    }

開発環境

	location ~ \.php$ {
		.....
    	fastcgi_param WEB_APP_ENV development;
		.....
    }

##### Apache

Apacheはサーバー環境変数として定義する。

(例) .htaccessのsetEnvディレクティブでWEB_APP_ENVを定義する例

開発環境

	Alias /foo /var/www/foo/app/webroot
	
	<Location /foo>
		.....
		SetEnv WEB_APP_ENV development
		.....
	</Location>


テスト環境

	Alias /foo /var/www/foo/app/webroot
	
	<Location /foo>
		.....
		SetEnv WEB_APP_ENV test
		.....
	</Location>

本番環境

	Alias /foo /var/www/foo/app/webroot
	
	<Location /foo>
		.....
		SetEnv WEB_APP_ENV production
		.....
	</Location>

#### データベース切替処理

##### Config/core.phpとConfig/database.phpで切替

Config/core.php

	if ( isset( $_SERVER[ 'WEB_APP_ENV' ] ) && $_SERVER[ 'WEB_APP_ENV' ] == 'production' ) {
		// 本番
		Configure::write( 'debug', 0 );
		Configure::write( 'database', 'production' );
	} elseif ( isset( $_SERVER[ 'WEB_APP_ENV' ] ) && $_SERVER[ 'WEB_APP_ENV' ] == 'test' ) {
		// テスト
		Configure::write( 'debug', 2 );
		Configure::write( 'database', 'test' );
	} else {
		// 開発
		Configure::write( 'debug', 2 );
		Configure::write( 'database', 'default' );
	}

Config/database.php

	class DATABASE_CONFIG {
	
		public $default = [
			.....
		];
	
		public $test = [
			.....
		];
	
		public $production = [
			.....
		];
	
		function __construct() {
			$connection = Configure::read('database');
			if (!empty($this->{$connection})) {
				$this->default = $this->{$connection};
			}
		}
	}

[CakePHPのデータベースを開発環境と本番環境で切り分ける方法  &raquo;  INSPIRE TECH](http://inspire-tech.jp/2011/04/cakephp_database_settings/)


##### Model/AppModelで切り替

__下記の方法で開発環境ではうまくいくがCircleCIではエラーが起きた。__

	MissingTableException: Table examples for model Example was not found in datasource default.

	<?php
	// app/Model/AppModel.php
	class AppModel extends Model {
		
		function __construct( $id = false, $table = null, $ds = null ) {
			parent::__construct(); /* 親クラスのコンストラクタを呼ばないとエラーになる */
			if ( isset( $_SERVER[ 'WEB_APP_ENV' ] ) && $_SERVER[ 'WEB_APP_ENV' ] === 'production' ) {
				$this->useDbConfig = 'production';
			} elseif ( isset( $_SERVER[ 'WEB_APP_ENV' ] ) && $_SERVER[ 'WEB_APP_ENV' ] === 'test' ) {
				$this->useDbConfig = 'test';
			}
		}
	}

##### ClassRegistry::initで切替

newを使わずClassRegistry::initでModelのインスタンスを作成すると自動的にテスト用のDBを参照する。

#### ユニットテストに必要なファイル(Modelの例)

* テスト対象 Model/Example.php
* テストケース Test/Case/Model/ExmpleTest.php
* フィクスチャ Test/Fixture/ExampleFixture.php

#### ユニットテスト実行

* ブラウザ
* コマンド

##### ブラウザ

ホストIPが192.168.33.10の例。

    http://192.168.33.10/test.php

##### コマンド

    $ Console/cake test [options] [<category>] [<file>]
    
(例1) アプリケーションのテスト

    $ Console/cake test app

アプリケーションの作成済みテストの一覧が表示されるので選択して実行する。

    App Test Cases:
    [1] AllTests
    [2] Controller/ExampleController
    [3] Model/Example
    ......

(例2) Test/Model/ExampleTest.phpの実行

    $ Console/cake test app Model/Example


#### Modelテスト例

Exampleモデルのsomeメソッドをテストは下記のようになる。


##### フォルダ構成

    app
    |
    |.....
    |
    |-- Model
        |
        |-- Example.php
    |
    |.....
    |
    |-- Test
        |
        |-- Case
            |
            |-- Model
                 |
                 |-- ExampleTest.php
        |
        |-- Fixture
            |
            |.....
            |-- ExampleFixture.php
            |


##### Exampleモデル

    // Model/Example.php
    <?php
    App::uses( 'AppModel', 'Model' );
    
    /**
     * Example Model
     *
     */
    class Example extends AppModel {
    
        /**
         * name
         *
         * @var
         */
        public $name = 'Example';
    
       ..... 
       .....
       
        /**
         * some
         */
        public function some() {
            ..... 
            return $value
        }
    }


##### Exampleモデルのテスト(ExampleTest.php)

    // Test/Case/Model/ExampleTest.php
	<?php
	App::uses( 'Example', 'Model' );
	
	/**
	 * Example Test Case
	 *
	 */
	class ExampleTest extends CakeTestCase {
	
		/**
		 * Fixtures
		 *
		 * @var array
		 */
		public $fixtures = array(
			'app.example' // --- (1)
		);
	
		/**
		 * setUp method
		 *
		 * @return void
		 */
		public function setUp() {
			parent::setUp();
			$this->Example = ClassRegistry::init( 'Example' );
		}
	
		public function testメソッドsomeの何らかのテスト() {
			$this->assertEquals( 'expect value', $this->Example->some() );
		}
	
		/**
		 * tearDown method
		 *
		 * @return void
		 */
		public function tearDown() {
			unset( $this->Example );
	
			parent::tearDown();
		}
	
	}

フィクスチャはテストで使うテーブル定義とレコード(テストデータ)を定義したファイル。  
例では(1)でフィクスチャを指定している。(1)はTest/Fixture/ExampleFixture.phpを参照する。  

##### フィクスチャ例1 ExampleFixture.php

下記はテストでもdatabase.phpの$defaultで定義された接続先のテーブル定義・データを使うフィクスチャの例。

	<?php
	/**
	 * ExampleFixture
	 *
	 */
	class ExampleFixture extends CakeTestFixture {
	
		/**
		 * Import
		 *
		 * @var array
		 */
		public $import = array('model' => 'Example', 'records' => true);
	
	}

Fixtureはbakeコマンドでインタラクティブに作成できる。

    $ Console/cake bake
    

テストの度にフィクスチャで指定したテーブルとレコードがdatabase.phpの$testで指定したデータベースに作成されテストが終わると破棄される。


##### フィクスチャ例2

CIサーバーでテストするときのようにフィクスチャにテーブル定義、レコードが必要なときはConsole/cake bakeコマンドでフィクスチャファイルにテーブル定義・データを含める。

	$ Console/Cake bake
 
	Welcome to CakePHP v2.6.1 Console
	---------------------------------------------------------------
	App : app
	Path: /var/www/application/current/app/
	---------------------------------------------------------------
	Interactive Bake Shell
	---------------------------------------------------------------
	[D]atabase Configuration
	[M]odel
	[V]iew
	[C]ontroller
	[P]roject
	[F]ixture
	[T]est case
	[Q]uit
	What would you like to Bake? (D/M/V/C/P/F/T/Q) 
	> F
	---------------------------------------------------------------
	Bake Fixture
	Path: /var/www/application/current/app/Test/Fixture/
	---------------------------------------------------------------
	Use Database Config: (default/test) 
	default
	Possible Models based on your current database:
	1. Account
	2. Item
	3. Part
	4. Product
	5. SchemaMigration
	6. User
	Enter a number from the list above,
	type in the name of another model, or 'q' to exit  
	[q] > 2
	Would you like to import schema for this fixture? (y/n) 
	[n] > n
	Would you like to use record importing for this fixture? (y/n) 
	[n] > n
	Would you like to build this fixture with data from Item's table? (y/n) 
	y
	Please provide a SQL fragment to use as conditions
	Example: WHERE 1=1  
	[WHERE 1=1] > 
	How many records do you want to import?  
	[10] > 10
	
	Baking test fixture for Item...



### データベースバージョン管理(マイグレーション)

> 1. MySQLAdminなどを使ってテーブルの作成
> 2. Migrationバージョンの生成
> 3. 次に差分を自動するためのSchemaファイルの生成

[Migrationsプラグインの実践的運用 - ２４時間CakePHP](http://d.hatena.ne.jp/hiromi2424/20111220/1324387254)


#### Migrationのバージョン生成

    $ Console/cake Migrations.migration generate -f

オプジョンfをつけてモデルのないテーブルもマイグレーションの対象に含める。
    
#### スキーマファイル作成

    $ Console/cake schema generate -f   // データベースからスキーマファイルを作成(Config/Schema/schema.php)
    
オプジョンfをつけてモデルのないテーブルものスキーマに含める。


### 本番環境でのスキーマ管理

本番環境のスキーマ管理はマイグレーション適用で行う。

	$ ./Console/cake Migrations.migration run

CircleCIで自動化の対象。

[CakePHP2系でマイグレーションを利用する方法 | Ryuzee.com](http://www.ryuzee.com/contents/blog/6108)

#### スキーマダンプ

shemaシェルはテーブル定義のダンプすることもできる。   
    
    $ Console/cake schema dump --write filename.sql  // filename.sqlへダンプ(Config/Schema/dump.sql)
 
[cakePHP2.3 Schema - Logicky Blog](http://endoyuta.com/2013/08/17/cakephp2-3-schema/)

## <a name="dev_process">開発手法</a>

* アジャイル
* BDD:振舞駆動開発 (開発手法)


### <a name="agile">アジャイル</a>

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



### <a name="bdd">BDD:振舞駆動開発 (開発手法)</a>

振舞駆動開発(BDD: Behavior Driven Develop)。

> BDDはユーザーストーリーと、そこから導きだされるビジネスロジックをテストすることに注目しています。
(p.178)

> ユーザーストーリーから具体的な利用シーンを抽出します。これを「シナリオ」と呼びます。
> BDDのフレームワークを使う場合は、ドメイン特化型言語(DSL domain-specific language)と呼ばれる記法で記述します。
> 代表的なDSLは「Gherkin」と呼ばれ、記述したシナリオは、受け入れテストとして「Cucumber 」や「Behat」などのテスティングフレー> > ムワークによって自動実行できます。
(p.181)


#### BDDの流れ

* BDDテストフレームワーク - Behat  
  > Behat is an open source behavior-driven development framework for PHP
  [Behat Documentation &mdash; Behat 2.5.3 documentation](http://docs.behat.org/en/v2.5/)
	+ sizuhiko/Bdd  
    CakePHP2用のプラグイン。
	+ behat/mink-goutte-driver  
    JavaScriptを使わずBehatを利用するプラグイン。

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


#### Behat/Mink (BDD用テスティングフレームワーク)

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

#### BDDと単体テスト

__フィーチャは最終的に単体テストの集まりを実行する。__


[目次へ戻る](#index)



# <a name="aws">AWS(Amazon Web Services)でWEBサービス運用</a>

## 目次

* [Linuxセキュリティ](#aws_security)
* [EC2](#aws_ec2)
* [RDS](#aws_rds)
* [Route 53](#aws_route53)
    + 独自ドメイン運用
* [S3](#aws_s3)
* [Ubuntu + Postfix](#aws_postfix) 
* [課金](#aws_bills)
* [Linuxコマンド(Ubuntu)](#aws_cmd_ubuntu)
* [AWS 用語](#aws_aws_terms)



## <a name="#aws_security">Linuxセキュリティ</a>

### 目的

EC2でUbuntuを安全に運用する。

1. サーバーOSへ安全にログイン
	+ EC2はSSHでログインする(パスワード認証は無効)。
	+ rootのログインを禁止する。
	+ EC2はでフォルトでrootのログインを禁止している。
2. サービスの不正アクセス対策  
  	+ サービスへのアクセス制御ははiptableでポート番号の開閉で制限する。
  	+ EC2はSecurity Groupsでポート番号の開閉を行う。

### root権限取得

(1) コマンド実行時に一時的にroot権限を取得する。

    $ sudo <command>

(2) rootへ変更する。

    $ su root
    # <command>

rootはプロンプトが$から#へ変更される。

### OSへのログイン

#### rootのログイン禁止

    $ su root
    $ echo > /etc/securetty

__EC2はでフォルトでrootログインを禁止している。デフォルトのログインユーザーははubuntu。__

#### SSHでのみ接続

#### SSH接続でrootのログイン禁止

/etc/ssh/sshd_config

    PermitRootLogin no

#### SSHポート番号変更

/etc/ssh/sshd_config

    Port 22222

#### SSHサーバー再起動

    /etc/init.d/ssh restart

__EC2は上記の変更をしても22番でログインできた。__

### ユーザー

#### ユーザー情報

    // パスワードやホームディレクトリなど
    $ cat /etc/passwd | grep <user>

    // ユーザー名･グループ名、ユーザーID、グループID
    $ id <user

#### ユーザー一覧

    // ユーザー情報表示
    $ cat /etc/passwd
    // ユーザー一覧表示
    $ cut -d: -f1 /etc/passwd

#### ユーザー作成

    $ sudo useradd <user>

### ユーザーパスワード

    $ sudo passwd <user>

#### グループ確認

	groups <user>

#### グループ一覧

	$ cut -d: -f1 /etc/group

#### グループ作成

    $ sudo groupadd <group>

#### ユーザーをグループへ追加/削除

    $ sudo gpasswd -a <user> <group>

__aオプションを指定しないと既存グループが削除される。__

	$ sudo gpasswd -d <user> <group>
   
#### rootユーザーにパスワード設定

    $ sudo passwd root
    Enter new UNIX password: 
    Retype new UNIX password: 
    passwd: password updated successfully

#### suの利用制限

デフォルトはすべてのユーザーがsuでrootへ変更できる。
suコマンドを実行できるユーザーをwheelグループのユーザーに制限する。

/etc/pam.d/su

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

    // wheelグループへubuntu追加
    ubuntu@xxx:~$ sudo usermod -G wheel ubuntu
    
su rootができる。

    ubuntu@xxx:~$ su root
    Password: 
    root@xxx: #


### サービス

不要なサービスは停止する。

#### サービス一覧

    // RPM系のchkconfigと同様の機能を持つsysv-rc-confインストール
    $ sudo apt-get sysv-rc-conf
    // 起動サービス一覧表示
    $ sudo sysv-rc-conf --list

#### サービス停止

    $ sudo sysv-rc-conf service Off

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

    $ ssh -i private_key_file destination

SSHで鍵を指定(iオプション)した接続で鍵が見つからなければデフォルトの鍵(~/.ssh/id_rsa)で接続を試みる。  
iオプションでの鍵の指定は絶対パスまたは相対パスで指定する。

### <a name="aws_ec2_security">Security Groups</a>

* EC2
    + NETWORK & SECURITY
        - Security Groups


どのポートを空けるか(どのサービスを利用可能にするか)を各インスタンスごとにセキュリティグループとして設定する。  

### <a name="aws_ec2_ip">Elastic IPs</a>

* EC2
    + NETWORK & SECURITY
        - Elastic IPs

__T2 instances are VPC-only. Your T2 instance will launch into your VPC. Learn more about T2 and VPC.__

固定IPはインスタンスの起動・再起動で割り当てられる値が変わる。  
IPアドレスを再起動後も固定にするにはElastic IPが必要?。

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

    $ mysql -h endpoint -u username -p

AWSではパスワードなしでrootでログインすることはできない。


### 課金

[よくある質問 - Amazon RDS（リレーショナルデータベースサービス Amazon Relational Database Service） | アマゾン ウェブ サービス（AWS 日本語）](http://aws.amazon.com/jp/rds/faqs/)

上記ページの請求に関する記載がある。


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

    $ nslookup dns domain



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
* [Postfix+Dovecotによるメールサーバ構築 ｜ Developers.IO](http://dev.classmethod.jp/cloud/aws/mail_server_with_postfix_and_dovecot/)

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



## <a name="cmd">Linuxコマンド(Ubuntu)</a> 

### デーモンの起動・停止

    $ sudo service <daemon> start|stop|status|restart|force-reload

### ポート番号確認

    $ netstat -tlnp


### パッケージ確認

#### Debian系のインストール済みパッケージ確認  

    $ dpkg -l  
    // または
    $ aptitude search "~i"


### Debian系パッケージのインストールパス確認

    $ dpkg -L <package>  
    $ aptitude search <package>


### プロセス  

    $ ps -ef|grep postfix
  

### DNS確認  

    $ nslookup ndsname domain  
    $ host domain


### cat

標準出力へ出力する。


### find . -name target
 
    $ find . -name target


## <a name="terms">用語</a>

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


[目次へ戻る](#index)



## <a name="appendix">Apendix</a>

### <a name="appendix_owner">Appendix ディレクトリ/ファイル所有者・グループ変更</a>

    sudo chown [-f|-R] username (filename|dirname)
    sudo chgrp  [-f|-R]] groupname (filename|dirname)


### <a name="appendix_php_file">Appendix PHPのファイル作成雛形</a>

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

### <a name="appendix_nginx_default">Appendix 動作したNginx設定ファイルsite-available/default</a>

とりあえずUbuntu + Nginxで動作した設定。

    server {
            listen 80 default_server;
            listen [::]:80 default_server ipv6only=on;

            root /var/www/application/current/app/webroot;
            index index.php index.html index.htm;

            # Make site accessible from http://localhost/
            server_name example.com;

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


### <a name="appendix_nginx_default_book">Appendix 書籍「CakePHP 継続的インテグレーション」のNginx設定ファイルsite-available/default</a>

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


### <a name="appendix_php_ini">Appendix php.iniの設定を反映されないときの対処</a>

php.ini(/etc/php5/fpm/php.ini)を変更しNginxを再起動したが変更内容が反映されなかった(phpinfo関数で確認)。  

    $ sudo nginx -s stop
    $ sudo nginx
    // 再起動
    $ sudo nginx -s reload

php5-fpmを起動すると設定が反映された。

    $ cd /etc/init.d/p
    $ sudo php5-fpm

sudo php5-fpmを実行するさい下記エラーが表示された。

    [11-Feb-2015 07:05:36] ERROR: An another FPM instance seems to already listen on /var/run/php5-fpm.sock
    [11-Feb-2015 07:05:36] ERROR: FPM initialization failed

既存のプロセスを停止しさいどsudo php5-fpmを実行したらphp.iniの内容が反映された。

    $ ps aux | grep php-fpm
    root      2889  0.0  1.6 313876  6228 ?        Ss   06:07   0:00 php-fpm: master process (/etc/php5/fpm/php-fpm.conf)
    www-data  2890  0.0  2.0 313996  7564 ?        S    06:07   0:00 php-fpm: pool www
    www-data  2891  0.0  1.4 313876  5280 ?        S    06:07   0:00 php-fpm: pool www
    
    $ sudo kill -9 2889 2890 2891
    $ sudo php5-fpm


### <a name="appendix_zip">zipのインストール</a>

    $ sudo apt-get install zip
    
初回インストールで下記メッセージが表示された。

	E: いくつかのアーカイブを取得できません。apt-get update を実行するか --fix-missing オプションを付けて試してみてください。

sudo apt-get updateを実行後はインストールが正常に行われた。

    |-- example
        |
        |-- target
            |
            |...

targetを圧縮するサンプル

    $ cd /path/to/example
    $ zip -r target.zip ./target/
    

### <a name="appendix_chef_mysql">参考書籍 CakePHPで学ぶ継続的インテグレーションのChefでMySQLをインストールした場合のパスワード</a>


#### site-cookbooks/phpenv/recipes/default.rb

下記でパスワードが設定されている。

	# MySQLのrootアカウントのパスワードをセットする
	execute "set_mysql_root_password" do
	  command "/usr/bin/mysqladmin -u root password \"#{node['mysql']['root_password']}\""
	  action :run
	  only_if "/usr/bin/mysql -u root -e 'show databases;'"
	end
	
	
#### root_passwordの指定

site-cookbook/phpenv/attribute/default.rb

    default['mysql']['root_password'] = 'password'


### <a name="appendix_aws_sftp">Appendix FileZillaを使ったAWS EC2へのSFTP接続</a>

FileZilla設定方法は下記サイトを参考にして設定 
[WordPress】素人でも出来た！AWS EC2へのサーバー移行方法7 旧データをアップロード | 男子風呂（ぐ）](http://danshiblog.com/job/130128-amazon-ec2-server-setting-wordpressdata-upload.html)


* SFTPはSSHの使用ポート番号(22)を利用するのでAWSのSecurity Groupsで
別途ポートを開ける必要は無い。
* AWSはrootでは接続できないのでubuntuユーザーで接続
* PHPの実行ユーザーはwww-data
* FTPソフトでアップロードした場合のディレクトリ/ファイルの所有者はubuntu。
* 表示するだけならHTML/CSS/JavaScript/PHPの各ファイルの実行権限は004でよい
  (ディレクトリは755)。


### <a name="appendix_ubuntu_apache_mysql_php">Appendix Ubuntu + Apache + MySQL + PHP</a>

    $ sudo apt-get update

    $ sudo apt-get install apache2
    $ sudo apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql
    $ sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt

### Apache設定ファイル

/etc/apache2/apache2.conf


### <a name="appendix_berkshelf">Appendix Berkshelfはbundleで管理して使うとエラーがでるのでChefDKを使う</a>

[ChefDk の berkshelf で cookbook をダウンロードする - Qiita](http://qiita.com/shin1x1/items/872cf5b9396516068892)
[Chef Development Kit | Chef Downloads | Chef](https://downloads.chef.io/chef-dk/mac/#/)


### <a name="appendix_behat">Appendix behatインストールで発生したエラーへの対応</a>

#### エラー

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

#### 対応

下記ファイルに記載されているトークン(Token)をcomposer.jsonファイルのconfigへ追記する。
[Composer Update Fails due to Github Authorization · Issue #3542 · composer/composer](https://github.com/composer/composer/issues/3542)

    /home/vagrant/.composer/auth.json

composer.json

    {
        …..
        "config": {
            "vendor-dir": "Vendor",
            "github-oauth": {
                “github.com”: Token
            }
        },
        …..
    }



### <a name="appendix_jenkins_display">Jenkins設定画面でエラー</a>

#### エラー

    stderr: Host key verification failed.が出たら、git ls-remoteを叩く！

[Jenkinsを導入してGithub, Bitbucketから自動ビルドを可能にするまで](http://tsukaby.com/tech_blog/archives/250)

#### 対処

パスフレーズなしでキーを作成した。こちらで解決した可能性が高い。

[Google グループjenkinsからgithubへのssh接続](https://groups.google.com/forum/#!topic/jenkinsci-ja/JkjRAyQyOKE)

### <a name="appendix_jenkins">Appendix JenkinsのビルドでAPCの書き込みエラーが発生する問題</a>

PHPUnitでの単体テストがうまく行っていない


    vagrant@ci:/var/lib/jenkins/jobs/blogapp/workspace/app$  Console/cake test app AllTests
    PHP Warning:  _cake_core_ cache was unable to write 'cake_dev_eng' to File cache in /var/lib/jenkins/jobs/blogapp/workspace/app/Vendor/cakephp/cakephp/lib/Cake/Cache/Cache.php on line 323

[シェルからCakePHPを使うとAPC書き込みに失敗する問題 - 創作メモ帳](http://sousaku-memo.net/php-system/952)

#### 1. /etc/php5/cli/conf.dディレクトリ20-apc.iniを作成。


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

#### 1を記載したら下記のエラーがでるようになった。

    PHP Warning:  PHP Startup: Unable to load dynamic library '/usr/lib/php5/20121212/apc.so' - /usr/lib/php5/20121212/apc.so: cannot open shared object file: No such file or directory in Unknown on line 0

1. apc.soをインストールしたい。
2. sudo pecl install APCではエラー出てインストールできなかった。
3. apc.soはphp-develに含まれるらしい。
4. php-develをインストールするためにyumをインストールした。
  sudo apt-get install yum
5. yum install php-develではエラー


### <a name="appendix_cuisine">Appendix cuisineモジュール</a>

[sebastien/cuisine](https://github.com/sebastien/cuisine)

#### ImportError: No module named cuisine

最初、sudoを使いスーパーユーザーでcuisineをインストールしていた。

    post:
      - sudo pip install cuisine
      
ImportError: No module named cuisineが出たのでsudoなしでインストールし解決した。  
Pythonのモジュールはスーパーユーザーと一般ユーザーでインストールされるパスが異なる。

    post:
      - pip install cuisine

#### モジュール一覧

スーパーユーザーでインストールされたモジュール一覧。

    $ sudo pip freeze
    
一般のユーザーでインストールされたモジュール一覧。

    $ pip freeze



### <a name="appendix_so">Appendix soファイル</a>

> .soファイル 【 shared object file 】 .so形式 / .soフォーマット
>
> soファイル / so形式
> UNIX系OSの共有ライブラリのファイル形式。拡張子が「.so」であることからこのように呼ばれる。実行可能形式のプログラムが格納されているが、単体で起動することはできず、他のプログラムにリンクしてその機能を呼び出すようになっている。
>この形式のファイルはプログラムの実行時にリンクされる動的リンク(ダイナミックリンク)ライブラリで、ビルド時にリンクされる静的リンク(スタティックリンク)ライブラリの場合は「.a」(archiveの略)という拡張子になる。

[.soファイルとは 〔 soファイル 〕 【 shared object file 】 - 意味/解説/説明/定義 ： IT用語辞典](http://e-words.jp/w/2EsoE38395E382A1E382A4E383AB.html)


### <a name="appendix_ruby">Appendix Ruby</a>

    $ brew install ruby

brewをつかい最新版をインストールした(2.2.0)。
バージョンの確認をすると下記のようにMac OSに標準でインストールされているバージョンが表示される。

    $ ruby --version
    ruby 2.0.0p481 (2014-05-08 revision 45883) [universal.x86_64-darwin13]

rubyのバージョンを完全にアップデートする方法は解らなかった。
下記をためしたが解決はしていない。


#### rbenv/ruby-buildでインストール

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


### <a name="appendix_aws_recipe">Appendix AWS構築手順</a>

    $ sudo apt-get update
    $ sudo apt-get install php5 php5-cli php5-fpm php5-mysql php-pear php5-curl php5-dev php-apc php5-xsl php5-mcrypt mysql-server-5.5 nginx
    $ sudo mkdir -p /var/www/application/current/app/webroot
    $ cd /var/www/application
    $ sudo chown -R www-data current
    $ sudo usermod -G www-data ubuntu
    $ sudo chmod -R 775 current
    $ sudo vi /etc/nginx/sites-available/default
    $ sudo nginx -s reload
    
    
### <a name="appendix_basic">Appendix NginxでBasic認証</a>

ルートが/var/wwwのでサイト全体にBasic認証を設定する例。

#### .htpasswd作成

	// htpasswd作成ツールインストール
	$ sudo apt-get install apache2-utils
	
	// apr1 (Apache MD5) 暗号化 
	htpasswd -nbm <username> <password> > /var/www/.htpasswd

Nginx設定ファイル編集

今回は/etc/nginx/sites-available/defaultを編集した。

	location / {
		.....
		auth_basic            'Auth Basic Sample';
		auth_basic_user_file  '/var/www/.htpasswd';
	}


[Nginx でBasic認証(ユーザ名、パスワードを求める )するには | レンタルサーバー・自宅サーバー設定・構築のヒント](http://server-setting.info/centos/apache-nginx-7-basic-auth.html)

[目次へ戻る](#index)
