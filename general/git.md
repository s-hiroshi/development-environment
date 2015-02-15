## Git ホストOS Mac

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

HEADから行ったfileへの変更を取り消す。