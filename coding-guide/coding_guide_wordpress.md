## WordPress コーディングガイド

* [テーマスタイルガイド](#theme)
* [プラグインコーディング規約](#plugin)
* [スニペット](#wordpress_snippets)


### <a name="theme">テーマのコーディング規約</a>

#### 参考ガイド

[WordPress Coding Standards &laquo; WordPress Codex](http://codex.wordpress.org/WordPress_Coding_Standards)  
[CSS Coding Standards | Core Contributor Handbook](http://make.wordpress.org/core/handbook/coding-standards/css/)


#### テーマのコーディング規約 目次

* [style.css](#style)
* [CSS利用ツール](#tool)
* [スタイル記述順序](#order)
* [命名規則](#namerule)
* [フォルダ構造](#folder)
* [基本マークアップ](#basemarkup)
* [投稿エディタで入力可能な要素のスタイル](#editor)
* [サイドバーとウィジェット](#sid)
* [bodyクラス](#body)
* [postクラス](#post)
* [グローバルメニュー](#menu_global)
* [ヘッダー](#header)
* [sidebarとwidget](#side)
* [sticky](#sticky)
* [画像関連](#image)
* [ページネイト](#pagenate)
    + シングルページ
    + マルチページ
    + 一覧ページ
* [テーマディレクトリ登録必須スタイル](#themedirectory)



### <a name="style">style.css</a>


	@charset "utf-8";
	/*
	Theme Name: Example
	Theme URI:
	Description:
	Tags:
	Author:
	Author URI:
	Version:
	License: GNU GPL
	License URI: http://www.gnu.org/licenses/gpl-2.0.html
	*/
	
	/*
	Example Theme Copyright 2013 Hiroshi Sawai
	Example Theme is distributed under the terms of the GNU GPL
	*/
	
	/*
	target browser is modern browser
	firefox is latest version
	chrome is latest version
	opera is latest version
	IE is IE8 and more
	*/
	
	/*
	dependency
	jQuery latest version
	Bootstrap latest version
	*/
	
	/*
	compass
	*/
	@import "compass"



### <a name="tool">CSS利用ツール</a>

* Sass
* Compass
* SassDoc



### <a name="order">スタイル記載順序

* import Compass
* variable Sass/Compass
* Mixin    Sass/Compass
* declare utf-8
* html element style
* general classes
* wordpress classes
* boostrap classes
* container
* content
  + main
* header
* footer
* sidebar
* global menu
* breadcrumbs
* category/archive
* static page
* front-page
* post
* custom post type
* custom taxonomy
* clearfix



### <a name="namerule">命名規則

WordPressはクラスは下記命名規則を採用する。

    class-themename-sample.php

functions.phpで呼び出すクラスは同じ階層に配置する(テーマエディターで編集可能にする)。


### <a name="basemarkup">基本構造</a>

Bootstrapを使うことを前提としたマークアップを行う。


### レスポンシブ

__本節は修正が必要。__

IE8はレスポンシブに未対応。以下はIE9以上を仮定する。

3列

    <!DOCTYPE html>
    <html lang="ja">
    <head>
    <link rel="stylesheet" href="<?php echo esc_url( get_template_directory_uri( 'stylesheet_directory' ) ); ?>/bootstrap/css/bootstrap.min.css" media="all">
        <link rel="stylesheet" href="<?php echo esc_url( get_template_directory_uri( 'stylesheet_directory' ) ); ?>/bootstrap/css/bootstrap-responsive.min.css" media="all">
        <link rel="stylesheet" href="<?php echo esc_url( get_template_directory_uri( 'stylesheet_directory' ) ); ?>/awesome/css/font-awesome.min.css" media="all" />
        <link rel="stylesheet" href="<?php bloginfo( 'stylesheet_url' ); ?>" media="screen">
    </head>
    <body <?php body_class(); ?>>

    <div class="container">
        <div id="header">
        </div><!-- header -->
    </div><!-- container -->

    <div class="container">
        <div id="breadcrumbs">
            <?php
            echo Breadcrumbs::get_breadcrumbs();
            ?>
        </div><!-- breadcrumbs -->
    </div><!-- container -->

    <div class="container">
        <div class="content row-fluid">
            <div class="span4">
                <!-- statement -->
            </div><!-- span4 -->
            <div class="span4">
                <!-- statement -->
            </div>
            <div class="span4">
                <!-- statement -->
            </div>
        </div><!-- content row-fluid -->
    </div><!-- container -->

    <div class="container">
        <div id="footer">
            <div id="copy">Copyright &copy; 2005-2013 example All Rights Reserved</div>
        </div>
    </div><!-- container -->

    <?php wp_footer(); ?>
    </body>
    </html>

２列

```
<div class="container">
    <div class="content row-fluid">
        <div id="main" class="span8">
            <!-- statement -->
        </div><!-- span4 -->
        <div id="side" class="span4">
            <!-- statement -->
        </div>
    </div><!-- content row-fluid -->
</div><!-- container -->
```


### <a name="editor">投稿のスタイル</a>

入力エディタで指定可能なスタイルでBootstrapを上書きする必要のあるスタイルを定義する。

Sassの例

     .post {
       p {
           statement
       }
       ol {
           statement
       }
       statement
     }


### <a name="body">bodyへ指定するクラス

    <body <?php body_class(); ?>>

### <a name="post">投稿へ指定するクラス

投稿内容を囲む。

    <div <?php post_class( 'clearfix' ); ?>>

[テンプレートタグ/post class - WordPress Codex 日本語版](http://wpdocs.sourceforge.jp/%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%82%BF%E3%82%B0/post_class)

__ドキュメントは記載していないが固定ページのときはpageクラスが追加される。__



### <a name="menu_global">グローバルメニュー

### ID

    id=menu_global


### マークアップ

    <div id="menu_global_container" class="menu">
        <ul id="menu_global">
            ....
        </ul>
    </div><!-- menu -->


カスタムヘッダー

### functions.php

    /*
     * custom header
     */
    $defaults = array(
        'default-image' => '%s/images/front/graphic_main.jpg',
        'width'         => 1000,
        'header-text'   => false,
    );
    add_theme_support( 'custom-header', $defaults );

### <a name="header">header.php

containerクラスのwidthプロパティがデフォルトのとき。

    <div id="custom_header" class="container">
        <div class="row">
            <div class="span12">
                <img src="<?php header_image(); ?>" alt="" width="<?php echo HEADER_IMAGE_WIDTH; ?>" height="<?php echo HEADER_IMAGE_HEIGHT ?>" />
            </div><!-- custom_header -->
        </div><!-- row -->
    </div><!-- container -->


containerクラスのwidthプロパティをoverrideしているのとき。


    <div id="custom_header" class="container">
        <div class="row">
            <div class="span12">
                <img src="<?php header_image(); ?>" alt="" width="<?php echo HEADER_IMAGE_WIDTH; ?>" height="<?php echo HEADER_IMAGE_HEIGHT ?>" />
            </div><!-- custom_header -->
        </div><!-- row -->
    </div><!-- container -->


## <a name="side">sidebarとwidget

    <div id="side">
        <ul id="sidebar">
            <li class="widget">
                <h2 class="widgettile">
                <ul>
                    <li>...</li>
                    <li>...</li>
                    <li>...</li>
                </ul>
            </li>
            <li class="widget"></li>
            <li class="widget"></li>
        </ul>
    </div><!-- side -->

sidebar識別子はul要素に指定する。  
ウィジェットはsidebar>li要素単位で構成される。



## <a name="sticky">sticky

```css
@charset "utf-8";
.sticky {
    border: 1px solid #000;
}
```



## <a name="image">画像関連

[coding_guide_css_snippets.scss](coding_guide_css_snippets)


## <a name="pagenate">ページネイト

### シングルページ

* <- previous 過去の記事, 古い記事, 前の記事  
  日本語のときは過去の記事で統一する。
* -> next 新しい記事, 次の記事  
  日本語のときは新しい記事で統一する。

表示ページの時間軸を右方向にとる --&gt;

```php
<div id="nav_single">
    <span class="nav-previous"><?php previous_post_link( '%link', '<span class="meta-nav">&larr;</span> 前の記事' ); ?></span>
    <span class="nav-next"><?php next_post_link( '%link', '次の記事 <span class="meta-nav">&rarr;</span>' ); ?></span>
</div><!-- nav_single -->
```


### マルチページ

```css
/* multiple page */
.wp-link-pages {
    margin: 1.5em 0;
}
.wp-link-pages p {
    text-align: center;
}
```


### 一覧ページ(テーブルページネイト)

[http://www.yuriko.net/arc/2008/07/26/navigation/](http://www.yuriko.net/arc/2008/07/26/navigation/)  
[coding_guide_css_snippets.scss](coding_guide_css_snippets)



## <a name="themedirectory">テーマディレクトリ登録必須スタイル

```css
.sticky {}
.bypostauthor {}
.gallery-caption {}
```

# WordPressコーディング規約

* [テーマコーディング規約](#theme)
* [プラグインコーディング規約](#plugin)


__フォルダー構成など開発環境の整備__

&raquo; [development_environment_guide.md](development_environment_guide.md)


## <a name="theme">テーマのコーディング規約

### 参考ガイド

* [WordPress Coding Standards &laquo; WordPress Codex](http://codex.wordpress.org/WordPress_Coding_Standards)


### template_directory

とくべつの理由が無いときはstylesheet_directoryを使う。

    bloginfo( 'stylesheet_directory' );

### <a name="style"> スタイルシート(CSS)関連のガイド

&raquo; [coding_guide_wordpress_style.md](coding_guide_wordpress_style.md)


### style.cssの読み込み

    <link rel="stylesheet" href="<?php bloginfo( 'stylesheet_url' ); ?>" media="screen">


### ループ

#### 方法1

    <?php if( have_posts() ) : while( have_posts() ) : the_post() ?>
    <div <?php post_class( 'clearfix' ); ?>">
        <h2><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h2>
        <p><?php the_content( 'read more' ); ?></p>
        <div class="wp-link-pages"><?php wp_link_pages(); ?></div>
    </div><!-- post -->
    <?php endwhile; endif; ?>

#### 他の投稿を取得するループ 方法2

    <?php $lastposts = get_posts( 'post_type=post&category_name=slugname&posts_per_page=5' ); ?>
    <?php foreach( $lastposts as $post ) : setup_postdata( $post ); ?>
        <h2><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h2>
        <p><?php the_content( 'read more' ); ?></p>
        <div class="wp-link-pages"><?php wp_link_pages(); ?></div>
    <?php endforeach; ?>
    <?php wp_reset_postdata(); ?>


### <a name="wp_head">wp_head

#### 出力制御

    /*
     * remove wp head some action
     */
    remove_action('wp_head', 'wp_generator');
    remove_action('wp_head', 'rsd_link');
    remove_action('wp_head', 'wlwmanifest_link');
    remove_action('wp_head', 'wp_shortlink_wp_head');
    remove_action('wp_head', 'adjacent_posts_rel_link_wp_head');
    remove_action('wp_head', 'feed_links_extra' );



## <a name="plugin"> プラグインコーディング規約

### readme.txt作成

[http://wordpress.org/plugins/about/readme.txt](http://wordpress.org/plugins/about/readme.txt)

### 国際化 i18n pot,mo

配布はpotファイルと自国moファイル。


#### 作成ソフト

[WordPress プラグイン開発者用メモ 多言語リソースファイルの作成手順 | hijiriworld Web][1]

 [1]: http://hijiriworld.com/web/wp-plugin-pot/


### 圧縮

    zip -r foldername ../foldername -x@exclude.lst


#### 除外ファイルの指定(exclude.lst)

    .DS_Store
    Thumbs.db
    exclude.lst

### 登録

» [WordPress › Requests « WordPress Plugins][2]

 [2]: http://wordpress.org/plugins/add/


### Subversion

» [WordPress › WordPress Plugins][3]

 [3]: http://wordpress.org/plugins/about/svn/


### プラグイン固有のスタイル制御

    /*
     * プラグイン固有のスタイルシート制御
     */
    add_action( 'wp_print_styles', 'deregister_styles', 100 );
    function deregister_styles()
    {
        wp_deregister_style( 'plugin-stylesheet-name' );
    }

プラグインのスタイルシートはwp_enqueue_styleで探す。



### <a name="wordpess_snippets">WordPressスニペット</a>

#### wp_headの不要な表示削除

    /*
     * remove wp head some action
     */
    remove_action('wp_head', 'wp_generator');
    remove_action('wp_head', 'rsd_link');
    remove_action('wp_head', 'wlwmanifest_link');
    remove_action('wp_head', 'wp_shortlink_wp_head');
    remove_action('wp_head', 'adjacent_posts_rel_link_wp_head');
    remove_action('wp_head', 'feed_links_extra' );


#### auto feed

    /*
     * auto feed
     */
    add_theme_support( 'automatic-feed-links' );


#### ページ一覧にキャプチャと抜粋表示

	<?php foreach($lastposts as $post) : setup_postdata($post); ?>
		<h2><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h2>
		<div <?php post_class( 'clearfix' ); ?>">
			<?php if ( has_post_thumbnail() ) : ?>
			<div class="post-thumbnail"><?php the_post_thumbnail(); ?></div>
			<?php endif; ?>
			<?php the_excerpt(); ?>
		</div><!-- post -->
	<?php endforeach; ?>
	<?php wp_reset_postdata(); ?>


#### 抜粋を固定ページでも利用

	/*
	 * 抜粋を固定ページでも利用
	 */
	add_post_type_support( 'page', 'excerpt' );
	
	function new_excerpt_mblength ( $length )
	{
		 return 150;
	}
	add_filter('excerpt_mblength', 'new_excerpt_mblength' );
	
	function new_excerpt_more( $more )
	{
		return ' ・・・・・ ';
	}
	add_filter( 'excerpt_more', 'new_excerpt_more' );

#### 子ページ一覧取得

	<?php
	/**
	 * スラッグからIDを取得して子ページを取得
	 */
	$page = get_post( get_the_ID() );
	$slug = $page->post_name;
	$parent = get_page_by_path( $slug );
	$parent_id = $parent->ID;
	?>
	<ul>
		<?php $lastposts = get_posts('post_type=page&posts_per_page=5&post_parent=' . $parent_id ); ?>
		<?php foreach($lastposts as $post) : setup_postdata($post); ?>
		<li><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></li>
		<?php endforeach; ?>
		<?php wp_reset_postdata(); ?>
	</ul>


#### 固定ページがサブページかどうか判定

	/**
	 * 固定ページがサブページか判定する
	 *
	 * @return 親ページのページIDまたはfalse
	 * @param なし
	 */
	function is_subpage () {
		global $post;                              // $post には現在の固定ページの情報があります
		if ( is_page() && $post->post_parent ) {   // 現在の固定ページが親ページを持つかどうかをチェックします
			$parentID = $post->post_parent;        // 親ページの ID を取得します
			return $parentID;                      // 親ページの ID を返します
		} else {                                   // 親ページを持たないので...
			return false;                          // ...false を返します
		}
	}


#### get all category by post id

	/**
	 * get all category by post id
	 *
	 * @param $post_id
	 * @return array of category id
	 */
	function getAllCatByPost( $post_id ) {
		$cat_ids = array();
		foreach( get_the_category( $post_id )  as $obj ) {
			array_push( $cat_ids, $obj->term_id );
		}
		return $cat_ids;
	}


#### get all tag by post id

	/**
	 * get all tag by post id
	 *
	 * @param $post_id
	 * @return array of tag id
	 */
	function getAllTagByPost( $post_id ) {
		$tag_ids = array();
		foreach( get_the_tags( $post_id )  as $obj ) {
			array_push( $tag_ids, $obj->term_id );
		}
		return $tag_ids;
	}

### テーマカスタマイザー

#### get page slag name that is showed to front page

	/**
	 * get page slag name that is showed to front page
	 *
	 * @param string $slug_string page slug string that is divided by comma
	 * @return array
	 */
	function getPageSlugs( $slug_string ) {
		if (isset( $slug_string ) === false || $slug_string === '' ) {
			return false;
		}
		$slugs = explode( ',', $slug_string );
		$slugs_new = array();
		foreach($slugs as $slug ) {
			array_push($slugs_new, trim( $slug ) );
		}
		return $slugs_new;
	}


#### category slag name  display to front page

	/*
	 * category slag name  display to front page
	 */
	function getCategorySlugs( $categoryStr )
	{
		if ( false === isset($categoryStr) || '' === $categoryStr ) {
			return false;
		}
		$slugs = explode( ',', $categoryStr);
		$newSlugs = array();
		foreach($slugs as $slug) {
			array_push($newSlugs, trim($slug));
		}
		return $newSlugs;
	}


#### pager

```
<div><?php posts_nav_link('&nbsp;|&nbsp;','&laquo;前のページ','次のページ&raquo;'); ?></div>
```


### CSSスニペット

#### 必須クラス

	/* -------------------------------------------------------------------
	# WordPressで必ず設定するクラス
	------------------------------------------------------------------- */
	.sticky {
	  margin: 0 0 $font-size * 2;
	  padding: 20px;
	  border: 1px solid silver;
	}
	
	.gallery-caption {
	  background-color: rgba(0, 0, 0, 0.7);
	  -webkit-box-sizing: border-box;
	  -moz-box-sizing:    border-box;
	  box-sizing:         border-box;
	  color: #fff;
	  font-size: 12px;
	  line-height: 1.5;
	  margin: 0;
	  max-height: 50%;
	  opacity: 0;
	  padding: 6px 8px;
	  position: absolute;
	  bottom: 0;
	  left: 0;
	  text-align: left;
	  width: 100%;
	}
	
	.gallery-caption:before {
	  content: "";
	  height: 100%;
	  min-height: 49px;
	  position: absolute;
	  top: 0;
	  left: 0;
	  width: 100%;
	}
	
	.bypostauthor > article .fn:before {
	  content: "\f408";
	  margin: 0 2px 0 -2px;
	  position: relative;
	  top: -1px;
	}



### 画像必須スタイル

	/* -------------------------------------------------------------------
	# WordPress 投稿画像スタイル
	------------------------------------------------------------------- */
	/*
	## 画像配置
	
	投稿エディターの画像挿入時に指定する配置スタイル。  
	marginは$font-sizeを基準に考える。
	*/
	.aligncenter {
	  margin: $font-size*1.5 auto;
	  display: block;
	}
	.alignright {
	  margin-left: $font-size*1.5;
	  margin-top: 0;
	  margin-bottom: $font-size;
	  display: block;
	  float: right;
	}
	.alignleft {
	  margin-right: $font-size*1.5;
	  margin-top: 0;
	  margin-bottom: $font-size;
	  display: block;
	  float: left;
	}
	.wp-caption-text {
	  margin: 0 !important;
	  padding: $font-size/4 !important;
	  font-size: $font-size - 2;
	  text-align: center;
	}
	
	/*
	## アイキャッチ(thumbnail)

```
<div>
   <?php if ( has_post_thumbnail() ) : ?>
      <div class="post-thumbnail"><?php the_post_thumbnail(); ?></div>
   <?php endif; ?>
   <div><?php the_content( '続きを読む &raquo;' ); ?></div>
</div>
```
*/
.attachment-post-thumbnail {
  margin: 0 $font-size*1.5 $font-size 0;
  float: left;
}


### リンク関連

	/* -------------------------------------------------------------------
	# WordPress 投稿画像スタイル
	------------------------------------------------------------------- */
	/*
	## 画像配置
	
	投稿エディターの画像挿入時に指定する配置スタイル。  
	marginは$font-sizeを基準に考える。
	*/
	.aligncenter {
	  margin: $font-size*1.5 auto;
	  display: block;
	}
	.alignright {
	  margin-left: $font-size*1.5;
	  margin-top: 0;
	  margin-bottom: $font-size;
	  display: block;
	  float: right;
	}
	.alignleft {
	  margin-right: $font-size*1.5;
	  margin-top: 0;
	  margin-bottom: $font-size;
	  display: block;
	  float: left;
	}
	.wp-caption-text {
	  margin: 0 !important;
	  padding: $font-size/4 !important;
	  font-size: $font-size - 2;
	  text-align: center;
	}
	
	/*
	## アイキャッチ(thumbnail)
	
	```
	<div>
	   <?php if ( has_post_thumbnail() ) : ?>
		  <div class="post-thumbnail"><?php the_post_thumbnail(); ?></div>
	   <?php endif; ?>
	   <div><?php the_content( '続きを読む &raquo;' ); ?></div>
	</div>
	```
	*/
	.attachment-post-thumbnail {
	  margin: 0 $font-size*1.5 $font-size 0;
	  float: left;
	}


#### ウィジェット(組み込み)

	/*
	# WordPress widget
	
	@name widget  
	@requires _variables.scss  
	@variable $widget-font-size
	
	```
	<div id="side">
	  <ul id="sidebar">
		<li class="widget widget_recent_entries" id="recent-posts-2">
		  <h2 class="widgettitle">最近の投稿</h2>
		  <ul>
			<li><a href="#">タイトル1</a></li>
			<li><a href="#">タイトル1</a></li>
		  </ul>
		</li>
	  </ul>
	</div>
	```
	*/
	.widget {
	  clear: both;
	  margin-bottom: $font-size * 3;
	  font-size: $font-size - 1;
	  h2 {
		margin-bottom: $font-size;
		background: transparent;
		font-weight: bold;
		font-size: $font-size;
	  }
	  ul {
		margin: 0;
	  }
	  li {
		clear: both;
		margin-bottom: 2px;
		list-style-type: none !important; /* not remove. if it remove, list type is circle. */
		&:last-child {
		  border-bottom: none;
		}
	  }
	  li:before {
		content: "\f105  ";
		font-family: FontAwesome;
	  }
	}
	
	/*
	## カレンダー
	
	```
	<ul id="sidebar">
	<li class="widget widget_calendar" id="calendar-2">
		<div id="calendar_wrap">
			<table id="wp-calendar">
			<caption>2013年7月</caption>
			<thead>
			<tr>
				<th title="月曜日" scope="col">月</th>
				<th title="火曜日" scope="col">火</th>
				<th title="水曜日" scope="col">水</th>
				<th title="木曜日" scope="col">木</th>
				<th title="金曜日" scope="col">金</th>
				<th title="土曜日" scope="col">土</th>
				<th title="日曜日" scope="col">日</th>
			</tr>
			</thead>
			<tfoot>
			<tr>
				<td class="pad" id="prev" colspan="3">&nbsp;</td>
				<td class="pad">&nbsp;</td>
				<td class="pad" id="next" colspan="3">&nbsp;</td>
			</tr>
			</tfoot>
			<tbody>
			<tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td>
			</tr>
			<tr>
				<td>8</td><td>9</td><td>10</td><td>11</td><td>12</td><td>13</td><td>14</td>
			</tr>
			<tr>
				<td>15</td><td>16</td><td>17</td><td>18</td><td>19</td><td>20</td><td>21</td>
			</tr>
			<tr>
				<td><a title="Hello world!" href="http://shikinohana.localhost/date/2013/07/22">22</a></td><td>23</td><td>24</td><td id="today">25</td><td>26</td><td>27</td><td>28</td>
			</tr>
			<tr>
				<td>29</td><td>30</td><td>31</td>
				<td colspan="4" class="pad">&nbsp;</td>
			</tr>
			</tbody>
			</table>
		</div>
	</li>
	</ul>
	```
	*/
	#wp-calendar {
	  width: 100%;
	  th {
	  }
	  td {
		text-align: center;
	  }
	  a {
		text-decoration: underline;
	  }
	}
	
	/*
	## widget menu
	*/
	.widget_nav_menu li {
	  clear: both;
	  float: none !important;
	  background: #FFF;
	  a {
		display: inline;
	  }
	}


#### コメントスタイル

	/* -------------------------------------------------------------------
	# WordPress コメント
	------------------------------------------------------------------- */
	.comments {
	  .comment-list {
		li {
		  list-style-type: none;
		}
		
		.comment {
		  margin-bottom: $font-size * 2.5;
		  .comment-body {
			p {
			  margin-bottom: $font-size;
			}
			.comment-author {
			  margin-bottom: $font-size;
			}
			.comment-meta {
			  margin-bottom: $font-size;
			}
			.comment-reply-link:before  {
			  content: "\f064";
			  font-family: FontAwesome;
			}
		  }
		}
	  }
	}
	.children {
	  margin-top: $font-size * 2;
	}


#### 投稿スタイル

	/* -------------------------------------------------------------------
	# WordPress post style
	
	投稿(固定ページ投稿含む)スタイル。デフォルトはBootstrapのスタイル。  
	------------------------------------------------------------------- */
	.main {
	  h1 {
		font-size: 18px;
	  }
	  h2 {
		font-size: 17px;
	  }
	  h3 {
		font-size: $font-size;
	  }
	  h4 {
		font-size: $font-size;
	  }
	  h5 {
		font-size: $font-size;
	  }
	  h6 {
		font-size: $font-size;
	  }
	  p {
		margin-bottom: $font-size*2;
		font-size: $font-size;
		line-height: $font-size*2;
	  }
	  img {
		max-width: 100%;
		height: auto;
	  }
	  blockquote {
		margin-bottom: $font-size*2;
		font-size: $font-size;
		line-height: $font-size*2;
	  }
	  table {
		margin-bottom: $font-size*2;
		border: 1px solid #DDD;
	  }
	  thead {
		/* bootstrap */
	  }
	  tfoot {
		/* bootstrap */
	  }
	  caption {
		/* bootstrap */
	  }
	  col {
		/* bootstrap */
	  }
	  colgroup {
		/* bootstrap */
	  }
	  tbody {
		/* bootstrap */
	  }
	  tr {
		/* bootstrap */
	  }
	  th {
		padding: $font-size/3 $font-size/2;
		border: 1px solid #DDD;
		font-size: $font-size;
		line-height: $font-size*2;
	  }
	  td {
		padding: $font-size/3 $font-size/2;
		border: 1px solid #DDD;
		font-size: $font-size;
		line-height: $font-size*2;
	  }
	  dl {
		margin-bottom: $font-size*2;
	  }
	  dt {
		margin-bottom: $font-size/2;
		font-size: $font-size;
		line-height: $font-size*2;
	  }
	  dd {
		margin-bottom: $font-size;
		font-size: $font-size;
		line-height: $font-size*2;
	  }
	  ul {
		margin-bottom: $font-size*2;
	  }
	  ol {
		margin-bottom: $font-size*2;
	  }
	  li {
		/* bootstrap */
		font-size: $font-size;
		line-height: $font-size*2;
	  }
	  address {
		margin-bottom: $font-size*2;
	  }
	  abbr {
		/* 略語 */
		/* bootstrap */
	  }
	  cite {
		/* bootstrap */
	  }
	  ins {
		/* bootstrap */
	  }
	  del {
		/* bootstrap */
	  }
	  code {
		/* bootstrap */
	  }
	  pre {
		margin-bottom: $font-size*2;
	  }
	  q {
		/* bootstrap */
		font-style: italic;
	  }
	  sub {
		/* bootstrap */
	  }
	  sup {
		/* bootstrap */
	  }



### メタ情報(クラス名 ユーザー定義)

	/*
	## 投稿メタ情報
	
	@name list meta
	@requires _list.scss
	@variable $font-size  
	@variable $color-secondary
	
	```
	<ul class="list-meta">
	  <li class="list-meta__item--date">投稿時間</li>
	  <li class="list-meta__item--category">カテゴリー</li>
	  <li class="list-meta__item--author">作成者</li>
	</ul>
	```
	*/
	.list-meta {
	  margin: 0 0 $font-size * 2;
	  padding: 0;
	  .list-meta__item {
		list-style-type: none;
		padding: 0 $font-size;
		
	  }
	  .list-meta__item--category {
		@extend .list-meta__item;
		&:before {
		  content: "\f07b";
		  font-family: FontAwesome;
		}
	  }
	  .list-meta__item--tag {
		@extend .list-meta__item;
		&:before {
		  content: "\f02b";
		  font-family: FontAwesome;
		}
	  }
	  .list-meta__item--date {
		@extend .list-meta__item;
		&:before {
		  content: "\f073";
		  font-family: FontAwesome;
		}
	  }
	  .list-meta__item--link {
		@extend .list-meta__item;
		&:before {
		  content: "\f0c1";
		  font-family: FontAwesome;
		}
	  }
	  .list-meta__item--comments {
		@extend .list-meta__item;
		&:before {
		  content: "\f075";
		  font-family: FontAwesome;
		}
	  }
	}

