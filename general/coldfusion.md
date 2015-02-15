# 変数スコープ

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