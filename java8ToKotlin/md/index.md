### ヌーラボのアカウント基盤 API を
### Java から Kotlin に移行して気づいた
### 3 つのメリット・デメリット

<a href="https://twitter.com/kasecato" target="_blank"><img style="border-radius: 50% !important;" src="img/twitter.jpg" width="140px" alt="Keisuke KATO"></a>

### [Kotlin Fest 2018](https://kotlin.connpass.com/event/91666/)

2018/8/25 <a href="https://github.com/kasecato" target="_blank">@kasecato</a> from <a href="https://nulab-inc.com/">Nulab</a>



## ヌーラボではアカウント基盤をリデザイン中
「ユーザ体験を改善したい」

|                     | Before                               | After  |
|:--------------------|:-------------------------------------|:-------|
| サーバサイド       | Java 8                               | Kotlin |
| クライアントサイド | Haxe<br/>Knockout.js<br/>Thymeleaf 3 | React  |



## なぜ Kotlin ？
- タテマエ
    - モダンな言語で生産性を高めたい
- ホンネ
    - 「Java」というだけで世間の目が…
        - ヌーラボには Scala と Go の開発者が多い
        - 世間の [Java dis](https://twitter.com/search?f=tweets&vertical=default&q=java%20%E3%83%AC%E3%82%AC%E3%82%B7%E3%83%BC) で下がり続ける開発モチベーション
    - 「再構築なのに新しいことには挑戦しないのか？」という葛藤
    - しかし鍛えられた貴重な資産があるので JVM は捨てたくない



### 『Kotlinイン・アクション』を読んだ
<img  src="img/kotlin_in_action.jpg" width="380px">



## 『Kotlinイン・アクション』 序文

> JavaのコードベースはJetBrainsにとって非常に貴重な資産であり、これを失うこと、あるいは相互運用性の困難さによって価値を下げることは回避しなければなりません 

- 同じ課題設定を解決した言語があった！
- 『Kotlinイン・アクション』 序文に深く共感した！



## 1 週間後 Java 8 を Kotlin に全移行



## メリット 1
## コード行数が -22% 減少

|            |      |  Java 8 | Kotlin | 増減率 |
|:-----------|:-----|--------:|-------:|-------:|
| コード行数 | main | 10,228L | 7,958L |   -22% |
|            | test |  6,509L | 7,018L |    +7% |

- [Kotlin 公式では概算で 40% 削減可能](https://kotlinlang.org/docs/reference/faq.html#what-advantages-does-kotlin-give-me-over-the-java-programming-language)
- [フランス NPO の開発事例でも 1 万行 が 8 千行に](https://blog.ninja-squad.com/2018/05/22/kotlin-migration/)



## なぜコード行数が 22% も減少?
- Kotlin は簡潔
    - 特にコレクション操作が素晴らしい

```java
// Java
final List<Integer> accountIds = accounts.stream()
        .map(Account::getId)
        .collect(Collectors.toList());
```

```kotlin
// Kotlin
val accountIds = accounts.map { it.id }

// メンバ参照もある; map(Account::getId)
// 遅延コレクションもある; asSequence() を使用
```



- Spring コンストラクタ インジェクション

```java
// Java
@Service
public class DbAccountService implements AccountService {

	private final RiskAssessor riskAssessor;

	public DbAccountService(final RiskAssessor riskAssessor) {
		this.riskAssessor = riskAssessor;
	}

}
```

```kotlin
// Kotlin
@Service
class DbAccountService (private val RiskAssessor riskAssessor): AccountService {

}

// プライマリ コンストラクタで初期化されたプロパティも定義可能
```



## デメリット 1
## コンパイル時間が 2 秒増加

|                |      | Java 8 | Kotlin | 増減率 |
|:---------------|:-----|-------:|-------:|-------:|
| コンパイル時間 | main |   5.5s |   7.6s |   +70% |
|                | test |   5.0s |   8.9s |   +78% |

- 「2秒」気にしますか？
- コード行数が増えればもっと遅くなるのでは？
- Scala より速ければ(ry



## デメリット 2
## 366 個のテスト時間が 13 秒増加

|          |      | Java 8 | Kotlin | 増減率 |
|:---------|:-----|-------:|-------:|-------:|
| 実行時間 | test |  46.5s |  59.5s |   +28% |

- [「Kotlinの隠れたコストについてのベンチマーク」](https://postd.cc/kotlinshiddencosts-benchmarks/)
    - Kotlin が高速: ラムダ式，コンパニオン オブジェクト，ローカル関数，null 安全
    - Kotlin が低速: 可変引数＋スプレッド演算子，デリゲート プロパティ



## まとめ
[Nulab Apps チームでは Kotliner を待っています！](https://www.wantedly.com/projects/75901)



## ありがとうございました 🙇‍
<a href="https://nulab-inc.com/"><img src="img/nulab_logo_color.svg" width="300px" alt="nulab" /></a>
### by [Nulab Apps チーム](https://nulab-inc.com/ja/about/careers/nulab-apps-developer/)
## Kotlin かわいい！(´Д｀)(´Д｀)



## ビルド環境
- MacBook Pro (15-inch, 2016)
- Processor: Intel Core i7 2.9 GHz
- Memory: 16GB 2133 MHz LPDDR3
- Java HotSpot(TM) 64-Bit Server VM (build 25.171-b11, mixed mode)
- Kotlin version 1.2.51-release-125 (JRE 1.8.0_171-b11)
- 標本数: 5 (90 パーセンタイル値)
- Clean & Build
    - $./gradlew :kotlinProj:clean build --profile
