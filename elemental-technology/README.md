# 要素技術

## 目次

* [SQL](#sql)
* [MySQL](#mysql)
* [Git](#git)
* [SSH](#ssh)
* [正規表現](#regex)
* [Smartphone](#smartphone)
* [iOSプログラム](#ios)
* [Android](#android)
* [ColdFusion](#coldfusion)



# <a name="sql">SQL</a>

## キーワード

    関係代数
    CASE
    INNER JOIN
    UNION ALL
    サブクエリ
    カーソル
    メモリテーブル
    ISNULL (Transact-SQL)



##  関係代数

> 1) SQLと関係代数
> 
> 関係モデルでは、行を(列名、データ型、列の値)の組み合わせを要素とする集合としてとらえ、表を行という要素の集合としてとらえる。これらのデータの演算は、関係代数という演算体系において定義されており、SQLにおけるデータ加工の命令も、関係代数に基づいている。
> 
> 2) 関係代数の基本的な演算
> 
> 関係代数で定義されている基本的な演算を、RDBMSの用語に置き換えて表現すると、以下のようになる。(図参照)
> 
> * 通常の集合演算
> 
> 以下の演算は、表を行という要素の集合としてとらえた場合の、通常の集合演算となる。
> 
> - 和 A∪B
> 
> 表Aに属するまたは表Bに属する行で構成される表。
> 
> 型適合(AとBとで列数、列名、各列のデータ型が一致)の場合に可能な演算。
> 
> 両方に属する行を含む。同じ行の重複は取り除く。
> 
> - 差 A－B
> 
> 表Aに属するかつ表Bに属さない行で構成される表。
> 
> 型適合の場合に可能な演算。
> 
> - 積 A∩B
> 
> 表Aに属するかつ表Bに属する行で構成される表。
> 
> 型適合の場合に可能な演算。「共通」「交わり」とも呼ばれる。
> 
> 積は、差を用いて、A∩B＝A－(A－B) で表現される。
> 
> - 直積 A×B
> 
> 表Aに属する行と表Bに属する行との全ての組み合わせで構成される表。
> 
> A×Bの要素となる各行は、Aに属する行とBに属する行との組み合わせとなる。
> 
> 「デカルト積」とも呼ばれる。
> 
> - 商 A÷B
> 
> C×B＝Aとなるような表C。
> 
> 商は関係代数では基本的な演算だが、SQLでは対応した命令が存在しない。
> 
> * 関係代数独自の演算
> 
> - 選択
> 
> 表の行のうち、指定した条件に一致する行で構成される表。
> 
> 「制限」とも呼ばれる。
> 
> - 結合
> 
> 表の直積演算の結果となる表に、選択演算を適用した結果の表。
> 
> 表Aと表Bとの直積となる表に、選択演算を適用した結果の表を、AとBとの結合という。
> 
> - 射影
> 
> 表の各行から指定した列を取り出した行で構成される表。
> 
> 列は複数指定も可能。

[I-22-3. 集合演算の概念 | 日本OSS推進フォーラム](http://ossforum.jp/en/node/710)



## 結合

[結合の図](https://cacoo.com/diagrams/zUGZdgTqhVmHjXcl)

### 内部結合(INNER JOIN)

	SELECT field1, field2 FROM table_left INNER JOIN table_right ON table_left.field1 = table_right.field1


### 外部結合(OUTER JOIN)

左の表を全て残す。左表は条件節を満たす行が右表になくてもNULLで埋めて全て取り出す(下記2つは等価)。

	SELECT field1, field2 FROM table_left LEFT OUTER JOIN table_right ON table_left.field1 = table_right.field1
	SELECT field1, field2 FROM table_left LEFT JOIN table_right ON table_left.field1 = table_right.field1

RIGHT OUTER JOINは右の表を全て残す。


### クロス結合(直積)

両テーブルの直積。MySQLはJOINで条件説(ON、USING)が指定されていないときはクロス結合する(下記3つは等価)。

	SELECT field1, field2 FROM table_left CROSS JOIN table_right
	SELECT field1, field2 FROM table_left JOIN table_right
	SELECT field1, field2 FROM table_left ,table_right



## Transact-SQL

### データの格納

#### 値を格納(変数宣言と値設定)

宣言

    DECLARE @myvariable 型(INT, VARCHARなど)

設定

    SELECT @myvariable = mytable.myvariable FROM mytable
    -- SET @myvariable = (SELECT myvariable FROM mytable) -- 上記と同じ

#### 結果レコード格納

* WITH文
* メモリテーブル \#memorytable  メモリテーブルは先頭に\#を付ける

##### メモリテーブル

    SELECT
         field1
        ,field2
    INTO #memorytable FROM
    (
        SELECT
             field1
            ,field2
        FROM mytable
        WHERE field3 = 1
    )
    
[SELECT INTO を使用した行の挿入](http://technet.microsoft.com/ja-jp/library/ms190750%28v=sql.105%29.aspx)

#### 結果レコードの取り出し

##### カーソル(CURSOR)

    DECLARE my_cursor CURSOR
        FOR SELECT
             col1
            ,col2
        FROM othertable
        WHERE
            col1 = #memorytable.field1
        AND
            col2 = #memorytable.field2

        OPEN my_cursor

        FETCH NEXT FROM my_cursor
            INTO col1, col2

        WHILE (@@fetch_status = 0)

        BEGIN
            /* レコード処理 */

            FETCH NEXT FROM my_cursor INTO col1, col2
        END

    CLOSE my_cursor
    DEALLOCATE my_cursor

__カーソル名はサフィックスにcursorを付ける__


### 条件分岐

    CASE WHEN THEN ELSE END


### IF文

    IF 条件式
        BEGIN
            /* 処理 */
        END
    ELSE IF 条件式
        BEGIN
            /* 処理 */
        END
    ELSE
        BEGIN
            /* 処理 */
        END


### ISNULL (Transact-SQL)

[ISNULL (Transact-SQL)](http://msdn.microsoft.com/ja-jp/library/ms184325.aspx)

#### ColdFusionのnullの扱い

> ColdFusion では、null データ型は使用されません。ただし、データベースや Java オブジェクトなどの外部ソースから null 値を受け取った場合は、単純値として使用されるまで null 値が維持され、使用時に空の文字列 ("") に変換されます。また、Java オブジェクトを呼び出す際に JavaCast 関数を使用して、ColdFusion の空の文字列を Java の null に変換することができます。

[Adobe&#160;ColdFusion&#160;9 * データ型](http://help.adobe.com/ja_JP/ColdFusion/9.0/Developing/WSc3ff6d0ea77859461172e0811cbec09af4-7ffa.html)


### 文字列から日付型への変換

Transact-SQL 日付型へ変更可能な文字列 2014/11/26 13:26:03.000


### ユーザー定義関数実装(例としてテーブル値関数)
  
1. Microsoft SQL Server Management Studio起動
2. オブジェクトエクスプローラー接続 
3. データベース選択(Manage)
4. プログラミング > 関数 > テーブル値函数
5. 右クリック > 新しいインラインテーブル関数
6. SQLを記載
7. 実行 - 関数が作成されうる。
  
同名の関数が既にあるときは一度削除して実行する。

### SQL Tips

条件文を動的に作成するときのTIPS。  
WHERE 1 = 1(つねに真)をつけることで条件式をwhere, andでかき分ける必要が無い。

	SELECT col1, col2 form table
	WHERE 1 = 1
	<cfif Trim(col1Value) neq "">
		and  col1 = <CFQUERYPARAM value="#Col1Value#" cfsqltype="CF_SQL_INTEGER">
	</cfif>
	<cfif Trim(col2Value) neq "">
		and  col2 = <CFQUERYPARAM value="#Col2Value#" cfsqltype="CF_SQL_VARCHAR">
	</cfif>



# <a name="mysql">XAMPPと一緒にインストールされたMySQL/phpMyAdmin</a>

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



## <a name="git">ホストOS(Mac)のGit利用</a>

### インストール

[Git - Gitのインストール](http://git-scm.com/book/ja/v1/%E4%BD%BF%E3%81%84%E5%A7%8B%E3%82%81%E3%82%8B-Git%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)


### リポジトリ作成

    $ git init


### リモートリポジトリ作成(GitHubの例)

    $ git remote add origin git@github.com:username/repositoryname.git


### リモートリポジトリの解除

    $ git remote remove origin


### git diffまとめ

	$ git diff              // ワーキングツリーとインデックスの比較
	$ git diff --cached     // インデックスとHEADの比較
	$ git diff HEAD         // ワーキングツリーとHEADの比較
	$ git diff HEAD^..HEAD  // HEAD^とHEADの比較

[git diff の使い方がほんの少し理解できた - murankの日記](http://d.hatena.ne.jp/murank/20110320/1300619118)


### gitのdiffツール

* Araxis Merge for mac


### Araxis Merge for macをgitのdiff/mergeツールとして登録

#### git設定ファイル(.gitconfig)

~/.gitconfig

#### .gitconfigの内容

	[diff]
		tool = araxis
	[merge]
		tool = araxis


### difftoolコマンド

diffでツールを使うときはgit diffの代わりにgit difftoolを使う。

	$ difftool HEAD^..HEAD
	$ difftool HEAD^..HEAD -- readme.txt # ファイルパスを指定


### git reset/checkout

git取り消し(やり直し)のまとめ。

#### reset コミットに対して行う処理

	$ git reset --soft HEAD^    // HEADをHEAD^へ変更 インデックス、ワーキングツリーは変更しない
	$ git reset HEAD^           // HEADとインデックスをHEAD^へ変更、ワーキングツリーは変更しない
	$ git reset --hard HEAD^    // HEAD、インデックス、ワーキングツリーをすべてHEAD^へ変更

resetはファイル単位の変更には対応していない。

## checkout

	$ git checkout file
	$ git checkout 

fileのワーキングディレクトリの内容をHEADへ戻す。



# <a name="ssh">SSH(Secure Shell)</a>

## SSHの認証

1. パスワード認証  
  ユーザー名とパスワードで認証する。
2. 公開鍵認証
  公開鍵/秘密鍵のペアーで認証する。


## 公開鍵認証とは

公開鍵と秘密鍵のペアで認証する。公開鍵はサーバに配置し秘密鍵はクライアンが持つ。  
サーバの公開鍵とペアの秘密鍵を持つクライアントからの接続のみを許可する。

秘密鍵へパスフレーズを設定できる。


## 公開鍵、秘密鍵の作成場所

ssh-keygenコマンドで鍵を作成する。
一般に作成した鍵は/Users/ユーザー名/.sshディレクトリにid_rsa(秘密鍵)とid_rsa.pub(公開鍵)として配置する。

__Macでは秘密鍵が必要なソフトはデフォルトで.ssh/id_rsaを参照する場合ある。__


## 公開鍵、秘密鍵の作成方法

    $ ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (/Users/xxx/.ssh/id_rsa): /Users/xxx/.ssh/id_rsa
    Enter passphrase (empty for no passphrase):    パスフレースを入力
    Enter same passphrase again:  再度パスフレーズを入力

.sshディレクトリにid_rsa(秘密鍵)、id_rsa.pub(公開鍵)が作成される。


## sshコマンド

    $ ssh [オプション] ホスト名 [コマンド]
    $ ssh example.com
    
秘密鍵を指定して接続

    $ ssh -i 秘密鍵のパス example.com


# <a name="regex">PhpStormの正規表現例</a>

## コード整形

### コメント1

    <!-- ?/([a-z0-9-_]+) ?-->

    <!-- $1 -->


before

    <!-- /id_or-scelector -->

after

    <!-- id_or_selector -->


### コメント2

    ^( *)(<!-- /?[a-zA-Z0-9-_]+ -->)(.*)$


    $1$3$2

before

    <!-- content --></div>

after

    </div><!-- content -->

### ()

    \(([a-zA-z0-9$]+)\)

    ( $1 )

before

    if ($example) {

    if ( $example ) {

### フォーマット

    \(([^ )]+)

    ( $1


  
# <a name="smartphone">Smartphone</a>

* [デバイス](#device)
  + [解像度](#resolution)
  + [ブラウザ表示領域](#display)
  + [ブラウザ](#browswer)
  + [フォント](#font)
  + [シェア](#share)
* [開発手法](#way)
* [設計(ルール)](#rule)
  + [ワイヤーフレーム](#wireframe)
* [制作](#creation)
* [テスト・確認](#test)


<a name="device">Device
----------

### <a href="resolution">画面解像度

|デバイス|device pixel ratio|device width|解像度|
|------|-------------------|-----------|-----|
|iPhone5|2|640|640x960|
|iPhone4|2|640|640x960|
|iPhone3|1|320|320x480|
|iPad 第3世代|2|768|1536|
|iPad 第1-2世代|1|768|768|1024|

### <a name="display">ブラウザ表示領域

[iPhone アプリ研究会 Mobile Safariを知る 〜 第一回 画面領域](http://appteam.blog114.fc2.com/blog-entry-236.html)

### <a name="browser">標準ブラウザー

|OS | 端末 | ブラウザ |レンダリングエンジン|
|---|---|---|--- |
|iOS|iPhone|Safari|WebKit |
|Android|一般|ブラウザ|WebKit |
|Android|Nexus|Google Chrome|WebKit |

### <a name="font">フォント

#### ポートレイトとランドスケープのデフォルトのフォントサイズ

ポートレイトとランドスケープを切り替えると自動的にフォンとサイズも変わる。
*ピクセル指定しているときは変更しない。*

[MEMO 4 ME: iPhoneランドスケープ表示でのフォントサイズ](http://memo4me.seesaa.net/article/180558691.html)

#### フォントファミリー

|OS | ゴシック | 明朝 | san serif | serif|
|---|---|---|---|---|
|iOS| ヒラギノ角ゴシック|ヒラギノ明朝| Helveticaなど複数 | Times New Romanなど複数[^1]|
|Android| | | | |


### <a name="share">シェア

Androidバージョン別利用シェア

[http://developer.android.com/about/dashboards/index.html](http://developer.android.com/about/dashboards/index.html)

[^1]:iPhoneアプリTypefacesで確認できる。


<a name="way">開発手法
----------

大きく下記の方法がある。

|ソース|URL|リダイレクト|CSSブレイクポイント|手法|
|---|---|---|---|---|
|単一|同一|なし|あり|Responsive|
|マルチ|同一|あり|なし|UAで振り分け(同一URL)|
|マルチ|異なる|あり|なし|UAで振り分け(同一URL)|

* [Responsive レスポンシブ](#responsive) 単一ソース。
* [リダイレクト](#redirect) マルチソース。
  + [同一URL](#same)
  + [異なるURL](#different)

[WordPress](#wordpress)案件はユーザーエージェントに応じてテーマを切りえるマルチソース,同一URLがよい。
*ポートレイトとランドスケープの切り替えはCSSのブレイクポイントを使う(ハイブリット)。*

### <a name="responsive">Responsive 単一URL, 単一ソース

メディアクエリを使いブレークポイントで適用するスタイルを変える。
レスポンシブデザインは将来、デバイスの解像度が増減しても適用スタイルが変更するだけなので前方互換性が確保される。

### viewport

    <meta name="viewport" content="width=device-width, initial-scale=1">

PCで表示されても問題ない。

### Bootstrap 2.3.1

ブレークポイントは5つ。

*1200px以上*
主にデスクトップ向け

    -------------
    |           |
    |           |
    |           |
    -------------

*980px以上*
主にデスクトップ向け

     ---------
     |       |
     |       |
     ---------

*768px以上*
Tablet

    -------
    |     |
    |     |
    |     |
    -------

*767px未満*
主にiPhone5, iPhone4

    -----
    |   |
    |   |
    |   |
    -----

*480px以下*
iPhone3

    ---
    | |
    | |
    ---

#### 参考サイト
starbucks.com

### <a name="redirect">デバイスに応じて異なるコンテンツを表示

「スマートフォンサイトとPCサイトでURLアドレスは同一であるべきか、それとも別々の異なるURLアドレスとするか」
http://sem.kwhunter.com/2012/01/google-bot-news-for-mobile2.html

#### <a name="same">条件分岐 単一URL、マルチソース

* dominos.jp
* http://www.cocacola.co.jp/
* 楽天オークション


#### <a name="different">異なるURLへリダイレクト マルチURL, マルチソース

* バッファロー


<a name="rule">設計(ルール制定)
----------

* 「このページの上へ」はなし。
* 担当者の機種を確認する。
* 電話をかけられるようにする。
* 地図は標準アプリを使う。
* テーブルは可能な限り使わない。


### <a name="wireframe"> ワイヤーフレーム

* [http://mrare.ca/iphone-ipad-and-browser-wireframe-templates](http://mrare.ca/iphone-ipad-and-browser-wireframe-templates)


<a name="creation">制作
----------

### 画像

* ラスターデータは写真など他の方法(CSS3・Webフォント・SVG)で代替できないとき以外は使わない[^1]。

#### Retinaディスプレイへの対応

iPhone4からはRetinaディスプレイが採用されている。

* ビットマップデーターは縦横2倍の大きさで作成する。
* 画像は2で割り切れるサイズにする。


[^1]: Webフォント, SVGはベクターデータ。SVGはAndroid3.0以前は未対応。<br>
JPG, GIF, PNGはラスターデータ。<br>
画像ソフトで作成したベクターデータもWeb用に上記形式で書き出すとラスターデータになる。
*ただしPNGは拡張性があるのでFireworks PNGのように画像以外のベクターデータを含むものもある。*


<a name="test">テスト・確認
----------

* [iOS](ios.md)
* [Android](android.md)

### WordPress

ユーザーエージェントにようるブラウザの切り替えシュミレーターで確認する。

### オンラインブラウザテスト

* https://saucelabs.com/
* http://www.browserstack.com/start?cbsid=browserlab


WordPressを使ったSmartphone開発
==========

WordPressはユーザーエージェントで適用テーマを変える方法が効率的。

プラグイン
----------

ユーザーエージェントでテーマを切り替えるプラグイン。

プラグイン名|備考
---------|----
[Mobile Detector](http://wordpress.org/plugins/mobile-detector/) |


フィーチャーフォン
==========

http://www.google.com/gwt/n



<a name="ios">iOS シミュレータでiPhoneサイトを確認する</a>

iOS シミュレーターとSafariの開発でチェックするときのメリッット・デメリット

iOS
==========

* Xcode
* iOS Simulator


Xcode Version 4.6
---------

&raquo; [デベロッパツールの概要 - Apple Developer](https://developer.apple.com/jp/technologies/tools/)

    /Applications/Xcode.app

iOS シミュレータ
----------

### iOS シミュレータインストール

Xcodeを起動して下記からシミュレータをダウンロード・インストールする。複数のバージョンを選べる。iOS 6 Simulatorを選択した。2013.2.25

    Xcode.app -> preference -> Download

*iOSシミュレータはXcodeをインストールすると自動的にインストールされるわけではない。*


### iOS シミュレータ起動

##### Xcodeから起動

    Xcode -> Open Developer Tool -> iOS Simulator

##### 2 iOS シミュレータを単体で起動

    /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/Applications


Safari 開発
----------

Safari -> preference -> 詳細 -> 開発をメニューに追加

開発 -> ユーザーエージェント -> iphone

safariの最小幅が380にしかならない。


Script Browser Plus(iPhoneアプリ)
----------

ソースコードを確認。



# <a name="android">Android</a>

Android スマートフォンシミュレータでサイトを確認する。


Android SDK
----------

* Android SDK
http://developer.android.com/sdk/index.html


### Android SDK Manager

* adt-bundle-mac-x86_64-20130219/sdk/tools

### AVD(Android Virtual Device)

Android スマートフォンエミュレーターはAVDを定義して起動する

    ADK Manager > Tools > Manage AVDs..



# <a name="coldfusion">ColdFusion</a>

## 変数スコープ

[Adobe&#160;ColdFusion&#160;9 * スコープでの変数の作成および使用](http://help.adobe.com/ja_JP/ColdFusion/9.0/Developing/WSc3ff6d0ea77859461172e0811cbec22c24-7fd0.html)


# CFSCRIPTとJavaScript

__CFSCRIPTの文法はJavaScriptと似ているがサーバーで処理される。__

## カスタムタグ

カスタムタグはCFMODULEタグで呼び出す。


# CFPARAM

> 宣言されていない場合だけ、値をセットしたい、なんて時に役に立ちます。

[Bugle Diary: [coldfusion]新人研修-cfparamで変数宣言](http://temping-amagramer.blogspot.jp/2007/07/coldfusion-cfparam.html)  
["cfset"と"cfparam": ColdFusionの日記](http://coldfusionlover.seesaa.net/article/248355709.html)

# ColdFusion MXとAjax

jQuery 1.3.2はAjax通信可能だがColdFusionはJSONの返信方法が無いので
プレーンテキスト(またはXML)として出力する。

## Ajax Request

	// submitでフォルト処理停止
	$("#myform").submit( function({
		return false;
	)};
	
	var data = "param1" + $("##param1") + "&param2" + $("##param2");
	$.ajax({
		type: "GET",
		url: "ajax.cfm",
		data: data,
		success: function ( response ) {
			alert( "Success" );
		}
	});

## Ajax Response (ajax.cfm)

	<CFSETTING showDebugOutput="No">
	<CFSILENT>
		<CFPARAM name="param1" default="">
		<CFPARAM name="param2" default="">
	
		<CFSET param1 = (Url.param1)>
		<CFSET param2 = (Url.param2)>
	
		<!--- 施策データを送信用に整形 --->
		<CFSET retParam	= "TEST: #param1# : #param2#">
	</CFSILENT>
	<!--- レスポンスを返す--->
	<CFOUTPUT>
		#retParam#
	</CFOUTPUT>


# EXEファイルの実行タグ CFEXECUTE

EXEファイルを実行するファイル。

# [WDDX](http://ja.wikipedia.org/wiki/WDDX)

[cfwddx](http://help.adobe.com/livedocs/coldfusion/7_jp/htmldocs/wwhelp/wwhimpl/js/html/wwhelp.htm?href=part_cfm.htm)

> CFML データ構造体の、XML ベースの WDDX 形式へのシリアル化およびシリアル化解除を行います。WDDX とは、標準の汎用的な方法で複雑なデータ構造体を記述するための XML の言語の一部です。WDDX を実装すると、アプリケーションサーバープラットフォーム、アプリケーションサーバー、およびブラウザなどの情報に対して HTTP プロトコルを使用できるようになります。
> このタグでは、JavaScript ステートメントを生成して、WDDX パケットや CFML データ構造体の内容に相当する JavaScript オブジェクトのインスタンスを作成します。これには Unicode が使用されます。 


# DB null値の扱い

> ColdFusion では、null データ型は使用されません。ただし、データベースや Java オブジェクトなどの外部ソースから null 値を受け取った場合は、単純値として使用されるまで null 値が維持され、使用時に空の文字列 ("") に変換されます。また、Java オブジェクトを呼び出す際に JavaCast 関数を使用して、ColdFusion の空の文字列を Java の null に変換することができます。

[Adobe&#160;ColdFusion&#160;9 * データ型](http://help.adobe.com/ja_JP/ColdFusion/9.0/Developing/WSc3ff6d0ea77859461172e0811cbec09af4-7ffa.html) WDDX（Web Distributed Data eXchange）とは、プログラミング言語やプラットフォームやトランスポート層に依存しないデータ交換機構であり、異なる環境や異なるコンピュータ間でのデータ交換を実現する。数値、文字列、ブーリアンといった単純なデータ型をサポートし、それらを組み合わせた構造体や配列やレコードなどもサポートする。WDDX は各種言語でサポートされている。Ruby、Python、PHP、Java、C++、.NET、Flash、LISP、Haskellなどがある。
