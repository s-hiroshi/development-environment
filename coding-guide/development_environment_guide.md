開発環境整備
=====

* [IDE](#ide)
* [フォルダー構成](#folder)
* [デバッグ](#debug)
* [品質管理](#quality)
* [自動化](#auto)
* [テンプレート化](#template)
* [スニペット](#snippets)

IDE<a name="ide">
-----

PhpStorm

<a name="folder">フォルダー構成
-----

* [クライアントテーマ開発](#client-theme)
* [プラグイン開発](#plugin)


### <a name="client-theme">案件テーマ開発

```
my-project
    |
    |- cloaking    GitHubで管理するときはcloakingをwp-contentやthemesに配置することもある
        |
        |-- contract
        |
        |-- database
        |
        |-- design
        |
        |-- fm_client
        |
        |-- legacy
        |
        |-- specification
    |
    |- server
        |
        |-- public_html
            |
            |-- wordpress
                |
                |-- wp-content
                    |
                    my-theme
                        |
                        |-- .sass-cache    # Compassが自動で作成
                        |
                        |-- font-awesome   # Web Font
                        |
                        |-- bootstrap      # CSS Frame Work
                        |
                        |-- css
                             |
                             |-- style_body.css        # Grunt, Compassで作成
                             |
                             |-- style_body.min.css    # Gruntで作成
                             |
                             |-- style_header.css
                             |
                        |
                        |-- docs          # documentation
                        |    |-- css      # StyleDocco
                             |
                             |-- js       # YUI Doc
                             |
                             |-- php      # phpDocumentor
                        |
                        |-- grunt
                        |
                        |-- images
                            |
                            |-- front    # homeではなくfront
                                 |
                                 |-- graphic_main.jpg # カスタムヘッダーのときはデフォルト画像
                            |
                            |-- menu
                                 |
                                 |-- menu_global
                        |
                        |-- js
                        |
                        |-- node_modules
                            |
                            |-- grunt
                            |
                            |-- その他のプラグイン
                        |
                        |-- sass        # Sassコンポーネント
                            |
                            |-- _variable.scss
                            |
                            |-- ...
                        |
                        |
                        |-- test
                            |
                            |-- js           # Phantomjs + Qunit
                            |
                            |-- php          # PHPUnit
                        |
                        |-- config.rb        # Compass設定ファイル
                        |
                        |-- Gruntfile.js     # Grunt処理記載ファイル
                        |
                        |-- index.html
                        |
                        |-- package.json     # Grunt設定ファイル
                        |
                        |-- style.css
                        |
                        |-- style_body.scss
                        |
                        |-- screenshot.png
                        |
                        |-- class-example.php    #クラスはWordPress管理画面から編集するためにテーマの直下に配置

```

### <a name="plugin">自社プラグイン開発

```
my-plugin
    |
    |-- admin
    |
    |-- docs
        |
        |-- css StyleDocco
        |
        |-- js  YUI Doc
        |
        |-- php phpDocumentor
    |
    |-- includes
    |
    |-- language
        |
        |-- my-plugin-ja.mo
        |
        |-- my-plugin-ja.po  配布ファイルには含まなくて良い(potと各言語のmoがあればよい)
        |
        |-- my-plugin.pot
    |
    |
    |-- my-plugin.php
    |
    |-- exclude.lst zip圧縮の除外リスト設定ファイル
    |
    |-- readme.txt
    |
    |-- test
        |
        |-- js Phantomjs + Qunit
        |
        |-- php PHPUnit

```


<a name="debug">デバッグ
-----

* JavaScript
Firebug
* PHP
PhpStorm &plugs; Xdebug


<a name="quality">品質管理
-----

* [コード整形](#cordeformat);
* [ドキュメンテーション](#documentation)
* [テスト](#test)
* [バージョン管理](#vcs)
* [CSSプリプロセッサ](#csspre)
* [テンプレート化](#template)

### <a name="codeformat">コード整形

* JavaScript JSLint/JSHint
* PHP PhpStorm

### <a name="documentation">ドキュメンテーション

* CSS StyleDocco
* JavaScript YUIDoc
* PHP phpDocumentor

```
docs
    |
    |-- css
    |
    |-- js
    |
    |-- php
```

##### StyleDocco

    my-theme$ styledocco -n "my-theme" -o "docs/css"

##### YUI Doc

    my-theme$ yui -o ./docs/css

##### phpDocumentor

    phpdoc -d . -t ./docs/php

-d directory
-t target

### <a name="test">テスト

* JavaScript Phantomjs + QUnit
* PHP PHPUnit

### <a name="vcs">バージョン管理

* Git

#### .gitignore

```
.DS_Store
Thumbs.db
.idea
.gitignore
.sass-cache
svn
```

### <a name="csspre">CSSプリプロセッサ

* Sass
* Compass

##### Sass

##### Compass

    my-project$ compass create
    my-project$ compass create --bare

フレームワークの場所

    /Library/Ruby/Gems/1.8/gems/compass-0.12.2/frameworks/compass/stylesheets

config.rg

    # Require any additional compass plugins here.

    # Set this to the root of your project when deployed:

    css_dir = "css"
    sass_dir = "."
    image_dir = "images"

    # You can select your preferred output style here (can be overridden via the command line):
    # output_style = :expanded or :nested or :compact or :compressed

    # To enable relative paths to assets via compass helper functions. Uncomment:
    relative_assets = true

    # To disable debugging comments that display the original location of your selectors. Uncomment:
    line_comments = false


    # If you prefer the indented syntax, you might want to regenerate this
    # project again passing --syntax sass, or you can uncomment this:
    # preferred_syntax = :sass
    # and then run:
    # sass-convert -R --from scss --to sass sass scss && rm -rf sass && mv scss sass



<a name="auto">自動化
-----

PhpStorm,  Gruntで自動化する。

### Grunt

WordPressのstyle.cssをミニファイする。

1. Compassでstyle_body.scssからcssディレクトリへstyle_body.cssを作成する。
2. css/style_body.cssをミニファイしてstyle_body.min.cssを作成する。
3. style_header.cssへstyle_body.min.cssを結合してstyle.cssを作成する

##### config.rb(Compass)

    css_dir = "css"
    sass_dir = "."
    image_dir = "images"


##### プラグインインストール

    $ npm install grunt --save-dev
    $ npm install grunt-contrib-cssmin --save-dev
    $ npm install grunt-contrib-concat --save-dev
    $ npm install grunt-contrib-compass --save-dev


##### フォルダ構成

    my-theme
    |
    |-- css
        |-- style_header.css  # テーマスタイルのヘッダー用
    |-- config.rb             # Compass設定ファイル
    |-- Gruntfile.js
    |-- package.json
    |-- style_body.scss       # テーマスタイルのボディー用


Gruntfile.js

    module.exports = function(grunt) {
        grunt.initConfig({
            compass: {
                dist: {
                    options: {
                        config: 'config.rb'
                    }
                }
            },
            cssmin: {
                compress: {
                    files: {
                        'css/style_body.min.css': [ 'css/style_body.css' ]
                    }
                }
            },
            concat: {
                dist: {
                    src: ['css/style_header.css', 'css/style_body.min.css'],
                    dest: 'style.css'
                }
            }
        });

        grunt.loadNpmTasks( 'grunt-contrib-compass' );
        grunt.loadNpmTasks( 'grunt-contrib-cssmin' );
        grunt.loadNpmTasks( 'grunt-contrib-concat' );

        // Default task(s).
        grunt.registerTask( 'default', [ 'compass', 'cssmin', 'concat' ] );
    };


##### コマンド実行

    my-theme $ grunt



<a name="template">テンプレート化
-----

### WordPress テンプレートファイル

汎用できるものは全て下記テーマに一元化する。

     /%XAMPP%/htdocs/github/wordpress/wp-content/themes/develop

### Sass

できるだけコンポーネント化して下記へ一元化する。

    /%XAMPP%/htdocs/github/snippets/css/sass


Snippets
-----

一元化する。
