# Modern Java Web Application Development with Spring Boot

* スライド, https://k--kato.github.io/modernJWeb
* サンプル, https://github.com/k--kato/marsweather

# 技術要素

Web Big Picture

![Web big picture](img/web_bigpicture.jpeg)

## Webアプリケーション
* アーキテクチャの理解
 * Web API
 * REST
 * HTTP
* フロントエンド
 * Chrome 拡張機能
 * Chrome 開発者ツール
 * HTML
 * Javascript
 * CSS
 * thymeleaf
 * Cookie とセッション
* バックエンド
 * Web サーバ
   * Apache httpd
   * Apache Tomcat
 * Web アプリケーション フレームワーク
   * Spring Boot (Java)
   * Doma 2 (Java)

## データベース
* DBMS
 * PostgreSQL
   * pgAdmin III
* SQL
 * Create (INSERT)
 * Read (SELECT)
 * Update
 * Delete
* リレーショナル演算
 * Join
 * Union
* トランザクション
 * Commit
 * Rollback
* テーブル以外のオブジェクトについて
 * View
 * Trigger
 * Function

## ソースコード管理
* Git
* GitHub
* Maven

## プログラミング
* Java 8
 * 平均の算出
 * 単純ソート（バブルソート）
* IntelliJ IDEA Community

## Windows/Linuxの基本操作
* コマンドラインでJavaがコンパイルできる
* コマンドラインでGitが操作できる

## ネットワーク
* 共有フォルダ
* プリンタ

## その他
* 正規表現
* エクセル数式（マクロより数式の方がよく使います）

# インストール

* Web アーキテクチャ
 * [Chrome (Version: 49.0.2623.75+)](https://www.google.com/chrome/browser/desktop/index.html)
 * [DHC REST Client (Version: 1.2.4+)](https://chrome.google.com/webstore/detail/dhc-rest-client/aejoelaoggembcahagimdiliamlcdmfm)

* データベース
 * [PostgreSQL Windows x86-32 or x86-64 (Version: 9.5.1+)](http://www.enterprisedb.com/products-services-training/pgdownload#windows)

* ソースコード管理
 * [Git for Windows 32bit or 64bit (Version: 2.7.2+)](https://git-for-windows.github.io/)

* プログラミング
 * [Java SE Development Kit Windows x86 or x64 (Version: 8u73+)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
 * [IntelliJ IDEA Community Edition Windows (Version: 15.0.4+)](https://www.jetbrains.com/idea/download/#section=windows)

# 環境構築

1. GitHub アカウントを作成 https://github.com/join
1. GitHub のソースコード marsweather を `Folk` (Folk ボタンを押下) する https://github.com/k--kato/marsweather
1. IntelliJ IDEA Community Edition を開く
 1. `Check out from Version Control` から `GitHub` を選択
    1. GitHub のアカウント情報を入力
    1. Git Repository URL : `https://github.com/作成したGitHubのユーザ名/marsweather.git` を入力
    1. `Clone` ボタンを押下
    1. Checkout From Version Control > `Yes` を選択 
 1. File > Project Structure > Project を選択
    1. Project SDK > `New` ボタンを押下 > `JDK` を選択 > `C:\Program Files\Java\jdk1.8.0_73` を選択 > `1.8` を選択
    1. Project language level > `8 Lambdas, type annotations etc.` を選択
 1. Run > Edit Configurations
    1. `+` を選択 > `Application` を選択 > Main Class: `jp.co.cam.net.Application` を入力
 1. Run > Debug を選択
 1. Chrome で http://localhost:8080/mars?date=2016/03/01 を確認する 

# 参考ノート

1. BLOG.IK.AM, 「Spring Boot + Doma2を使おう」, https://blog.ik.am/entries/371
1. Qiita, 「Spring Boot + Doma2で2WaySQLを使うまで」, http://qiita.com/ARS_2000/items/4c680ec9b31725687e04
1. Qiita, 「Spring Boot 使い方メモ」, http://qiita.com/opengl-8080/items/05d9490d6f0544e2351a
