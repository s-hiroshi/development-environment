# SSH(Secure Shell)

## SSHの認証

1. パスワード認証  
  ユーザー名とパスワードで認証する。
2. 公開鍵認証
  公開鍵/秘密鍵のペアーで認証する。


## 公開鍵認証とは

公開鍵と秘密鍵のペアで認証する。公開鍵はサーバに配置し秘密鍵はクライアンが持つ。  
サーバの公開鍵とペアの秘密鍵を持つクライアントからの接続のみを許可する。

秘密鍵へパスフレーズを設定できる。


## 公開鍵、秘密鍵の作成場所

ssh-keygenコマンドで鍵を作成する。
一般に作成した鍵は/Users/ユーザー名/.sshディレクトリにid_rsa(秘密鍵)とid_rsa.pub(公開鍵)として配置する。

__Macでは秘密鍵が必要なソフトはデフォルトで.ssh/id_rsaを参照する場合ある。__


## 公開鍵、秘密鍵の作成方法

    $ ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (/Users/xxx/.ssh/id_rsa): /Users/xxx/.ssh/id_rsa
    Enter passphrase (empty for no passphrase):    パスフレースを入力
    Enter same passphrase again:  再度パスフレーズを入力

.sshディレクトリにid_rsa(秘密鍵)、id_rsa.pub(公開鍵)が作成される。


## sshコマンド

    $ ssh [オプション] ホスト名 [コマンド]
    $ ssh example.com
    
秘密鍵を指定して接続

    $ ssh -i 秘密鍵のパス example.com
  
