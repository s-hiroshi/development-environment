## 案件

### ツール

* CSS プリプロセッサー
    + Sass/Compass
* ドキュメンテーションツール
    + CSS  
      hologram
    + JavaScript  
      YUI Doc
* テスト
	+ QUnit
* コードインスペクション
	+ WordPress  
	  PHP_CodeSniffer
    + JavaScript  
      JSLint, JSHint
* デプロイ
    + grunt-sftp-deploy
* 自動化ツール Grunt


### フォルダ構成の例

	mytheme
	|
	|— css						// サードパーティーCSS
	|
	|- dev
		|
		|— css					// コンパイルして mytheme/style.cssを作成
			 |— _style1.scss
			 |- _style2.scss
			 |..
			 |- _styleN.scss
			 |..
			 |- _style.scss
		
		|— js					// grunt-contrib-concatで結合してmytheme/js/mytheme.js作成
			 |— script1.js
			 |- script2.js
			 |..
			 |- scriptN.js
			 |
			 |-- test			// grunt-contrib-qunitで自動化 
			 	|
			 	|-- qunit-1.19.0.js
			 	|-- qunit-1.19.0.css
			 	|-- test.html
			 	|-- test.js
		|
		|- jsdocs				// YUIDocフォルダ grunt-contrib-yuidocでJavaScrptドキュメンテーション作成
		|
		|- node_modules
		|
		|- phpcs				// PHP_CodeSniffer
		|
		|- styledocs            // hologramフォルダ grunt-hologramでスタイルガイド作成
		|
		|- wpcs					// WordPress-Coding-Standardsフォルダ
		|
		|- ftppass				// grunt-sftp-deployのFTP情報ファイル
		|
		|- config.rg			// Compass設定ファイル           
		|
		|— Gruntfile.js         // Grunt設定ファイル
		|
		|- hologram_config.yml	// hologram設定ファイル
		|
		|— package.json         // Gruntパッケージファイル
	|
	|- images					// テーマの画像 
	|
	|- js						//  mytheme.jsとサードパーティーのスクリプト
	|
	|- index.php
	|
	|- style.css


### Sass

CSSプリプロセッサー。

[Sass: Syntactically Awesome Style Sheets](http://sass-lang.com/)

#### Sassインストール

    $ sudo gem install sass

#### コンパイル

style.scssをコンパイルし同フォルダにstyle.cssを作成する例。

    $ sass style.scss style.css

#### 変更を監視して自動コンパイル

    $ sass --watch style.scss:style.css

ctrl + Cで監視を停止する。

### Compass

Sassを使ったCSS作成フレームワーク。

[Compass Home | Compass Documentation](http://compass-style.org/)  
[Sass/Compass のインストールと基本的な環境設定 | Web Design Leaves](http://www.webdesignleaves.com/wp/htmlcss/652/)

#### インストール

    $ sudo gem install compass
 
#### 初期化

    $ compass create --bare
    
contrib.rbとsassフォルダが作成される。

#### config.rb(Compass設定ファイル)

    $ cd /path/to/mytheme/dev
    $ compass create --bare

	mytheme
    |
    |—dev
    |  |— css
    |  |    |— style.scss
    |  |
    |  |— contrib.rb
    |
    |—index.html
    |
    |- style.css

contrib.rbの設定例

    // 設定
    css_dir = ".."
    sass_dir = “css”
    // compass専用コメントの出力を停止
    line_comments = false

上記例ではcompassコマンドでコンパイルするとdev/cssフォルダのモジュールファイルを除いたscssファイルをコンパイルしmythemeディレクトリへ出力する。

#### 変更を監視して自動コンパイル

    $ compass watch css/style.scss

### Grunt

[Grunt: The JavaScript Task Runner](http://gruntjs.com/)

Gruntはgrunt-cliのみグローバルへインストールしGrunt本体も含めて各パッケージはプロジェクトごとのフォルダへインストールする。

#### 1. node/npmのインストール

[node.js](http://nodejs.org/)

Gruntはnode.js/npmを使う。

node.jsはサイトからpkgファイルをダウンロードしインストールする。  
npmはnode.jsと一緒にインストールされる。

#### 2. grunt command line interface(grunt-cli)をグローバルへインストール

	$ sudo npm install -g grunt-cli

#### 3. プロジェクトフォルダにpackage.jsonを作成

	{
	  "name": "example",
	  "version": "0.0.1"
	}

#### 4. プロジェクトフォルダにGrunt本体とプラグインをインストール

grunt本体とプラグインはプロジェクトごとにプロジェクトフォルダへインストールする。

#### 5. Grunt本体のインストール

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

### hologram

スタイルガイド。

### インストール

	$ sudo gem install hologram

### Gruntへ導入

	$ npm install grunt-hologram --save-dev

### hologram init

Gruntfile.jsがあるフォルダで下記コマンド実行します。

	$ hologram init

設定フィルhologram_config.ymlが作成されます。

### Gruntfile.js

Gruntfileへ下記を記載します。

	grunt.loadNpmTasks('grunt-hologram');
	
	hologram: {
		generate: {
			options: {
				config: 'hologram_config.yml'
			},
		},
	},

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
			name: 'mytheme',
			description: 'mytheme documentation.',
			version: '0.0.1',
			url: 'http://www.example.com',
			options: {
				paths: "js",
				outdir: "jsdocs"
			}
		}
	}

### SFTPを使ったデプロイ

[thrashr888/grunt-sftp-deploy · GitHub](https://github.com/thrashr888/grunt-sftp-deploy)


### PHP_CodeSnifferとWordPress-Coding-Standards

下記リンクにPHP_CodeSnifferをスタンドアローンでインストールする方法が記載してある。

[WordPress-Coding-Standards/WordPress-Coding-Standards](https://github.com/WordPress-Coding-Standards/WordPress-Coding-Standards)

mytheme/dev/phpcs、mytheme/dev/wpcs

	cd /path/to/mytheme/dev
	git clone https://github.com/squizlabs/PHP_CodeSniffer.git phpcs
	git clone -b master https://github.com/WordPress-Coding-Standards/WordPress-Coding-Standards.git wpcs
	cd phpcs
	./scripts/phpcs --config-set installed_paths ../wpcs

#### Gruntへの設定

	phpcs: {
		application: {
			src: ["../*.php"]
		},
		options: {
			bin: './phpcs/scripts/phpcs',
			standard: "WordPress"
		}
	},

#### QUnitの自動化

Qunitの自動化はgrunt-contrib-qunitを使います。grunt-contrib-qunitはPhantomJSで実行されます。  
grunt-contrib-qunitはgrunt-lib-phantomjsも同時にインストールします。


	$ npm install grunt-contrib-qunit --save-dev


QUnitはCDNから読み込むのではなくローカルに保存して読み込んでください。

### 静的HMTLへ書き出し

StaticPressを使い静的ファイルを作成します。

下記エラーが発生しました。

	Fatal error:  Maximum execution time of 30 seconds exceeded in /var/www/wordpress/wp-content/plugins/staticpress/includes/class-static_press.php on line 956

php.iniのmax_execution_timeを30から200へ変更しました。

	max_execution_time = 200