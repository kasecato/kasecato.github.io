## WebAuthnで実現する
## パスワードレス認証

<a href="https://twitter.com/kasecato" target="_blank"><img style="border-radius: 50% !important;" src="img/twitter.jpg" width="180px" alt="Keisuke KATO"></a>

#### SecHack365 2019 福岡回
#### @ヌーラボ本社3F

2019/8/22 <a href="https://twitter.com/kasecato" target="_blank">@kasecato</a>



- 2019年8月16日 (金)
- 私立大1年の少年
- 電子計算機使用詐欺と不正アクセス禁止法違反の容疑で逮捕
<!-- .element: class="fragment" data-fragment-index="10" -->
- 大学生4人を同容疑で書類送検
<!-- .element: class="fragment" data-fragment-index="20" -->
- その手口は…
<!-- .element: class="fragment" data-fragment-index="20" -->



<a href="https://mainichi.jp/articles/20190817/k00/00m/040/033000c" target="_blank">
    <img src="img/9.jpg"/>
</a>



## フィッシング

- 1995年の AOL（インタネット サービス プロバイダ）
<!-- .element: class="fragment" data-fragment-index="10" -->
- AOLスタッフを装いパスワードを不正に送信させたかったが，監視されていた
<!-- .element: class="fragment" data-fragment-index="20" -->
- チャット ルームの監視フィルタ回避スクリプトが <>< だった
<!-- .element: class="fragment" data-fragment-index="30" -->
- <>< が魚に見えた
<!-- .element: class="fragment" data-fragment-index="40" -->
- fish (魚) + phreaking (不正に無料で電話をかける行為) = Phishing
<!-- .element: class="fragment" data-fragment-index="50" -->



フィッシングは…
- 1995年から未解決
    - セキュリティ技術の敗北
    - 年間5千億円の影響 (2014年)
- 2005年 [17万件](https://en.wikipedia.org/wiki/Phishing#2010s)
- 2018年 [104万件](https://en.wikipedia.org/wiki/Phishing#2010s)



2017年 Google社員 8万5千人のフィッシング被害が[ゼロ件](https://www.gizmodo.jp/2018/08/google-employees-secret-to-never-getting-phished-is-using-physical-security-keys.html)に



<a href="https://cloud.google.com/titan-security-key/?hl=ja" target="_blank">
    <img src="img/hero_2x.png"/>
</a>



## FIDO U2F

- Google は全社員に「FIDO U2F」の使用を義務付けた
- FIDO U2F?
    - Fast Identity Online = 素早いオンライン認証
<!-- .element: class="fragment" data-fragment-index="10" -->
    - Universal 2nd Factor = ユニバーサルな2要素
<!-- .element: class="fragment" data-fragment-index="10" -->
    - （名前を聞いてもよくわからないと思った）
<!-- .element: class="fragment" data-fragment-index="20" -->
- 2要素認証に物理デバイスを使用する
<!-- .element: class="fragment" data-fragment-index="30" -->
    1. パスワード認証 = 知識の要素
    1. 物理デバイス = 所有の要素 👈 義務化



### 認証の3要素

- 1975年 [FIPS 41](https://www.ncjrs.gov/pdffiles1/Digitization/29281NCJRS.pdf)
    - Computer Security Guidelines for Implementing the Privacy Act of 1974
        - (1) Something the person knows;
        - (2) Something the person has;
        - (3) Something the person is.
    - 712年 古事記に登場する三種の神器 = has 所有の要素
<!-- .element: class="fragment" data-fragment-index="10" -->
- 2019年の私たちはなぜ2段階認証が…
<!-- .element: class="fragment" data-fragment-index="20" -->



なぜ Google が FIDO でフィッシング被害を撲滅できたのか？



## FIDO の基本コンセプト

- 公開鍵暗号を用いたチャレンジレスポンス認証
- Authenticator を用いた秘密鍵の生成と保護
- origin に紐づく認証情報の管理
- ローカルでのユーザー認証

出典: もとほんや, 『[WebAuthn 本](https://techbookfest.org/event/tbf05/circle/28720014)』, p.9, (Oct, 2018). [@watahani](https://twitter.com/watahani)



<a href="https://developers.yubico.com/WebAuthn/WebAuthn_Developer_Guide/Overview.html" target="_blank">
    <img src="img/fido2_app_architecture.png"/>
</a>




### 公開鍵暗号を用いたチャレンジレスポンス

- 1976年 Diffie と Hellman が公開鍵暗号化と認証を提案
- 1977年 Rivest, Shamir と Adleman が公開鍵暗号方式を構築
`$$ C \equiv M^E \pmod N $$`
`$$ M \equiv C^D \pmod N $$`
- 1985年 Koblitz と Miller が楕円曲線暗号を提案
<!-- .element: class="fragment" data-fragment-index="10" -->
    - RSA 2048 ビットを ECC 206 ビット程度で担保
<!-- .element: class="fragment" data-fragment-index="10" -->
    - 組込み向け = FIDO の認証器でも使用
<!-- .element: class="fragment" data-fragment-index="10" -->



### 公開鍵暗号を用いたチャレンジレスポンス

- 1979年頃 Bob Bosen がチャレンジレスポンス認証を実装
- TRS-80 用のゲーム「80 Space Raiders」のコピー防止機能
- ゲーム起動画面に表示されるランダムな数 = チャレンジ
- ゲーム付属の表でチャレンジに対応する数を入力 = レスポンス
`$$ M \equiv C^D \pmod N $$`
`$$ C \equiv M^E \pmod N $$`



### 公開鍵暗号を用いたチャレンジレスポンス

<a href="https://developers.yubico.com/WebAuthn/WebAuthn_Developer_Guide/Overview.html" target="_blank">
    <img src="img/fido2_app_architecture.png" width="440px"/>
</a>

- 登録
    - ブラウザは RP からチャレンジを取得
<!-- .element: class="fragment" data-fragment-index="10" -->
    - 認証器は鍵ペアを生成
<!-- .element: class="fragment" data-fragment-index="20" -->
        - 秘密鍵でチャンレンジに署名 = レスポンス
<!-- .element: class="fragment" data-fragment-index="30" -->
    - RP は公開鍵で署名を検証
<!-- .element: class="fragment" data-fragment-index="40" -->
        - 公開鍵を登録
<!-- .element: class="fragment" data-fragment-index="50" -->



### 公開鍵暗号を用いたチャレンジレスポンス

<a href="https://developers.yubico.com/WebAuthn/WebAuthn_Developer_Guide/Overview.html" target="_blank">
    <img src="img/fido2_app_architecture.png" width="440px"/>
</a>

- 認証
    - ブラウザは RP からチャレンジを取得
<!-- .element: class="fragment" data-fragment-index="10" -->
    - 認証器は秘密鍵を導出
<!-- .element: class="fragment" data-fragment-index="20" -->
        - 秘密鍵でチャレンジに署名 = レスポンス
<!-- .element: class="fragment" data-fragment-index="30" -->
    - RP は署名を検証
<!-- .element: class="fragment" data-fragment-index="40" -->
        - ユーザを認証
<!-- .element: class="fragment" data-fragment-index="50" -->



### Authenticator を用いた秘密鍵の生成と保護

- 1994年 FIPS 140-1
- 2001年 [FIPS 140-2](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.140-2.pdf)
    - Security Requirements for Cryptographic Modules
        - 耐タンパ性
            - プロービング攻撃
            - フォールト攻撃
            - サイドチャネル攻撃
- 2019年9月 FIPS 140-3



### origin に紐づく認証情報の管理

<a href="https://developers.yubico.com/U2F/Protocol_details/Key_generation.html" target="_blank">
    <img src="img/key_generation.png" width="480px"/>
</a>

- AppID (rpId) = origin = ドメイン名
- 鍵ペアの生成にドメイン名が含まれる
    - フィッシング耐性がある
<!-- .element: class="fragment" data-fragment-index="10" -->



### ローカルでのユーザー認証

- リモート認証
    - パスワードはネットワーク上に流れる
    - パスワードはサーバに保存される
        - サーバからの情報漏えい
        - パスワードのリスト型攻撃
- ローカル認証
    - 生体認証や PIN はネットワーク上に流れない
    - 生体情報や PIN はローカルのデバイスに保存される
    - デバイスは耐タンパ性がある



## [WebAuthn](https://www.w3.org/TR/webauthn/) とは

- Web Authentication: An API for accessing Public Key Credentials Level 1
    - ブラウザに実装された 2 つの API
        - 登録 = navigator.credentials.create()
        - 認証 = navigator.credentials.get()
    - ブラウザと認証器が会話するための API
        - 認証器から鍵ペアを生成する
        - 認証器から秘密鍵で署名を生成する



## 登録 = navigator.credentials.create()

<a href="https://www.w3.org/TR/webauthn/#fig-registration" target="_blank">
    <img src="img/webauthn-registration-flow-01.svg"/>
</a>



## 認証 = navigator.credentials.get()

<a href="https://www.w3.org/TR/webauthn/#fig-authentication" target="_blank">
    <img src="img/webauthn-authentication-flow-01.svg"/>
</a>



## まとめ

- FIDO の基本コンセプトがわかった
    - 公開鍵暗号を用いたチャレンジレスポンス認証
    - Authenticator を用いた秘密鍵の生成と保護
    - origin に紐づく認証情報の管理
    - ローカルでのユーザー認証
- WebAuthn はブラウザに実装された 2 つの API
    - 登録 navigator.credentials.create()
    - 認証 navigator.credentials.get()
    - WebAuthn は認証器と会話するための API
- パスワードを使わず公開鍵認証基盤でユーザ認証できた



## 参考ノート

- 毎日新聞, 「ネットバンク不正接続で現金詐取　容疑の大学生逮捕　名古屋」, https://mainichi.jp/articles/20190817/k00/00m/040/033000c
- IPA, 『情報セキュリティ白書2019』, p.20, (August, 2019)
- デービッド・サンガー, 『世界の覇権が一気に変わる サイバー完全兵器』, pp.207-224, pp.152-157, (May, 2019)
- Wikipedia, "Early AOL phishing," https://en.wikipedia.org/wiki/Phishing#Early_AOL_phishing



- ギズモード・ジャパン, 「Google社員のフィッシングを0にした物理キー。余りの効果に、Google｢もう自社でつくる｣、元祖のYubicoは苦笑い」, https://www.gizmodo.jp/2018/08/google-employees-secret-to-never-getting-phished-is-using-physical-security-keys.html
- Google, 「Titan セキュリティ キー」, https://cloud.google.com/titan-security-key/?hl=ja
- FIDO, 「FIDOアライアンスの沿革」, https://fidoalliance.org/fido%e3%82%a2%e3%83%a9%e3%82%a4%e3%82%a2%e3%83%b3%e3%82%b9%e6%a6%82%e8%a6%81/fido%e3%82%a2%e3%83%a9%e3%82%a4%e3%82%a2%e3%83%b3%e3%82%b9%e3%81%ae%e6%b2%bf%e9%9d%a9/?lang=ja
- NIST, "Computer Security Guidelines for Implementing the Privacy Act of 1974," https://csrc.nist.gov/publications/detail/fips/41/archive/1975-05-30
- Richard E. Smith『認証技術 パスワードから公開鍵まで』, p.399, p.255, (April, 2003).



- 板倉征男, 外川政夫, 『ネット社会と本人認証―原理から応用まで』, p.29, (July, 2010).
- もとほんや, 『WebAuthn 本』, p.9, (Oct, 2018).
- 別冊日経サイエンス，「サイバーセキュリティー」, p.63, (April, 2016).
- 﨑山一男, 菅原健, 李陽, 『暗号ハードウェアのセキュリティ』, p.39, pp.80-81, (June, 2019).



- NIST, "Security Requirements for Cryptographic Modules," https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.140-2.pdf
- Wikipedia, "FIPS 140-3", https://en.wikipedia.org/wiki/FIPS_140-3
- Yubico, "Key generation," https://developers.yubico.com/U2F/Protocol_details/Key_generation.html
- Yubico, "FIDO2/WebAuthn Overview," https://developers.yubico.com/WebAuthn/WebAuthn_Developer_Guide/Overview.html
- W3C, "Web Authentication: An API for accessing Public Key Credentials Level 1," https://www.w3.org/TR/webauthn/
