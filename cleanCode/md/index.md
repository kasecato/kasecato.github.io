## TL;DR Clean Code アジャイルソフトウェア達人の技

<a href="https://twitter.com/kasecato" target="_blank"><img style="border-radius: 50% !important;" src="img/twitter.jpg" width="200px" alt="Keisuke KATO"></a>

2018/xx/xx <a href="https://twitter.com/kasecato" target="_blank">@kasecato</a>



## About me
```js
const addTimeline2018 = (profile) => {
    profile.timeline.push({
        year: 2018,
        lang: "Java 9",
        company: "Nulab",
        team: "Nulab Apps"
    })
}
```



## [Clean Code アジャイルソフトウェア達人の技](https://www.kadokawa.co.jp/product/301710000205/)
* Robert C. Martin（ロバート・C・マーティン） 著
    * Clean Code: A Handbook of Agile Software Craftsmanship
    * 2008/8/1 (2017/12/18 再販)
    * Pearson Education
    * ¥4,104 💸



## Clean Code に期待したもの
* コードの作法
* アーキテクチャの作法
* リファクタリングの作法



## Clean Code の実際
* 命名
* 関数
* コメント
* 



## 僕が欲しかった Clean Code のまとめを作った



## 命名
* 引数の順序を覚えないといけない問題
```java
asserEquals(expected, actual)
assertExpectedEqualsActual(expeced, actual)
```
* メンバープレフィクスはなし，シンタックス ハイライトで
* 賢いとプロは違う。プロは他の人でも理解可能な透明なコードを書く



## 関数
* try ブロックの中身は関数化しよう
* ダイクストラの構造化プログラミングは入口と出口がひとつであるべきだが，関数が小さければ複数の return や break, continue はよりコードを豊かにする



## コメント
* 意図の説明
* 道標はゴミ
* 



## オブジェクトとデータ構造
* デメテルの法則 (オブジェクトの内部を知るべきではない)



## エラー処理
* スペシャルケースパターン
    * null を返さず空オブジェクトを返す



## 境界
* 外部境界の学習テストを残す
    * 破壊的変更を検知


## まとめ



## 参考ノート
1. アスキードワンゴ,「Ｃｌｅａｎ　Ｃｏｄｅ アジャイルソフトウェア達人の技 コードを書き、読み、洗練する」, https://www.kadokawa.co.jp/product/301710000205/
