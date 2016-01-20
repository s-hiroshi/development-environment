# Vagrant

## ヘルプ

    $ vagrant --help

## Upgrade

Vagrant公式サイトからパッケージをダウンロードしインストールしてください。

## トラブルシューティング

> A VirtualBox machine with the name 'vccw.dev' already exists.
> Please use another name or delete the machine with the existing
> name, and try again.

発生原因と解決策を調べてください。  
競合しているboxをdestroyすると改善することがあります。

    // vagrant global-status
    $ vagrant destroy <box名>

## ローカル全体のVagrant環境確認

    // ローカルにインストールされているVagrant環境表示
    $ vagrant global-status
    // キャッシュをクリアして表示
    $ vagrant global-status --prune

## boxのリスト

    $ vagrant box list

## boxの更新情報

    // Vagrantfile配置フォルダで下記コマンド実行します。
    $ vagrant box outdated

## boxのアップデート

    // Vagrantfile配置フォルダで下記コマンド実行します。
    $ vagrant box update

## boxの削除

    $ vagrant destroy <box名>