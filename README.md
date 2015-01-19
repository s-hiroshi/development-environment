# 開発環境構築メモ

AWSでサービスを運用するために勉強していることのメモ。  
勉強中の内容を書き留めた個人的なメモ書き。

## 目標

1. 継続的的インテグレーションが可能な開発環境構築
2. AWSでサービス運用

## 参考書

[「CakePHPで学ぶ継続的インテグレーション」 渡辺 一宏  (著), 吉羽 龍太郎 (著), 岸田 健一郎  (著), 穴澤 康裕 (著), 丸山 弘詩  (編集)](http://www.amazon.co.jp/CakePHP%E3%81%A7%E5%AD%A6%E3%81%B6%E7%B6%99%E7%B6%9A%E7%9A%84%E3%82%A4%E3%83%B3%E3%83%86%E3%82%B0%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3-%E6%B8%A1%E8%BE%BA-%E4%B8%80%E5%AE%8F/dp/4844336789/ref=tmm_pap_title_0?ie=UTF8&qid=1421710653&sr=8-1)


## 内容

* [継続的インテグレーション](#ci)
* [AWSでサービス運用](#aws)


# 継続的インテグレーション

## 本記事の内容

「CakePHPで学ぶ継続的インテグレーション」Impressの読書メモ

## メモ目次

* [CI(継続的インテグレーション)](#ci)
* [アジャイル](#agile)
* [BDD:振舞駆動開発 (開発手法)](#bdd)
* [HTML/CSS/JavaScriptのCI](#html_css_javascript_ci)
* [PHPのCI](#php_ci)
* [CakePHPのCI](#cakephp_ci)
* [VirtualBox + Vagrant + Chef Soloを使ったCI環境構
  築](#virtualbox_vagrant_chef)
    + 開発サーバー(develop)
    + CIサーバー(ci)
    + デプロイサーバー(deploy)
* Appendix
    + [1. Terminalでパスを確認](#appendix1)
    + [2. Linux](#appendix2)
    + [3. パッケージ管理システム](#appendix3)
    + [4. Ubuntu]
    + [4. Nginx](#appendix4)
    + [5. PHP](#appendix5)
    + [6. CakePHP](#appendix6)
    + [7. Ruby](#appendix7)
    + [8. Berkshelfはbundleで管理して使うとエラーがでるのでChefDKを使う](#appendix8)
    + [9. behatインストールで発生したエラーへの対応](#appendix9)
    + [10.  JenkinsのビルドでAPCの書き込みエラーが発生する問題](#appendix10)
    + [11. gitのエディタを変更](#appendix11)
    + [12. 用語](#appendix12:)
    + [13. 英語](#appendix13)
    + [14. CIサーバーJenkinsのパス](#appendix14)



## <a name="ci">CI(継続的インテグレーション)</a>

継続的な開発を効率的に行うための開発手法。

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


## <a name="html_css_javascript_ci">HTML・CSS・JavaScriptの継続的インテグレーション</a>

### 開発ツール

* デバックツール ブラウザ
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

### 継続的開発

各ツールをGruntで自動化する。

### フォルダー構成

    example
    |
    |— index.html
    |
    |— css
         |— style.min.css        // 公開サイトへデプロイ
    |—  js
         |— scripts.min.js       // 公開サイトへデプロイ
    |
    |— dev
         |— css
              |— doc            // CSS スタイルガイドフォルダ StyleDocco
              |— src
                   |— style.css
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


### Grunt

[Grunt: The JavaScript Task Runner](http://gruntjs.com/)

gruntは一般的にgrunt-cliのみグローバルへインストールし
grunt本体も含め各プラグインはプロジェクトごとのフォルダへインストールする。

#### 1. node/npmのインストール

[node.js](http://nodejs.org/)

Gruntはnode.js/npmを使う。

node.jsはpkgファイルをダウンロードしインストールする。
npmはpkgファイルでnode.jsをインストールした場合は同時にインストールされる。

#### 2. grunt command line interface(grunt-cli)のグローバルへのインストール

    $ sudo npm install -g grunt-cli

#### 3. プロジェクトフォルダにpackage.jsonを作成

    {
      "name": "example-project",
      "version": "0.0.1"
    }

#### 4. プロジェクトフォルダにGrunt本体とプラグインをインストール

grunt本体とプラグインはプロジェクトごとにプロジェクトフォルダへインストールする。

##### 4-1. grunt本体をインストールする。

    $ cd <example-project path>
    $ npm install grunt --save-dev

–save-devをつければpackage.jsonのdevDependenciesプラグイン情報が追記される。

    {
      "name": "example-project",
      "version": "0.0.1",
      "devDependencies": {
      "grunt": "~0.4.1"
      }
    }

##### 4-2. プラグインをインストール

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

#### 5. Gruntfile.jsの記載



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

#### 6. gruntの確認

    $ grunt  # watchを実行(watchにタスクとして他の処理を指定しているのでそれらも順番に実行)
    $ grunt <package name>   # pakege nameのみ実行

#### package.jsonをもとにインストール

    $ npm install

既存のnode_modulesフォルダがあれば削除しておく。

#### プラグインのバージョンアップ

    $ nom update —save-dev

すべてのパッケージがアップデートされる。


### Sass

CSSプリプロセッサー。

[Sass: Syntactically Awesome Style Sheets](http://sass-lang.com/)

#### Sassインストール

    $ sudo gem install sass

#### Sassのコンパイル

sassコマンドを使いstyle.scssをコンパイルして同じフォルダーにstyle.cssを作成する例。

    $ sass style.scss style.css

#### 変更を監視して自動コンパイル

    $ sass –watch style.scss:style.css

watchはターミナルではctrl + Cで停止する。

#### バージョン確認

    $ sass —version
    3.4.9

#### パス確認

    $ which sass
    /usr/bin/sass


### Compass

Sassを使うCSS作成フレームワーク。

[Compass Home | Compass Documentation](http://compass-style.org/)
[Sass/Compass のインストールと基本的な環境設定 | Web Design Leaves](http://www.webdesignleaves.com/wp/htmlcss/652/)

#### インストール

    $ gem install compass
 
#### バージョン確認

    $ conpass —version
    1.0.1

#### パス確認

    $ which compass
    /usr/bin/compass

#### コンパス初期化

    $ create compass --bare

    contrib.rbとsassフォルダが作成される。

#### config.rb(Compass設定ファイル)

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

上記例ではcompassコマンドでコンパイルするとdev/sassフォルダーのモジュールファイルを除いたscssファイルをコンパイルしcssディレクトリへ出力する。

#### コンパイル

    $ compass watch css/sass/main.scss

#### バージョン確認

    $ compass —version
    Compass 0.12.2 (Alnilam)

#### パス確認

    $ which compass
    /usr/bin/compass


### StyoeDocco

CSS スタイルガイド。

#### インストール

    $npm install -fg styledocco

オプションfは必須。
パーミションの関係でsudoでインストール。

    $ sudo npm install -fg styledocco

##### インストール先
/usr/local/lib/node_modules/styledocco/bin/styledocco

#### パスの確認

    $ which styledocco
    /usr/local/bin/styledocco

#### スタイルガイド作成

    $ cd mytheme
    $ styledocco -n "My Theme" -o docs/css style.css

#### Grunt + StyleDocco

[grunt-styledocco](https://www.npmjs.com/package/grunt-styledocco)

#### Gruntfile.js

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


### Code Inspections(検査)

 * jsLint  PhpStormはデフォルトでサポート。
 * jsHint  PhpStormはデフォルトでサポート
    jsLintより緩い


### QUnit テストツール

[QUnit](qunitjs.com)

#### Grunt + Qunit(+PhantomJS)

[gruntjs/grunt-contrib-qunit](https://github.com/gruntjs/grunt-contrib-qunit)

grunt-contrib-qunitはPhantomJSを含む。

#### インストール

    $ npm install grunt-contrib-qunit —save-dev

Qunitファイル(js/css)をCDNから読み込むと正常にテストできなかった。
ダウンロードして配置した。


### YUI Docド JavaScript キュメンテーションツール

[YUIDoc – Javascript Documentation Tool](http://yui.github.io/yuidoc/)
[YUIDoc Syntax Reference](http://yui.github.io/yuidoc/syntax/index.html)

#### インストール

    npm -g install yuidocjs.

#### コマンド

    yuidoc

#### Grunt + YUI DOC

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


## <a name="php_ci">PHPのCI</a>

### Composer

PHPパッケージ管理ツール。

#### Composerのインストール

    $ curl -sS https://getcomposer.org/installer | php

composer.pharがカレントディレクトリにダウンロードされる。
ダウンロードしたcomposer.pharは通常composerへ名前を変更しパスの通っているディレクトリへ移動する。(例 /usr/local/bin)
パスを通した時コマンドは下記で実行できる。

    $ composer --version

パスが通っていないときはcomposer(名前を変更していないときはcomposer.phar)コマンドは下記のように実行する。

    $ php /fullpath/composer --version


#### Composerの初期化

    $ composer init

composer.jsonファイルが作成される。

#### composer.json

    {
        "require": {
            "monolog/monolog": "1.0.*"
        }
    }

[Composer ドキュメント日本語訳](http://kohkimakimoto.github.io/getcompo:ser.org_doc_jp/doc/01-basic-usage.html)

Composerでインストールしたパッケージをコマンドを実行した
ディレクトリにvendor[^conposer-cake]ディレクトリを作成しそこへ配置して管理する。

[^conposer-cake]: デフォルトはConposerはインストールしたパッケージをvendorディ
レクトリに配置する。しかしCakePHPはVendorディレクトリを利用するためvendorから
Vendorへ変更する。変更はcomposer.jsonのconfig.vendor-dirプロパティで設定する。

#### composer.jsonに記載したすべてのパッケージインストール

    $ composer install


1. パッケージをインストール
2. composer.lockファイルを作成しバージョンを固定する。

#### パッケージの追加
  
    // 通常のインストール
    $ composer require
    // 開発で必要なパッケージ
    $ composer require --dev
 
composer requireはパッケージを追加しcomposer.jsonへ追記する。

composer require-devは開発環境でのみ必要なパッケージを追加するときに使う。
composer installではインストールされず--devオプションを付けてcomposer install --devでインストールされる。

#### パッケージがインストールされる場所


通常composer.jsonと同階層にvendorディレクトリへインストールされる。

上記composer.jsonの例では

    vendor/monolog/monolog

    |-- composer.json
    |-- composer.lock
    |-- vendor
          |--monolog
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


### テストツール

* [PHPUnit #x2013; The PHP Testing Framework](https://phpunit.de/)
  単体テスト。
* [Xdebug - Debugger and Profiler Tool for PHP](http://xdebug.org/)
  テストコードカバレッジ取得。

### PHPUnit

PHPUnitのインストールの方法は幾つかある。

#### phpunit.pharをダウンロード

> ➜ wget https://phar.phpunit.de/phpunit.phar
>
>➜ chmod +x phpunit.phar
>
>➜ sudo mv phpunit.phar /usr/local/bin/phpunit
>
>➜ phpunit --version

#### ComposerでPHPUnitをインストール

##### composer.json

    {
        "require-dev": {
            "phpunit/phpunit": "3.7.*"
        }
    }


##### インストール

    $ composer install --dev

require-devへ設定したパッケージはcomposer installを--devオプションをつけて実行しないとインストールされない。よって開発環境だけで必要なパッケージをインストールするときに使われる。

##### phpunitコマンド

    vendor/bin/phpunit

##### 基本テスト

CalcTest.php

    <?php
    require_once(“Calc.php”);

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


##### テスト実行

    $ vendor/bin/phpunit CalcTest

コマンドが実行されたディレクトリ(ワーキングディレクトリ)からの相対パスでファイルを検索するので上記例では同階層にCalcTest.php/Calc.phpが必要

##### --bootstrapオプション

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

###### bootstrap.php

    <?php
    require_once('Calc.php');  // workingディレクトリからの相対パス

###### 実行

    $ vendor/bin/phpunit --bootstrap Test/bootstrap.php CalcTest

##### 設定ファイル phpunit.xml


### Code Inspections(検査)

PHP_CodeSniffer


### ドキュメンテーションツール

phpDocumentor

### デバック

XDebug



## <a name="cakephp_ci">CakePHPのCI</a>

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


## <a name="virtualbox_vagrant_chef">VirtualBox + Vagrant + Chef Soloをで継続的CI環境構築(開発環境構築/プロビジョニング/デプロイ)</a>

### 仮想化ソフトウェア

* VirtualBox
* VMware

仮想化ソフトのゲストOSを開発環境として利用する。


### 仮想開発環境構築ソフトウェア

仮想化ソフトウェアの制御やゲストOSのプロビジョニングを行う。

### Vagrant 仮想開発環境構築ソフトウェア

> Vagrant（ベイグラント）は、FLOSSの仮想開発環境構築ソフトウェア[1]。VirtualBoxをはじめとする仮想化ソフトウェアやChef（英語版）やSalt（英語版）、Puppetといった構成管理ソフトウェアのラッパーとみなすこともできる。

Wiki


### Chef

構成管理ツール /プロビジョニングツール


> プロビジョニング（英: Provisioning）は、本来は「準備、提供、設備」などの意味であり、現在では通常、音声通信やコンピュータなどの分野における、ユーザーや顧客へのサービス提供の仕組みを指す。

Wiki


### VirtualBox + Vagrant + Chef

プロビジョニングツールChef Client, Chef SoloはVagrantプラグインvagrant-omnibusで導入する。設定はVagrantfileに記載する。

### Vagrantの役割

1.仮想サーバの起動・終了
2. ホストOSとゲストOSでディレクトリ共有
   例) develop.vm.synced_folder “application”, "/var/www/application/current", (Vagrantfile)
3.仮想サーバのプロビジョニング
   Knife-solo, berkshelf


[Command-Line Interface - Vagrant Documentation](http://docs.vagrantup.com/v2/cli/)

#### Vagrantのボックス追加

    $ vagrant box add [options] <name, url, or path>
    # vagrant box add —helpでオプション等を確認する。
 
例) Bentoのopscode-ubuntu-14.04の場合

    $ vagrant box add opscode-ubuntu-14.0.4 http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-14.04_chef-provisionerless.box

#### 初期化

    $ vagrant init

#### 起動・休止・シャットダウン・削除

    $ vagrant up                 // 起動
    $ vagrant halt               // シャットダウン
    $ vagrant destroy            // 削除

vagrant shutdownはない。

#### SSHで接続

    $ vagrant ssh

#### vagrant プラグインの導入

    $ vagrant plugin install sahara
    $ vagrant plugin install vagrant-omnibus
    $ vagrant plugin install vagrant-cachier

##### vagrant plugin installで発生したエラーへの対応

###### エラーメッセージ

> Bundler, the underlying system Vagrant uses to install plugins,
> reported an error. The error is shown below. These errors are usually
> caused by misconfigured plugin installations or transient network
> issues. The error from Bundler is:

> An error occurred while installing nokogiri (1.6.5), and Bundler cannot continue.
> Make sure that `gem install nokogiri -v 1.6.5 succeeds before bundling.

###### 解決策

    $ gem install --install-dir ~/.vagrant.d/gems nokogiri -v '1.6.5'

[vagrant pluginをインストールしようとするとnokogiriエラーがでてインストールできない時の対策 | hypermkt blog](http://blog.hypermkt.jp/can-not-install-vagant-plugin-by-nokogiri/)


#### プロビジョニング

Vagrantは仮想サーバーを起動したときや指定したタイミングでプロビジョニングを行うことができる。


#### プロビジョニングツール

Chef(Chef Solo, Chef Server)
[Chef | IT automation for speed and awesomeness | Chef](https://www.chef.io/chef/)

#### knife-solo

Chef Soloのリポジトリを作成する。

[knife-solo](http://matschaffer.github.io/knife-solo/)

#### Berkshelf

Chefのクックブックとその依存関係を管理する。

BerkshelfをBundlerを使いインストールしたとき下記コマンドでエラーが発生した。
Bundlerを使わずChefDKを利用することで解決した。

    $ bundle exec berks vendor ./cookbooks

#### ChefDk

Chef関連のツール集(Berkshelf, Knife-soloなど)

[Chef Development Kit | Chef Downloads | Chef](https://downloads.chef.io/chef-dk/)
 

Bundlerでエラーが発生した下記処理もChefDKで解決した。

    $ bundle exec knife cookbook create <name> -o site-cookbooks

###### ChefDk導入後

    $ knife cookbook create <name> -o site-cookbooks

#### Vagrant, Chefを使ったプロビジョニング手順

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


#### Capistrano3

開発環境からデプロイ環境にデプロイするツール。


## 継続的インテグレーションサーバー

### Jenkins

GitHubリポジトリ

Jenkinsの設定画面でエラーがでる。下記を行う


stderr: Host key verification failed.が出たら、git ls-remoteを叩く！

[Jenkinsを導入してGithub, Bitbucketから自動ビルドを可能にするまで](http://tsukaby.com/tech_blog/archives/250)


パスフレーズなしでキーを作成した。こちらで解決した可能性が高い。

[Google グループjenkinsからgithubへのssh接続](https://groups.google.com/forum/#!topic/jenkinsci-ja/JkjRAyQyOKE)




## <a name="appendix1">Appendix 1. terminalでパスを確認</a>

    $ echo $PATH



## <a name="appendix2">Appendix 2. Linux</a>

* Debian系
* レッドハット(RPM)系


### ファイル・ディレクトリの検索

    $ cd /
    4 sudo find . -name <target>

### Ubuntu

> Ubuntu（ウブントゥ[7]、国際音声記号[ʊˈbʊntuː]; oo-BOON-too[8]）は、Debian GNU/Linuxをベースとしたオペレーティングシステム (OS) である。Linuxディストリビューションの1つであり、自由なソフトウェアとして提供されている。

[Ubuntu - Wikipedia](http://ja.wikipedia.org/wiki/Ubuntu)



## <a name="appendix3">Appendix 3. パッケージ管理システム</a>

### Homebrew — OS X用パッケージマネージャー

Homebrewはパッケージを/usr/local/binへインストール

#### Homebrewのインストール

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

#### バージョン確認

    $ brew --version
    0.9.5

#### パス確認

    $ which brew
    /usr/local/bin/brew

#### パッケージのインストール

    $ brew install パッケージ名  // パッケージのインストール

####  パッケージ一覧確認

     $ brew list        // インストールしたパッケージ
 
    ant libyaml readline sqlite wget
    ios-sim openssl ruby subversion

#### パッケージの更新

    $ brew upgrade     // インストールしたパッケージを更新

#### HomeBrew自体の更新

    $ brew update      // Homebrew自体を更新
 

### npm

node.jsパッケージ管理ツール。
node.jsのインストール時に同時にインストールされる。

 [npm Documentation](https://docs.npmjs.com/)

#### node.js(およびnpm)インストール

下記サイトからpkgファイルをダウンロードしてでインストールする。

[node.js](http://nodejs.org/)


#### バージョン確認
 
   $ npm —version
   1.3.25

#### パス確認

    $ which npm
   /usr/local/bin/npm

#### ヘルプ

$ npm -h        # クイックヘルプ --helpはない
$ nom -l         # display full usage info

#### パッケージのインストール

    $npm install パッケージ名        // カレントフォルダーにインストール
    $npm install -g パッケージ名   // グローバルにインストール

-gをつけてインストールするときはroot権限が必要。
必要に応じてsudoで実行する。

#### パッケージ一覧確認

   $ npm list    // カレントディレクトリにインストールしたパッケージ
   $ npm list -g // グローバルにインストールしたパッケージ

#### パッケージの更新

   $ npm update パッケージ名          // カレントにインストールしたパッケージのアップデート
    $ npm update -g パッケージ名    // グローバルにインストールしたパッケージの更新

をグローバルにインストールしたパッケージを更新するときはroot権限が必要になる。必要に応じてsudoで実行する。

#### npm自体の更新

    $ npm update  npm
    $ npm update  npm -g

npmをグローバルにインストールしているときはroot権限が必要になる。
必要に応じてsudoで実行する。


### gem

rubyパッケージ管理ツール。

#### バージョン確認
 
    $ gem —version

#### パス確認

    $ which gem
   /usr/bin/gem

#### パッケージ一覧確認
 
    $ gem list --local # ローカルのパッケージ表示
    $ gem list --both # ローカル、リモートの両方とも表示。

デフォルトは —localが指定。

#### gemヘルプ
 
    $ gem --help または　$ gem -h
    $ gem bar -h # gem barのヘルプ
    $ gem list -h # 例 gem listのヘルプ


#### パッケージ更新

    $ sudo gem update パッケージ名     // gemでインストールしたパッケージfooの更新

#### gem自体の更新
 
    $ sudo gem update --system # gem自体の更新
 

例) sass, compassの更新

    $ sudo gem update sass
    $ sudo gem update compass


__gemはデフォルトは必ず/usr/binへインストールされる？__


[そろそろ整理しておきたい、Gemコマンドの使い方 - ばくのエンジニア日誌](http://bakunyo.hatenablog.com/entry/2013/05/23/%E3%81%9D%E3%82%8D%E3%81%9D%E3%82%8D%E6%95%B4%E7%90%86%E3%81%97%E3%81%A6%E3%81%8A%E3%81%8D%E3%81%9F%E3%81%84%E3%80%81Gem%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%81%AE%E4%BD%BF%E3%81%84%E6%96%B9)


### wget

> wget とは、UNIXコマンドラインで HTTP や FTP 経由のファイル取得を行えるツールです。
Webサイトであれば、リンク先を階層で指定して一気に取得することができ、オフラインでじっくり読んだり、ミラーサイトを簡単に作ることが可能です。
また、ダウンロードが途中で止まってしまった場合は、途中からやり直すレジューム機能があり便利です。

wget はGNUプロジェクトで作られているフリーソフトです。改良も配布も自由なので、是非活用しましょう。
出典http://tech.bayashi.net/svr/doc/wget.html


### cIRL

> cURL（カール[2]）は、さまざまなプロトコルを用いてデータを転送するライブラリとコマンドラインツールを提供するプロジェクトである。cURLプロジェクトは libcurl と cURL の2つの成果を生んでいる。


### apt

Debian系のパッケージ管理システム

### apt-get

aptを利用したパッケージ管理コマンド
追加したリポジトリの一覧表示方法は?


### rpm

> 米Red Hat社が開発したパッケージ管理システム。パッケージのインストールやアンインストールを行う。インストールの際は依存関係のあるパッケージ(動作に必要な他のパッケージ）を調べ，もしシステムにそれらパッケージが存在しない場合は，その旨をユーザーに通知する。また，アンインストールする際に，そのパッケージが他パッケージと依存関係がある(他のパッケージの動作に必要とされる)場合には，アンインストールできない旨を通告する。さらに，既にシステムにインストール済みのパッケージをチェックすることも可能。パッケージのインストール，アンインストール，アップデートには管理者権限が必要。
 

### yum

> Yellowdog Updater Modified (Yum ヤム)はLinuxのRPM Package Managerのパッケージを管理するメタパッケージ管理システムである。Yumは デューク大学のLinux@DUKEプロジェクトでセス・ヴィダル（英語版）を始めとするボランティアによって開発された。

yumはパッケージの置き場であるレポジトリを登録する必要がある。

#### yumインストール

    $apt-get install yum

#### レポジトリインストール

(例) epelレポジトリ

レポジトリのインストールにはrpmが必要。

    apt-get install rpm


1. レポジトリファイルを取得

    $ wget https://dl.fedoraproject.org/pub/epel/6/x86_64/2048-cli-0.9-4.git20141214.723738c.el6.x86_64.rpm
 
2. レポジトリファイルのインストール

    $ rpm --upgrade --verbose --hash 2048-cli-0.9-4.git20141214.723738c.el6.x86_64.rpm


Ubuntuはrpmコマンドを使うとrpm: RPM should not be used directly install RPM packages, use Alien instead!のメッセージ。
alienでレポジトリをインストール

    // alienインストール
    $ apt-get install alien
    // レポジトリインストール
    alien 2048-cli-0.9-4.git20141214.723738c.el6.x86_64.rpm

[CentOS6でRPMforge、Remi、EPELをyumレポジトリに追加する方法](http://dqn.sakusakutto.jp/2013/02/centos6_yum_remi_epel_rpmforge.html)


    Setting up Local Package Process
    Examining /var/tmp/yum-root-3fSaTt/ius-release-1.0-11.ius.centos6.noarch.rpm: ius-release-1.0-13.ius.centos6.noarch
    Marking /var/tmp/yum-root-3fSaTt/ius-release-1.0-11.ius.centos6.noarch.rpm to be installed
    Resolving Dependencies
    --> Running transaction check
    ---> Package ius-release.noarch 0:1.0-13.ius.centos6 will be installed
    --> Processing Dependency: epel-release = 6 for package: ius-release-1.0-13.ius.centos6.noarch
    --> Finished Dependency Resolution
    Error: Package: ius-release-1.0-13.ius.centos6.noarch (/ius-release-1.0-11.ius.centos6.noarch)
               Requires: epel-release = 6
     You could try using --skip-broken to work around the problem
     You could try running: rpm -Va --nofiles --nodigest

#### パッケージインストール

    $ sudo apt-get install yum-utils
    $ sudo yum-config-manager --enable

### Bundler

rubyの依存関係解決の標準パッケージ管理ツール。



## <a name="appendix4">Appendix 4. Ubuntu</a>

### Apache + MySQL + PHPのインストール

    $ sudo apt-get update

    $ sudo apt-get install apache2
    $ sudo apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql
    $ sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt

### Apache設定ファイル

/etc/apache2/apache2.conf



## <a name="appendix4">Apenddix 4. Nginx</a>

### プログラム

    $ which nginx
    /usr/sbin/nginx


### 起動・停止・再起動

[ubuntuでnginxの起動と最低限のコマンド](http://joppot.info/2014/03/10/970)


### 設定ファイル

    /etc/nginx/nginx.conf

nginx.confに下記記載がある。

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;

### sites-available, sites-enabled

    /etc/nginx/sites-available
    /etc/nginx/sites-enabled

#### site-available/default

__初期状態ではlocationがコメントアウトされておりphpファイルへアクセスするとダウンロードしてしまう。下記のようにコメントアウトを外す。__

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


sites-availableディレクトリに記載したファイルへのシンボリックリンクをsite-enabledに置く。

[軽量で高速なウェブサーバNginxを、Ubuntu 12.04に導入する(設定編その１) | 近藤嘉雪のプログラミング工房日誌](http://blog.kondoyoshiyuki.com/2012/12/09/setting-1-nginx-on-ubuntu-12-04/)


### ログ

nginx.confで設定する。

    ##
    # Logging Settings
    ##

    access_log /var/log/sginx/access.log;
    error_log /var/log/nginx/error.log;

[nginxのログ出力変更 - Qiita](http://qiitj.com/hito3/items/0e539e82ee3c410cccf1u)



## <a name="appendix5">Appendix 5. PHP</a>

### PHP実行環境

* Apache + モジュール/CGI
    - モジュール
    Apache + php_mod: Apacheのモジュールとして動かす
    - CGI
    Apach + php-cgi: ApacheからCGIとして呼び出す
* Nginx + php-fpm
  [PHP: FastCGI Process Manager (FPM) - Manual](http://php.net/manual/ja/install.fpm.php)
  NginxのCGIからCGIとして呼び出す
* CLI(Command Line Interface)
  コマンドで実行する


### php.iniの確認

* ブラウザ
  phpinfo関数をブラウザでアクセスして実行する
  Configuration File (php.ini) ,Path/Loaded Configuration Fileを確認する。
* CLI
  $ php -r 'phpinfo();' | grep php.ini


### 実行環境の例(phpinfo関数をブラウザで実行)

| PHP | PHP Version 5.5.9-1ubuntu4.5 |
|-----|-----|
|Configuration File (php.ini) Path|/etc/php5/fpm|
|Loaded Configuration File|/etc/php5/fpm/php.ini|
|Scan this dir for additional .ini files|/etc/php5/fpm/conf.d|
|Additional .ini files parsed|/etc/php5/fpm/conf.d/05-opcache.ini, /etc/php5/fpm/conf.d/10-pdo.ini, /etc/php5/fpm/conf.d/20-curl.ini, /etc/php5/fpm/conf.d/20-json.ini, /etc/php5/fpm/conf.d/20-mcrypt.ini, /etc/php5/fpm/conf.d/20-mysql.ini, /etc/php5/fpm/conf.d/20-mysqli.ini, /etc/php5/fpm/conf.d/20-pdo_mysql.ini, /etc/php5/fpm/conf.d/20-readline.ini, /etc/php5/fpm/conf.d/20-xdebug.ini, /etc/php5/fpm/conf.d/20-xsl.ini|
|PHP API|20121113|
|PHP Extension|20121212|
|Zend Extension|220121212|


### PHPの実行ユーザーの確認

<?php echo `whoami`; ?>


### PECL :: The PHP Extension Community Library

[PECL :: The PHP Extension Community Library](http://pecl.php.net/)

#### PECLの代表的ライブラリ

* [PECL :: Package :: imagick](http://pecl.php.net/package/imagick)
* [PECL :: Package :: oauth](http://pecl.php.net/package/oauth)

#### PECLライブラリのインストール

> PECLのインストール用には、PEAR同様に「pecl」コマンドが提供されている。インストール方法もほぼPEARと同じだが、インストール後に設定ファイル（php.ini）の「extension」でインストールしたモジュールを指定する必要がある点が異なる。
Wiki

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

[プログラミング日誌 :: LinuxのPHPに拡張モジュールを入れる方法](http://nb-tech.doorblog.jp/archives/51670170.html)

##### ライブラリ配置ディレクトリ

    /usr/lib/php5/20121212   <- どのファイルでこのディレクトリがPECLライブラリ保存先として指定しているかは不明

    curl.so  mcrypt.so  mysql.so    pdo_mysql.so  readline.so  xsl.so
    json.so  mysqli.so  opcache.so  pdo.so


##### extension=\<library name\>

php.iniに追記せず/etc/php5/fpm/conf.dディレクトリに各ライブラリごとのiniファイルがありextension=\<library name\>の記載がされている。

    vagrant@develop:/etc/php5/fpm/conf.d$ ls
    05-opcache.ini  20-curl.ini  20-mcrypt.ini  20-mysql.ini      20-readline.ini  20-xsl.ini
    10-pdo.ini      20-json.ini  20-mysqli.ini  20-pdo_mysql.ini  20-xdebug.ini


#### extensionとzend_extension

[PHP extensionとZend extensionの違い - hnwの日記](http://d.hatena.ne.jp/hnw/20130715)


### ログの設定

ログをファイルへ保存する設定をphp.iniへ記載。


    display_errors = Off

    log_errors = On
    error_log = <path>



## <a name="appendix6">Appendix 6. CakePHP</a>

### デバックレベル

app/Config/core.php

    Configure::write('debug', 2);



## <a name="appendix7">Appendix 7. Ruby</a>

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



## <a name="appendix8">Appendix 8. Berkshelfはbundleで管理して使うとエラーがで
るのでChefDKを使う</a>

[ChefDk の berkshelf で cookbook をダウンロードする - Qiita](http://qiita.com/shin1x1/items/872cf5b9396516068892)
[Chef Development Kit | Chef Downloads | Chef](https://downloads.chef.io/chef-dk/mac/#/)


## Appendix 8. behatインストールで発生したエラーへの対応

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



## <a name="appendix9">Appendix 9. JenkinsのビルドでAPCの書き込みエラーが発生
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



## <a name="appendix10">Appendix 10. gitのエディタを変更</a>

Ubuntuでnanoをviへ変更

git config --global core.editor "vi"

[gitのエディタをnanoから他へ変更する | J-Linuxer](http://jlinuxer.dip.jp/?p=645)


## <a name="appendix11">Appendix 11 用語</a>

> .soファイル 【 shared object file 】 .so形式 / .soフォーマット

> soファイル / so形式
> UNIX系OSの共有ライブラリのファイル形式。拡張子が「.so」であることからこのように呼ばれる。実行可能形式のプログラムが格納されているが、単体で起動することはできず、他のプログラムにリンクしてその機能を呼び出すようになっている。
>この形式のファイルはプログラムの実行時にリンクされる動的リンク(ダイナミックリンク)ライブラリで、ビルド時にリンクされる静的リンク(スタティックリンク)ライブラリの場合は「.a」(archiveの略)という拡張子になる。

[.soファイルとは 〔 soファイル 〕 【 shared object file 】 - 意味/解説/説明/定義 ： IT用語辞典](http://e-words.jp/w/2EsoE38395E382A1E382A4E383AB.html)



## <a name="appendix12">Appendix 12. 英語</a>

* fixture by Weblio
  定着[固定]物，据え付け品; 取り付け具，備品
* Fabric by Weblio
1a【不可算名詞】 [種類・個々には 【可算名詞】] 布，織物 《★【比較】 cloth のほうが一般的》.
用例
enough fabric to make a coat 上着を作るのに十分な布.
b【不可算名詞】 織り方，生地.
2[単数形で]
a[集合的に] (教会などの)建物の外部 《屋根・壁など》.
b構造，組織 〔of〕.
* scaffold
音節scaf・fold 発音記号/skˈæf(ə)ld, ‐foʊld｜‐fəʊld/音声を聞く
【名詞】
1【可算名詞】
a(建築現場などの)足場.
b(ビルの窓をふく時に使うような)吊り足場.
2a【可算名詞】 絞首台，断頭台.
b[the scaffold] (絞首・断頭による)死刑.
用例
go to [mount] the scaffold 絞首台に登る, 死刑に処せられる.
3【可算名詞】 (野外の)組み立て舞台[ステージ，スタンド].
【動詞】 【他動詞】
〈建物に〉足場を設ける.


## <a name="appendix13">Appendix 13. Jenkinsのパス</a>

/var/lib/jenkins/jobs/blogapp/workspace/


