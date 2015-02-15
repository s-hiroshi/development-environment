# コーディング規約

* [ローマ字表示](#roma)
* [長音記号](#chouon)
* [文字コード](#character)
* [ファイル名](#filename)
* [ディレクトリ名](#directory)
* [URL](#url)
* [HTML](#html)
* [CSS](#css)
* [JavaScript](#javascript)
* [PHP](#php)
* [WordPress](#wordpress)
* [プログラム全般ガイド](#general)
* [プログラム参考ガイドライン](#reference)
* [制作フロー](workflow_guide.md)
* [開発環境整備](development_environment_guide.md)
* [参照リンク](#mylink)



## <a name="roma">ローマ字

ヘボン式

ok|no
---|---
shi|si
tsu|tu



## <a name="chouon">長音記号

     サーバー       ok
     サーバ         no
     パラメーター    ok
     パラメータ      no


## <a name="character">文字コード

特別な理由がないときはすべて(HTML, CSS, JavaScript, PHP)の文字コードはBOMなしUTF-8



## <a name="filename">ファイル名 汎用

### 使用可能文字

  * 数字
  * 英小文字
  * アンダースコア
  * ハイフン
  * . (ピリオド)


### 命名規則

単語の区切りはアンダースコア

    file_name.txt

バージョンや日付など数値との区切りはハイフン

    file_name-1.txt

数値の区切りはドット

    fine_name-1.0.0.txt

### 画像命名規則

#### 省略

原則省略名は使わない。
ただし下記のように業界で定着してい単語は使って良い(統一して使う)。

Web Designing vol141 p061をから引用

省略|省略無
---|-----
bg|background
btn|button
ico|icon
bnr|banner
h1,...,h6|heading1,...,heading6
thumb|thumbnail
img | image


#### 順序

ページ\_構造\_機能.拡張子

workページのheader部分のcloseボタンの例。

     home
       | --- work
       |       |- word_header_close.png
       |
       | --- about
               |- about_header_close.png



## <a name="directory">ディレクトリ名

### 使用可能文字

  * 数字
  * 英小文字
  * アンダースコア
  * ハイフン

### 命名規則

区切り文字はアンダースコアまたはハイフンを使う。


## <a name="url">URL

### 静的サイト

静的サイトは上記ファイル名、ディレクトリ名の規則に従う。

### 動的サイト

区切り文字はハイフンを利用する

&raquo;[Google と相性の良い URL の作成 - ウェブマスター ツール ヘルプ](http://support.google.com/webmasters/bin/answer.py?hl=ja&answer=76329&topic=2370420&ctx=topic)



## <a name="html">HTML

[hail2u/html-best-practices](https://github.com/hail2u/html-best-practices)



## <a name="css">CSS

### 言語仕様

[Cascading Style Sheets Level 2 Revision 1 (CSS&nbsp;2.1) Specification](http://dev.w3.org/csswg/css2/)
[CSS Working Group Editor Drafts](http://dev.w3.org/csswg/)


### インデント

  スペース2つ

    #example {
      width: 80px;
    }


### 命名規則

[Methodology / BEM. Block, Element, Modifier / BEM](https://bem.info/method/definitions/)  
[bem-methodology-ja/definitions.md at master · juno/bem-methodology-ja](https://github.com/juno/bem-methodology-ja/blob/master/definitions.md)  
[MindBEMding – getting your head ’round BEM syntax &ndash; CSS Wizardry &ndash; CSS, OOCSS, front-end architecture, performance and more, by Harry Roberts](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)


### セレクター

__スタイルの指定はできるだけclassを使いidはJavaScriptで要素を特定する必要がある場合に使う。__

    .example {
      statements
    }

特別な理由がないときは次のような書き方は避ける。

    div.example {
      statements
    }

CSSの重み(詳細度)にできるだけ依存しないように書く。  

[http://www.w3.org/TR/CSS21/cascade.html#specificity](http://www.w3.org/TR/CSS21/cascade.html#specificity)


### comment

コメントの英字は小文字。

    linebreak
    linebreak
    /* -------------------------------------------------------------------
    level 1 comment
    ------------------------------------------------------------------- */
    linebreak
    /* level 2 comment
    --------------------------------- */
    linebreak
    /* level 3 comment */


### プロパティ記述順序

CSS2 Specificationで出てくる順序。  
[http://hail2u.net/blog/webdesign/css2-property-order.html](http://hail2u.net/blog/webdesign/css2-property-order.html)

プロパティをオンラインでソートするサービス   
[http://csscomb.com/online/](http://csscomb.com/online/)


### color

use html hexadecimal color not color name

    ok
    #example {
        color: #fff;
    }

    no
    #example {
        color: #ffffff;
    }

    no
    #example {
        color: white;
    }


### charset

```css
@charset "utf-8";
```


### 事前確認

####  確認ブラウザ

* IE is IE8 and more
* Firefox is latest version
* Chrome is latest version
* Opera is latest version

#### その他

* 12px未満のフォント使わない。IE8で文字が揺れる不具合ある。
* IEのウェブフォントはEOTファイルを利用する。
* 基本スタイルを確定する。
  + フォント
  + カラー
  + リンクカラー
  + 基準余白
* 利用するHTML要素を確定する。


### <a name="sass">Sass

#### 参考ガイド

[Sass Guidelines](http://sass-guidelin.es/)

#### 変数

##### セパレーター

ハイフン(hyphen)で区切る。

ok

     $font-size

no

     $font_size

##### primary, secondary, tertiary, quaternary, quinary

カラーなどはprimary, secondaryをつかい区別する。

    $color-primary
    $color-secondary
    $color-tertiary
    $color-quaternary
    $color-quinary

##### 基本サイズの変数名

サイズの変数名はプロパティ名と同じにする。

    $width
    $height
    $line-height
    $font-size


bodyに文字サイズ$font-sizeを指定する。


    body {
      font-ize: $font-size;
    }

#### 引数

前後にスペースを入れない。

    $hoge($arg1, $arg2)



#### mixin

```css
@mixin base-space {
  margin: 0 0 20px;
  padding: 10px;
}
```

```css
p {
    @include base-space;
}
```

#### extend

```css
.btn {
  width: 120px;
  margin: 0 8px;
  border-radius: 5px;
  line-height: 2;
  text-align: center;
}
```

```css
.btn-primary {
  @extend .btn;
  background: #5588cc;
}
```


#### モジュール + スキン

Sassはextendを使いモジュール + スキンを実現する。  
モジュールの基本スタイルを.btnクラスとして作成する。基本スタイルに背景#5588ccのスキンをあてたクラス(.btn-primary)をextendを使い作成する。



## <a name="javascript">JavaScript

### 言語仕様

[Standard ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)


### 参考ガイド

下記ガイドを参考にする。

* [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [JavaScript Style Guide | Contribute to jQuery](http://contribute.jquery.org/style-guide/js/?rdfrom=http%3A%2F%2Fdocs.jquery.com%2Fmw%2Findex.php%3Ftitle%3DJQuery_Core_Style_Guidelines%26redirect%3Dno)


### 記述位置

スクリプトは特別の理由が無いときは外部ファイルに記述する。  
インラインで記述する必要があるときは下記のようにする。

```javascript
<script>
    function() {
        statements
    }
</script>
```

no

```javascript
<script>
function() {
}
</script>
```

### class name

UpperCamelCase

```javascript
function ClassName() {
    statements
};
```

### function/method name

lowerCamelCase

    function methodName() {
        statements
    };

### variable name

lower underscore separated

    var some_name;

### static variable/constant

Capital underscore separated

    SOME_STATIC

### comment

#### documentation comment

    /**
     *
     */

#### readability comment

    linebreak
    linebreak
    /*
     * block level
     */
    linebreak
    /* oneline level */


__ 圧縮することを考え//記法のコメントは使わない。__


### others

##### function

    function() {
        statement
    }

functionと()の間はスペース無し。()と{}の間は半角スペースを１ついれる。

##### 3項演算子

     ( condition ) ? true : false;

?, :の前後にスペースを一ついれる。

##### 即時実行 immediate invokation

```javascript
(function() {
    statements
}())
```

```javascript
var some = (fucntion() {
    return function() {
        statements
    };
}());
```

即時実行は常に括弧()で囲む。
行頭の即時実行は実行関数を括弧()で囲まないと構文エラーになる。
行頭以外でも常に囲むようにする。

### if, for

```javascript
if ( condition ) {
    statement
}

for ( var i = 0; i < length; i++ ) {
    statement
}
```

### switch

```javascript
switch ( expression ) {
case expression:
    statements
    break;
default:
    statements
}
```

### try catch 例外処理

catchの後にスペースを１つ入れる。

```javascript
try {
    statements
} catch ( e ) {
    statements
} finally {
    statements
}
```

引数名はeで統一する。

### 関数呼び出しと引数1

```javascript
some();
some( arg );
some( 'string', object );
some( [ 'arg1', 'arg2' ] );
some( i );  // ok
some(i);    // no
some(1);    // ok
some( 1 );  // no
```

### 関数呼び出しと引数2

&raquo; [Arrays and Function Calls](http://contribute.jquery.org/style-guide/js/#arrays-and-function-calls)

> Exceptions:
> // Function with a callback, object, or array as the sole argument:
> // No space on either side of the argument
> foo({
>     a: "alpha",
>     b: "beta"
> });
> // Function with a callback, object, or array as the first argument:
> // No space before the first argument
> foo(function() {
>     // Do stuff
> }, options );
> // Function with a callback, object, or array as the last argument:
> // No space after after the last argument
> foo( data, function() {
>     // Do stuff
> });

### イベントハンドラに指定するコールバック関数の引数

```JavaScript
$( 'selector' ).click(function( evt ) {
    statements
});
```

イベントハンドラのコールバック関数は引数名をevtで統一する。
eやeventを使わない。

### 変数定義

&raquo; [Arrays and Function Calls](http://contribute.jquery.org/style-guide/js/#arrays-and-function-calls)

> // Bad
> var foo = true;
> var bar = false;
> var a;
> var b;
> var c;
> // Good
> var a, b, c,
> foo = true,
> bar = false;

### 英語

* 子ども(孫を含まない)
  children
* 子孫(孫を含む)
  descendant


<a name="php">PHP
-----

### ベースとする規約

* [PHP-FIG — PHP Framework Interop Group](http://www.php-fig.org)
* [WordPress Coding Standards &laquo; WordPress Codex](http://codex.wordpress.org/WordPress_Coding_Standards)
* [Zend Framework](http://framework.zend.com/manual/1.12/ja/coding-standard.coding-style.html)

### <?php ?>

ok

```php
<?php
some code
?>
```

no
```php
<?php
    some code
?>
```

### 引数、添字

```php
example( $i ); // ok

example($i);   // no
```

```php
example( $arg1, $arg2 );
```

```php
$arr[ 'key' ];
```

```php
example( $arr[ 'key' ] );
```

*ただし引数、添字が数字一文字のときは前後に空白を含まない。*

```php
ok
example(1);
$arr[1];

no
$example( 1 );
$arr[ 1 ];

### 論理演算子

```php
if ( $first === true
    || $second === false
    || $third === true
) {
  // to do
}
```

```PHP
<?php if ( $first === true
    || $second === false
    || $third === true
) : ?>
```

論理演算子の前で改行し位置を合わせる。
&raquo; [Zend Framework](http://framework.zend.com/manual/1.12/ja/coding-standard.coding-style.html)

```php
if ( ! $boolean ) {
    statements
}
```
否定演算子!の前後にスペースを入れる。

### for loop

```php
for ( $i = 0; $i < $count; $i++ ) {
    statements
}
```

### 3項演算子

```php
$res = ( $some === true ) ? true : false;
```

?, :の前後にスペースを一ついれる。

### 文字列連結

連結演算子は行頭に配置する。
行頭の連結演算子の前にスペース4つ。後ろに1つ。

```php
$string = 'Lorem ipsum dolor sit amet,'
    . 'consectetur adipiscing elit.'
    . 'Phasellus fringilla sem eu dolor';
```

```php
echo 'Lorem ipsum dolor sit amet,'
    . 'consectetur adipiscing elit.'
    . 'Phasellus fringilla sem eu dolor';
```

&raquo; [Zend Framework](http://framework.zend.com/manual/1.12/ja/coding-standard.coding-style.html)

### キャスト

```php
$num = (int) $str
```

### クロージャー

```php
function example( $x )
{
    $callback = function( $parameter ) use( $x )
    {
        statements
    };
}

### HTMLの出力

```php
<div>
<?php if ( $some === true ) : ?>
    <p>some text</p>
<?php endif; ?>
</div>
```

リストの場合。

```php
<ul>
<?php if ( $some === true ) : ?>
    <li>some text</li>
<?php endif; ?>
</ul>
```

### comment

#### documentation comment

    /**
     *
     */

#### readability comment

    linebreak
    linebreak
    /*
     * large level1
     */
    linebreak
    /* large level2 */
    // base



## <a name="wordpress">WordPress

[coding_guide_wordpress.md](coding_guide_wordpress.md)



## <a name="general">プログラム全般

### インデント

半角スペース2つ。


### 1行の最大文字列

80文字


### クラスのメンバの記述順序

1. プライベート変数
2. パブリック変数
3. コンストラクタ
4. プライベートメソッド
5. パブリックメソッド


### ゲッター/セッター

アルファベット順にゲッター、セッターの順に記載する。

## <a name="reference">プログラム参考ガイド

### HTML/CSS

* [Google HTML/CSS Style Guide](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml)
* [CSS Coding Standards | Core Contributor Handbook](http://make.wordpress.org/core/handbook/coding-standards/css/)


### JavaScript

* [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [JQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
* [Code Conventions for the JavaScript Programming Language](http://javascript.crockford.com/code.html)  
* [JavaScript style guide – MDC](https://developer.mozilla.org/ja/docs/JavaScript_style_guide)


### PHP

* [PHP-FIG — PHP Framework Interop Group](http://www.php-fig.org)
* [WordPress Coding Standards &laquo; WordPress Codex](http://codex.wordpress.org/WordPress_Coding_Standards)
* [Coding Style - Zend Framework Coding Standard for PHP - Zend Framework](http://framework.zend.com/manual/1.12/en/coding-standard.coding-style.html)



## 制作フロー

[制作フロー](workflow_guide.md)



## 開発環境

[開発環境整備](development_environment_guide.md)



## Author

* Hiroshi Sawai
* [Info Town](http://www.info-town.jp)
* info@info-town.jp

