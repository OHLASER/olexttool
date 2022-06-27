# HRUKA 外部操作ツールセット
このリポジトリには、HARUKAに対して加工ファイルの送信や処理中の加工ファイル情報の読込などを外部から行うためのライブラリ、またそのライブラリを利用したCLIツールおよびWebアプリケーションのプロジェクトが含まれています。
ライブラリを使用することでユーザー独自のプログラムからHARUKAの操作を行えます。またWebアプリに応用して加工機接続中のPC以外からHARUKAを遠隔操作するといったことも可能になります。その実装の一例がolhrk_webです。
olhrl_webのビルド済バイナリも配布しています。[リリースページ](http://example.com)からご参照ください。

対象OS:Windows 10
開発環境:Visual Studio 2017

## 導入
このgitリポジトリをクローンした後、サブモジュールの取得が必要です。
具体的には以下のような手順でコマンドを実行してください。
```sh
git clone https://github.com/OHLASER/olexttool
cd olexttool
git submodule update --init
```

## ファイル/ディレクトリ構成
導入の作業後、以下のようなファイル構成となります。
|  ディレクトリ  |  説明  |
| ---- | ---- |
|  msvs  |  外部操作に関する各種プロジェクトを含むソリューションファイル(olhrk_client_0.sln)が含まれています。  |
|  oldlclr  |  CLIツール/Webアプリ共通で使用されるHARUKA操作用ライブラリの.NETプロジェクトが含まれます。olhrk_client_0.slnに参照されています。  |
|  olhrk_client  |  CLIツールの.NETプロジェクトが含まれます。olhrk_client_0.slnに参照されています。  |
|  olhrk_web  |  WebアプリのASP.NETプロジェクトが含まれます。olhrk_client_0.slnに参照されています。  |
|  README.md  |  本ドキュメントです。  |
|  moduleSpec.md  |  oldlclrの詳細な仕様を記載しています。  |

## 各プロジェクトの概要
### oldlclr - HARUKA通信用モジュール
oldlclrは、実行中のHARUKAを制御するC#ライブラリです。以下の公開メソッドを備えています。
下表のクラスが登録されています。一部クラスとメソッドはHARUKA側の利用で、クライアント側では使用を想定していません。
仕様の詳細は[このページ](http://example.com)を参照ください。

### olhrk_web
olhrk_webはC#で記述された ASP.NET Webアプリケーションです。HARUKAが動作しているWindowsでIISを起動して使用します。httpプロトコルを通じて、HARUKAの状態取得、データ転送を行うことができます。
HARUKAとの通信は以下のようにREST APIを通して行われます。
* HTTP[GET] Home/Staus 
 json形式のHARUKAの状態を取得することができます。書式については後述します。
* HTTP[POST] Home/Load 
 加工データを本文に追加することで、HARUKAにデータ転送を送ることが出来ます。書式については後述します。

### olhrk_client
HARUKAが動作しているWindowsで実行できるC#コンソールプログラムです。コマンド引数を利用して、HARUKAの状態取得、加工データの転送などが行えます。当該プログラムの利用方法は、IISを利用しないWebサーバを構築する場合に使用します。

## IISの設定
olhrk_webを使用する際、IISの設定についての注意点を記載します。
### IISマネージャ
olhrk_webを使用するサイトに紐づくアプリケーションプールの詳細設定画面上で、以下のように項目設定が行われている必要があります。
|  項目  |  説明  |
| ---- | ---- |
|  32ビットアプリケーションの有効化  | True  |
|  ID  | LocalSystem  |
<画像>
### Windowsの機能
以下のようにwindowsの機能を有効化する必要があります。
<画像>

### その他
IPアドレス、ポートの設定は任意の値で問題ありません。