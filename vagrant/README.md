# Vagrant

## ヘルプ

    $ vagrant --help

## Vagrant自体のUpgrade

Vagrant公式サイトからパッケージをダウンロードしインストールしてください。

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

    // box削除
    $ vagrant box remove <box名>
    // バージョンを指定して削除
    $ vagrant box remove <box名> --box-version 1.x.x

## VirtualBoxの状態

vagrant up/halt/destroyに対するVirtualBoxの状態です。

* vagrant up 実行中
* vagrant halt 電源オフ
* vagrant destroy 中断

## トラブルシューティング

### 事例1

> A VirtualBox machine with the name 'vccw.dev' already exists.
> Please use another name or delete the machine with the existing
> name, and try again.

発生原因と解決策を調べてください。  
競合しているboxをdestroyすると改善することがあります。

    // vagrant global-status
    $ vagrant destroy <box名>

### 事例2

> An error occurred in the underlying SSH library that Vagrant uses.
> The error message is shown below. In many cases, errors from this
> library are caused by ssh-agent issues. Try disabling your SSH
> agent or removing some keys and try again.
>
> If the problem persists, please report a bug to the net-ssh project.
>
> timeout during server version negotiating

上記エラーが発生したときSource TreeやPhpStormを終了したら改善しました。
