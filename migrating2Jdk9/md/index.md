## The Hitchhiker's Migration Guide to JDK 9
### 新機能の影で消えた機能

<a href="https://twitter.com/k_kato" target="_blank"><img style="border-radius: 50% !important;" src="img/twitter.jpg" width="200px" alt="Keisuke KATO"></a>

#### [Java関連もくもく会＠福岡 2017/05](https://javatoolstudy-fukuoka.connpass.com/event/57731/)

2017/5/26 <a href="https://twitter.com/k_kato" target="_blank">@k_kato</a>



## About me
```js
const addTimeline2017 = (profile) => {
    profile.timeline.push({
        year: 2017,
        lang: "Java 8",
        company: "Nulab",
        team: "Nulab Apps"
    })
}
```



## Ad1 => [サルでもわかるScala#52](https://connpass.com/event/58020/)

<a href="https://connpass.com/event/58020/" target="_blank"><img src="img/fpinscala.jpg" width="200px" /></a>

* #52 だけど...
    * 前回から FP in Scala をもう一度最初から読んでいる
    * Scala が初めてでも OK
    * 最終的にはモナドがわかる?
    * 前半読書で後半 Scala で好きなモノを作る



## Ad2 => [Scala福岡2017](https://scala.connpass.com/event/57941/)

<a href="https://scala.connpass.com/event/57941/" target="_blank"><img src="img/scalafukuoka2017.png" /></a>




## JDK 9 マイグレーション ガイド

* 今日話さないこと
    * JDK 9 の新機能
* 今日話すこと<!-- .element: class="fragment" data-fragment-index="1" -->
    * JDK 9 の**新消機能**<!-- .element: class="fragment" data-fragment-index="1" -->

> 地球を破壊することはアルファ・ケンタウリにある出張所に地球年で50年前から公示されていた - 『[銀河ヒッチハイク・ガイド](https://ja.wikipedia.org/wiki/%E9%8A%80%E6%B2%B3%E3%83%92%E3%83%83%E3%83%81%E3%83%8F%E3%82%A4%E3%82%AF%E3%83%BB%E3%82%AC%E3%82%A4%E3%83%89)』<!-- .element: class="fragment" data-fragment-index="10" -->

<!-- .element: class="fragment" data-fragment-index="10" -->



## [Java Platform, Standard Edition Oracle JDK 9 Migration Guide](https://docs.oracle.com/javase/9/migrate/toc.htm)

#### Release 9
#### Pre-General Availability: 2017-05-24



## マイグレーションの準備
* \> **JDK 9 EA を手に入れよ**
* 再コンパイル前に実行せよ
* 第三者のライブラリを更新せよ
* アプリをコンパイルせよ
* jdeps を実行せよ



### JDK 9 Early Access Build を手に入れよ
* [Docker](https://docs.docker.com/samples/openjdk/)
    ```bash
    docker pull openjdk:9
    ```
    ```bash
    docker run -it --rm -v "$PWD":/mnt -w /mnt openjdk:9 bash
    javac Main.java
    java Main
    jshell
    ```
* Homebrew (macOS)
    ```bash
    brew install Caskroom/versions/java9-beta
    ```
* インストーラ
    * [JDK 9 Early-Access Builds - jdk-9+181](http://jdk.java.net/9/)



## マイグレーションの準備
* ✅ JDK 9 EA を手に入れよ
* \> **再コンパイル前に実行せよ**
* 第三者のライブラリを更新せよ
* アプリをコンパイルせよ
* jdeps を実行せよ




### 再コンパイル前に実行せよ
* 実行できた!
    * 「標準の Java SE API だけのアプリはそのまま実行できるよ」<!-- .element: class="fragment" data-fragment-index="1" -->
        * ☕ これは一流の Java ジョークだと思った<!-- .element: class="fragment" data-fragment-index="2" -->
    * 日付と通貨のフォーマットが異なる可能性に注意 (JEP 252)<!-- .element: class="fragment" data-fragment-index="3" -->
* 実行できなかった!
    * VM のフラグを確認<!-- .element: class="fragment" data-fragment-index="10" -->
        * JDK 8 で廃止され，JDK 9 で削除された GC フラグがある (JEP 214)<!-- .element: class="fragment" data-fragment-index="11" -->
        * ☕ 要は事前に警告したぜ，と<!-- .element: class="fragment" data-fragment-index="12" -->



### JDK 9 に移行したら日付と通貨のフォーマットが変わった
* `CLDR`: Common Locale Data Repository が有効化
    * [JEP 252: Use CLDR Locale Data by Default](http://openjdk.java.net/jeps/252)
    * Unicode Consortium が作っている (1999)
    * 国際化の事実上の標準
* JDK/JRE 8 より同梱されており，**JDK 9 でデフォルト有効化**
```bash
java.locale.providers=CLDR,COMPAT,SPI
```
* JDK 8 互換に設定する方 (`CLDR` を削除する)
```bash
java -Djava.locale.providers=COMPAT,SPI
```



### JDK 9 に移行したら VM が起動しない
* GC フラグが JDK 9 で削除された
    * [JEP 214: Remove GC Combinations Deprecated in JDK 8](http://openjdk.java.net/jeps/214)
```bash
DefNew + CMS       : -XX:-UseParNewGC -XX:+UseConcMarkSweepGC
ParNew + SerialOld : -XX:+UseParNewGC
ParNew + iCMS      : -Xincgc
ParNew + iCMS      : -XX:+CMSIncrementalMode -XX:+UseConcMarkSweepGC
DefNew + iCMS      : -XX:+CMSIncrementalMode -XX:+UseConcMarkSweepGC -XX:-UseParNewGC
CMS foreground     : -XX:+UseCMSCompactAtFullCollection
CMS foreground     : -XX:+CMSFullGCsBeforeCompaction
CMS foreground     : -XX:+UseCMSCollectionPassing
```

> If they are moving from JDK 8 to JDK 9 then they will already have seen warning messages and thus **should not be surprised** <!-- .element: class="fragment" data-fragment-index="1" -->



## マイグレーションの準備
* ✅ JDK 9 EA を手に入れよ
* ✅ 再コンパイル前に実行せよ
* \> **第三者のライブラリを更新せよ**
* アプリをコンパイルせよ
* jdeps を実行せよ



### 第三者のライブラリを更新せよ
* FLOSS の JDK 9 テスト状況
    * FLOSS: Free/Libre and Open Source Software
    * [Quality Outreach](https://wiki.openjdk.java.net/display/quality/Quality+Outreach) に**申告ベース**で掲載されている



Sep 7, 2017
* ✅ アクティブ | ⭐ 断続的

|Quality Outreach FOSS |JDK9b181|
|-------------|--------|
|Apache Ant   | ✅ |
|Apache Maven | ✅ |
|Gradle       | ⭐ |
|Groovy       | ✅ |
|Clojure      | ⭐ |
|Scala        | ⭐ |



|Quality Outreach FOSS |JDK9b181|
|-------------|--------|
|Spring Framework | ✅ |
|Apache Tomcat    | ✅ |
|Eclipse Jetty    | ✅ |
|RedHat Netty     | ⭐ |
|Hibernate        | ⭐ |
|Jackson          | ✅ |
|SLF4J            | 不明 |
|Apache Commons   | ⭐ |



|Quality Outreach FOSS |JDK9b181|
|-------------|--------|
|JUnit 5      | ✅ |
|CheckStyle   | ⭐ |
|FindBugs     | ✅ |
|JaCoCo       | ✅ |



## マイグレーションの準備
* ✅ JDK 9 EA を手に入れよ
* ✅ 再コンパイル前に実行せよ
* ✅ 第三者のライブラリを更新せよ
* \> **アプリをコンパイルせよ**
* jdeps を実行せよ



### アプリをコンパイルせよ
* 警告とエラーを確認する
    * JDK 9 のコンパイル エラー (詳しくは後半で)
        * JDK 9 では変数名 "_" 1 文字が使えない
        ```java
        static Object _ = new Object();
        // error: as of release 9, '_' is a keyword, and may not be used as a legal  identifier.
        ```
        * JDK 9 では javac の `-source`/`-target` オプションで 5 (Java 5) 以下がエラー ([JEP 182](http://openjdk.java.net/jeps/182))
        ```bash
        javac -source 5 -target 5 Sample.java
        error: Source option 1.5 is no longer supported. Use 1.6 or later. 
        error: Target option 1.5 is no longer supported. Use 1.6 or later.
        ```



## マイグレーションの準備
* ✅ JDK 9 EA を手に入れよ
* ✅ 再コンパイル前に実行せよ
* ✅ 第三者のライブラリを更新せよ
* ✅ アプリをコンパイルせよ
* \> **jdeps を実行せよ**



### jdeps を実行せよ
* `jdeps` コマンドでライブラリの依存関係を調査する
    * -jdkinternals オプション
        * JDK の内部 API でクラス・レベルの依存関係を検索

```bash
>jdeps -jdkinternals Sample.class
Sample.class -> JDK removed internal API
   Sample  -> sun.misc.BASE64Encoder  JDK internal API (JDK removed internal API)

Warning: JDK internal APIs are unsupported and private to JDK implementation that are
subject to be removed or changed incompatibly and could break your application.
Please modify your code to eliminate dependency on any JDK internal APIs.
For the most recent update on JDK internal API replacements, please check:
https://wiki.openjdk.java.net/display/JDK8/Java+Dependency+Analysis+Tool

JDK Internal API                         Suggested Replacement
----------------                         ---------------------
sun.misc.BASE64Encoder                   Use java.util.Base64 @since 1.8
```
<!-- .element: class="fragment" data-fragment-index="1" -->



## マイグレーションの**準備**
* ✅ JDK 9 EA を手に入れよ
* ✅ 再コンパイル前に実行せよ
* ✅ 第三者のライブラリを更新せよ
* ✅ アプリをコンパイルせよ
* ✅ jdeps を実行せよ

### done!



#### ここからが
## 目次
* \> **JDK/JRE を変更する**
* 削除または変更された API
* デプロイ
* GC の変更
* 削除されたツール
* 削除された macOS 仕様



## JDK/JRE を変更する
* バージョン スキームが変更される
* [JEP 223: New Version-String Scheme](http://openjdk.java.net/jeps/223)
```bash
$MAJOR.$MINOR.$SECURITY+$BUILD
```

| | |
|--|-----------|
|旧|1.9.0_5-b20|
|新|9.0.1+20|

たとえば，npm [locate-java-home](https://github.com/jvilk/locate-java-home/blob/master/ts/lib/locate_java_home.ts#L172) では…
```ts
    var versionData = /(\d+\.\d+\.\d+)/.exec(output);
    var version = "0.0.0";
    if (versionData !== null) {
      version = versionData[1];
    }
```
<!-- .element: class="fragment" data-fragment-index="1" -->



## JDK/JRE を変更する
* JDK と JRE のファイル構成が変更される
* [JEP 220: Modular Run-Time Images](http://openjdk.java.net/jeps/220)
    * JDK 8 以前
        * JDK をインストールすると `jdk/jre/`
        * JRE をインストールすると `jre/`
        * `jdk/jre/` と `jre/` で同じバイナリが存在していた
    * JDK 9 以降
        * JDK と JRE のファイル構成が整理されバイナリ重複なし
            * bin/
            * conf/
            * lib/



## JDK/JRE を変更する
* lib ディレクトリから `rt.jar`，`tools.jar`，`dt.jar` が削除
    * [JEP 220: Modular Run-Time Images](http://openjdk.java.net/jeps/220)
    * tools.jar
        * IDE を作っている人は涙目<!-- .element: class="fragment" data-fragment-index="1" -->
        * 例えば VS Code の Scala LSP (ENSIME) はユーザ側の tools.jar を使用<!-- .element: class="fragment" data-fragment-index="1" -->
        * JDK 9 だと tools.jar (18MB) がない。パッケージ化も考える<!-- .element: class="fragment" data-fragment-index="1" -->
        * vscode-javac では NetBeans 軽量版の tools.jar (3 MB) を使用<!-- .element: class="fragment" data-fragment-index="10" -->



## JDK/JRE を変更する
* 拡張機能 (Extension) 機構が削除された
    * [JEP 220: Modular Run-Time Images](http://openjdk.java.net/jeps/220)
    * JVM に共通の jar をロードできる機能
    * 拡張機能機構を設定していると JDK 9 ではエラーになる
        * `java.ext.dirs` システム プロパティ を設定している
        * `lib/ext` ディレクトリに jar を配備している
    * 上記を削除して，代わりに -classpath で読み込む<!-- .element: class="fragment" data-fragment-index="1" -->

```bash
<JAVA_HOME>/lib/ext exists, extensions mechanism no longer supported; Use -classpath instead.
.Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.
```
<!-- .element: class="fragment" data-fragment-index="1" -->



## JDK/JRE を変更する
* 承認済みの標準オーバーライド機構が削除された
* [JEP 220: Modular Run-Time Images](http://openjdk.java.net/jeps/220)
    * 標準のパッケージを上書きできる機能
    * 承認済みの標準オーバーライド機構を設定していると JDK 9 ではエラーになる
        * `java.endorsed.dirs` システム プロパティを設定している
        * `lib/endorsed` ディレクトリに jar を配備している

```bash
<JAVA_HOME>/lib/endorsed is not supported. Endorsed standards and standalone APIs
in modular form will be supported via the concept of upgradeable modules.
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.
```



## 目次
* ✅ JDK/JRE を変更する
* \> **削除または変更された API**
* デプロイ
* GC の変更
* 削除されたツール
* 削除された macOS 仕様



### 削除または変更された API
* 削除された API
    * sun.*
        * `BASE64Encoder` や `BASE64Decoder` は `java.util.Base64` (JDK 8) を使う
        * 例外的に残した API
            * `jdk.unsupported` モジュールに移動
                * sun.misc.{Signal,SignalHandler}
                * sun.misc.Unsafe ([JEP 193: Variable Handles](http://openjdk.java.net/jeps/193))
                * sun.reflect.Reflection::getCallerClass(int) ([JEP 259: Stack-Walking API](http://openjdk.java.net/jeps/259))
                * sun.reflect.ReflectionFactory.newConstructorForSerialization



### 削除または変更された API
* 削除された API
* [JEP 162: Prepare for Modularization](http://openjdk.java.net/jeps/162)
    * java.util.logging.LogManager.addPropertyChangeListener
    * java.util.logging.LogManager.removePropertyChangeListener
    * java.util.jar.Pack200.Packer.addPropertyChangeListener
    * java.util.jar.Pack200.Packer.removePropertyChangeListener
    * java.util.jar.Pack200.Unpacker.addPropertyChangeListener
    * java.util.jar.Pack200.Unpacker.removePropertyChangeListener



### 削除または変更された API
* 削除された API
    * java.awt.peer
    * java.awt.dnd.peer
    * com.sun.image.codec.jpeg (JDK 1.2)
        * 代わりに javax.imageio (JDK 1.4) を使う
    * sun.rmi.transport.proxy.logLevel
    * sun.rmi.transport.tcp.proxy
    * sun.rmi.transport.proxy.connectTimeout
    * sun.rmi.transport.proxy.eagerHttpFallback
    * java.rmi.server.disableHttp



### 削除または変更された API
* `Thread.stop(Throwable)` が削除された
* `Thread.stop()` は JDK 8 に引き続き非推奨
    * `conf/security/java.policy` からデフォルトで `stopThread` が削除され，Thread.stop() は使えなくなる
    * JDK 9 で Thread.stop() を使う方法
    ```bash
    permission java.lang.RuntimePermission "stopThread";
    ```
    <!-- .element: class="fragment" data-fragment-index="1" -->



### 削除または変更された API
* java/javac の `-profile` コンパクト オプションが削除された
* [JEP 200: The Modular JDK](http://openjdk.java.net/jeps/200)
    * `-profile` は JDK 8 で追加された
    * 使用しない機能をストレージに割り当てないので，リソースに制約のあるデバイス上で Java アプリを実行できる
        * それはモジュール システムで OK
    * JDK 9 で -projile コンパクト オプションを使う方法
        * javac --release 8 -profile compat1 Main.java を使う<!-- .element: class="fragment" data-fragment-index="1" -->

        * java の -profile は完全に削除<!-- .element: class="fragment" data-fragment-index="1" -->




### 削除または変更された API
* 非推奨になった API を一括で調べる方法
    * jdeprscan

```bash
$ jdeprscan [ options ]{dir|jar|class}
class org/apache/commons/math3/util/MathUtils uses deprecated method java/lang/Double::<int>(D)V
class org/apache/commons/math3/util/Precision uses deprecated method java/math/BigDecimal::setScale(II)Ljava/math/BigDecimal;  
```
<!-- .element: class="fragment" data-fragment-index="1" -->



## 目次
* ✅ JDK/JRE を変更する
* ✅ 削除または変更された API
* \> **デプロイ**
* GC の変更
* 削除されたツール
* 削除された macOS 仕様



### デプロイ
* java `-version:` オプションが削除された
    * manifest ファイルの JRE のバージョン指定は警告 (JDK 10 でエラーにする予定)
    * [JEP 231: Remove Launch-Time JRE Version Selection](http://openjdk.java.net/jeps/231)
* applet タグの object属性，object と java object applet パラメータ タグは無視される
* JNLP の XML が標準パーサになり，`&amp;` を `&` で記述できるようになった
    * [JSR 056 for version 9](https://jcp.org/aboutJava/communityprocess/maintenance/jsr056/9.html)



## 目次
* ✅ JDK/JRE を変更する
* ✅ 削除または変更された API
* ✅ デプロイ
* \> **GC の変更**
* 削除されたツール
* 削除された macOS 仕様



### GC の変更
* [Garbage-First Garbage Collector (`G1`)](http://www.oracle.com/technetwork/jp/articles/java/garbage-first-collector-427543-ja.html) がデフォルトに設定される
    * JDK 8 はデフォルトが Parallel GC だった
    * G1 は GC の最大停止時間 (-XX:MaxGCPauseMillis) を設定できる
* `-XX:MaxPermSize=size` が削除された
* `-XX:PermSize=size` が削除された
    * [JEP 122: Remove the Permanent Generation](http://openjdk.java.net/jeps/122)
* GC ログが変更されるので GC ログのパーサは変更が必要になりそう
    * [JEP 271: Unified GC Logging](http://openjdk.java.net/jeps/271)



## 目次
* ✅ JDK/JRE を変更する
* ✅ 削除または変更された API
* ✅ デプロイ
* ✅ GC の変更
* \> **削除されたツール**
* 削除された macOS 仕様



### 削除されたツール
* JavaDB (Apache Derby) が削除された
    * JDK 7 から `jdk/db/` に格納されていた
    * 必要な場合は [Apache Derby](https://db.apache.org/derby/derby_downloads.html) からダウンロード
* JVM TI hprof Agent が削除された
    * heap dump は `jmap` を使う
* jhat が削除された
    * JDK 6 で追加されたヒープの可視化ツール



### 削除されたツール
* RMI CGI プロキシが削除された
    * bin/java-rmi.exe (Windows)
    * bin/java-rmi.cgi (Linux, Solaris)
* JMX RMIConnector の IIOP (Internet Inter-ORB Protocol) 通信 が削除された
    * javax.management.remote.rmi.RMIIIOPServerImpl が非推奨
    * 次の 2 クラスが生成されなくなった
        * org.omg.stub.javax.management.rmi._RMIConnection_Stub
        * org.omg.stub.javax.management.rmi._RMIConnection_Tie



### 削除されたツール
* Windows 32-bit クライアント VM モード (-client オプション) が削除された
    * Windows 32-bit サーバ VM モード (-server) はある
* Visual VM が削除
    * [NetBeans Profiler](https://blogs.oracle.com/nbprofiler/) を使う
* native2ascii が削除
    * ファイルを UTF-8 変換するツール
    * propeties ファイルが直接読めない例のヤツ



## 目次
* ✅ JDK/JRE を変更する
* ✅ 削除または変更された API
* ✅ デプロイ
* ✅ GC の変更
* ✅ 削除されたツール
* \> **削除された macOS 仕様**



### 削除された macOS 仕様
* com.apple.eawt と com.apple.eio はデフォルトでアクセスできない
* com.apple.concurrent と apple.applescript は削除
* AppleScript エンジンが削除
    * Apple 版の JDK 7 と JDK 8 で AppleScriptEngine.jar が含まれていた
* com.apple.concurrent.Dispatch が削除
    * 移行するには次の標準 API を implement する
        * java.util.concurrent.Executor
        * java.util.concurrent.ExecutorService



## 目次
* ✅ JDK/JRE を変更する
* ✅ 削除または変更された API
* ✅ デプロイ
* ✅ GC の変更
* ✅ 削除されたツール
* ✅ 削除された macOS 仕様

done!



## まとめ
* JDK 9 に移行するメリットは何ですか?
* JDK 9 は破壊的変更が発生している
* JDK 9 はモジュール システムによる構成変更の影響が大きい印象
* JDK 9 ではニッチな機能も削除されている
* JDK 9 リリースされるの?<!-- .element: class="fragment" data-fragment-index="1" -->



## 参考ノート
1. Oracle, "Java Platform, Standard Edition Oracle JDK 9 Migration Guide," https://docs.oracle.com/javase/9/migrate/toc.htm
