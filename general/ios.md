iOS シミュレータでiPhoneサイトを確認する
=========

iOS シミュレーターとSafariの開発でチェックするときのメリッット・デメリット

iOS
==========

* Xcode
* iOS Simulator


Xcode Version 4.6
---------

&raquo; [デベロッパツールの概要 - Apple Developer](https://developer.apple.com/jp/technologies/tools/)

    /Applications/Xcode.app

iOS シミュレータ
----------

### iOS シミュレータインストール

Xcodeを起動して下記からシミュレータをダウンロード・インストールする。複数のバージョンを選べる。iOS 6 Simulatorを選択した。2013.2.25

    Xcode.app -> preference -> Download

*iOSシミュレータはXcodeをインストールすると自動的にインストールされるわけではない。*


### iOS シミュレータ起動

##### Xcodeから起動

    Xcode -> Open Developer Tool -> iOS Simulator

##### 2 iOS シミュレータを単体で起動

    /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/Applications


Safari 開発
----------

Safari -> preference -> 詳細 -> 開発をメニューに追加

開発 -> ユーザーエージェント -> iphone

safariの最小幅が380にしかならない。


Script Browser Plus(iPhoneアプリ)
----------

ソースコードを確認。