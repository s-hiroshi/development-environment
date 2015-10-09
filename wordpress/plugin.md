## プラグイン開発


# プラグイン作成

## セキュリティー

nonce(number used once)を使いセキュリティーを確保します。

### 送信元

フォーム

	<form>
	<?php echo wp_nonce_field( 'example_action', '_wpnonce' ); ?>
	....

リンク

	wp_nonce_url(
		add_query_arg( array( 'action' => 'example' ), $url ),
		'example_action'
	);

送信先プログラム

	// 送信元が管理画面の場合
	if ( isset( $_POST['_wpnonce'] ) && $_POST['_wpnonce'] ) {
		if ( check_admin_referer( 'example_action', '_wpnonce' ) {
			// 処理
		}
	}

	// 送信元が管理画面では無い場合
	$nonce = isset( $_POST['_wpnonce'] ) ? $_POST['_wpnonce'] : null;
	if ( wp_verify_nonce( $nonce, 'example_action' ) )  {
		// 処理
	}


## 構成

* オプション Setting API  
	update_option/get_option/delete_option
* カスタム投稿タイプ  
  register_post_type
	+ カスタムフィールド  
	  add_meta_box WordPressはカスタムフィールドをwp_postmetaへ保存するのでadd_meta_boxを使います。


### Subversion

[https://wordpress.org/plugins/about/svn/](https://wordpress.org/plugins/about/svn/)

#### 最初のコミット

	$ cd wp-content/plugins
	$ svn co http://plugins.svn.wordpress.org/category-monthly-archives/ my-plugin-dir
	
	$ cd my-plugin-dir
	$ svn add trunk/*
	$ svn ci -m 'Adding first version of my plugin'

#### バージョンアップ(タグ付け)

WordPressはタグを付けるとバージョンアップになります。下記例はバージョン0.0.2を公開する例です。

	$ cd my-plugin-dir
	$ svn cp trunk tags/0.0.2
	$ svn ci -m "tagging version 0.0.2"