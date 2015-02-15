# 制作フロー

* [制作前チェックリスト](#checklist-production)
  + [レスポンシブ](#responsive)
  + [案件タイプ](#project-type)
  * [別ウィンドウ](#window)
* [公開前チェック](#checklist-launch)
* [SEOなど](#seo)

## <a name="checklist-production">制作前チェックリスト

### <a name="#responsive">レスポンシブ

  IE8はメディアクエリに対応していない。
  Twitter BootstrapのresponsiveはIE8では崩れる(rowクラス)。


### <a name="project-type">案件タイプ

+ WordPressサイト構築
     + PC
     + タブレット
     + スマートフォン
     + フィーチャーフォン
+ WordPressプラグイン開発
+ WEBサービス開発


### 既存サイト調査

* SiteSucker Mac
* Website Explore IE


### SNS

* リニューアルのさいは既存サイトSNSがリニューアル後と整合するか確認するために旧サイトのキャプチャをとる


### <a name="window">別ウィンドウ

* 外部リンクは別ウィンドウ
* 内部リンクは同一リンク


## <a name="checklist-launch">公開前チェックリスト

### WordPress

#### デバッックモード停止

wp-config.phpのデバックをfalseにする

#### 検索エンジン表示

設定 &gt; 表示設定の下記のチェックをはずす。

検索エンジンでの表示 検索エンジンがサイトをインデックスできないようにする

#### セキュリティー

セキュリティー面のチェックを忘れない。

#### サーバーファイル

下記ファイルはアップロードしない。

* readme.html
* readme-ja.html
* licence.txt
* wp-config-sample.php

#### ピンバック

ピンバックを解除するときは下記のチェックをはずす。

設定 > ディスカッション > この投稿に含まれるすべてのリンクへの通知を試みる


### リダイレクト

リダイレクト元をサーバから削除する。


### リンクチェック

#### Firefox

Page Link Checks Firefox extension


### 画像alt属性


Web Developer


### Facebook Like Box

IFRAMEで設置する。HTML5は読み込みが遅いときがある



## スマートフォン iPhone

* 実機
* iOS シミュレータ
* Safari > 開発
* Firefox デフォルト ツール > 開発 > レスポンシブデザインビュー


## その他

* robots.txt
* favicon.ico
* 画像のwidth, height
  原則指定しないがスクリプトで切り替えるときは指定する。指定しないとIEで画像サイズをうまく取得できない。
* _blank



## <a name="seo">SEO

### <a name="google">Google Sitemap

WordPressはプラグイン(Google XML Sitemapsなど)でsitemap.xmlを作成できる。
sitemap.xmlは通常bloginfo( 'url' )の直下に作成される。

ウェブマスター ツールにsitemap.xmlを送信する。
