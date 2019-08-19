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



### 認証の要素
- 1975年 [FIPS 41](https://www.ncjrs.gov/pdffiles1/Digitization/29281NCJRS.pdf)
    - Computer Security Guidelines for Implementing the Privacy Act of 1974
        - (1) Something the person knows;
        - (2) Something the person has;
        - (3) Something the person is.
- 2019年の私たちはなぜ2段階認証が…
<!-- .element: class="fragment" data-fragment-index="10" -->



## FIDO の基本コンセプト
- 公開鍵暗号を用いたチャレンジレスポンス認証
- Authenticator を用いた秘密鍵の生成と保護
- origin に紐づく認証情報の管理
- ローカルでのユーザー認証

もとほんや, 『[WebAuthn 本](https://techbookfest.org/event/tbf05/circle/28720014)』, p.9, (2018).



## 登録 = [navigator.credentials.create](https://w3c.github.io/webappsec-credential-management/#dom-credentialscontainer-create)
![MDN WebAuthn Registration](img/MDN_Webauthn_Registration_r3.png)



## 認証 = [navigator.credentials.get](https://w3c.github.io/webappsec-credential-management/#dom-credentialscontainer-get)
![MDN WebAuthn Authentication](img/MDN_Webauthn_Authentication_r1.png)



## 参考ノート
