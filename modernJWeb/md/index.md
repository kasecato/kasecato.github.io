## Modern Java Web Application Development with Spring Boot
March 7 2016 @kasecato



### Web アプリケーションとは？

![http](img/HTTP_Steps.png)



### Web アプリケーション史
<a href="img/Web_development_timeline.png" target="_blank">
![history of Web Apps](img/Web_development_timeline.png)
</a>



### Web アプリケーション構成例
![mvc](img/mvc_archi_logo.png)



### Web 開発者に知ってほしいこと




### Web アプリケーション
* アーキテクチャの理解
 * Web API
 * REST
 * HTTP



### Web アプリケーション
* フロントエンド
 * Chrome 拡張機能
 * Chrome 開発者ツール
 * HTML
 * Javascript
 * CSS
 * Thymeleaf
 * Cookie とセッション



### Web アプリケーション
* バックエンド
 * Web サーバ
    * Apache httpd
    * Apache Tomcat
 * Web アプリケーション フレームワーク
    * Spring Boot
    * Doma 2



### データベース
* DBMS
 * PostgreSQL
 * pgAdmin III
* SQL
 * Create (Insert)
 * Read (Select)
 * Update
 * Delete



### データベース
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



### ソースコード管理
* Git
* GitHub
* Maven




### プログラミング
* Java 8
 * 平均の算出
 * 単純ソート（バブルソート）
* IDE
 * IntelliJ IDEA Community Edition




### Windows / Linux の基本操作
* コマンドラインで Java がコンパイルできる
* コマンドラインで Git が操作できる




### ネットワーク
* 共有フォルダ
* プリンタ




### その他
* 正規表現
* エクセル数式（マクロより数式）



## 作ってみよう



### 問題
火星の天気を教えてください




### とりあえず検索してみる




![Google](img/googlelogo.png)

<a href="https://www.google.com/?q=mars+weather+api" target="_blank">https://www.google.com/?q=mars+weather+api</a>




![Google mars weather api](img/google_marsweatherapi.png)




### 火星の気象レポート API を発見
![maas api top](img/maas_api_top.png)




### 使い方は？




![maas api rest](img/maas_api_rest.png)




### Web ブラウザで開いてみる
<a href="http://marsweather.ingenology.com/v1/latest/?format=json" target="_blank">http://marsweather.ingenology.com/v1/latest/?format=json</a>



### 気象データが取得できた
![maas api latest json](img/maas_api_latest_json.png)




### どうやって取得できたのか？



### Web ブラウザで `F12` キーを押す
開発者ツールが起動する



### 開発者ツールでネットワークを観測

<a href="img/maas_api_http.png" target="_blank">
![maas api latest json http](img/maas_api_http.png)
</a>



### HTTP
* Web ブラウザは，入力された URL から HTTP Request を組み立て，送信している
* 送信先では HTTP Request を受信し，HTTP Response に気象データを含めて返信している

![http](img/HTTP_Steps.png)




### 開発環境を構築しよう

https://github.com/kasecato/marsweather



### 以上です
