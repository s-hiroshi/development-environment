# gitのdiffツール

* Araxis Merge for mac

## Araxis Merge for macをgitのdiff/mergeツールとして登録

### git設定ファイル(.gitconfig)

~/.gitconfig

### .gitconfigの内容

	[diff]
		tool = araxis
	[merge]
		tool = araxis

## diffコマンド(difftool)

	$ difftool HEAD^..HEAD
	$ difftool HEAD^..HEAD -- readme.txt # ファイルパスを指定

## diffのまとめ

	$ git difftool              // ワーキングツリーとインデックスの比較
	$ git difftool --cached     // インデックスとHEADの比較
	$ git difftool HEAD         // ワーキングツリーとHEADの比較
	$ git difftool HEAD^..HEAD  // HEAD^とHEADの比較

[git diff の使い方がほんの少し理解できた - murankの日記](http://d.hatena.ne.jp/murank/20110320/1300619118)

# git reset/checkout

gitの取り消し(やり直し)のまとめ

## reset コミットに対して行う処理

	$ git reset --soft HEAD^    // HEADをHEAD^へ変更 インデックス、ワーキングツリーは変更しない
	$ git reset HEAD^           // HEADとインデックスをHEAD^へ変更、ワーキングツリーは変更しない
	$ git reset --hard HEAD^    // HEAD、インデックス、ワーキングツリーをすべてHEAD^へ変更

resetはファイル単位の変更には対応していない。

## checkout

	$ git checkout file

fileをHEADからの変更を取り消す。