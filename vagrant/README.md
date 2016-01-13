# Vagrant

# ヘルプ

    $ vagrant --help

# ローカル全体のVagrant環境確認

    // ローカルにインストールされているVagrant環境表示
    $ vagrant global-status
    // キャッシュをクリアして表示
    $ vagrant global-status --prune

# boxのリスト

    $ vagrant box list

# boxのアップデート

Vagrantfile配置フォルダで下記コマンド実行します。

    $ vagrant box update

# boxの削除

    $ vagrant destroy <box名>