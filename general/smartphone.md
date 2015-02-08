Smartphone
==========

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