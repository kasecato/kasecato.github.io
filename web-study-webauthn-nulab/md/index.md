## WebAuthN
## 実際に導入してどうだったか

<a href="https://twitter.com/kasecato" target="_blank"><img style="border-radius: 50% !important;" src="img/twitter.jpg" width="180px" alt="Keisuke KATO"></a>

#### [web-study](https://web-study.connpass.com/) - [WebAuthN Study](https://web-study.connpass.com/event/149833/)
#### @[サイボウズ 東京オフィス](https://cybozu.co.jp/company/access/tokyo/)

2019/10/28 <a href="https://twitter.com/kasecato" target="_blank">@kasecato</a>



## WEB+DB PRESS Vol.114

![WEB+DB PRESS Vol.114](img/webdb_vol114.png)



## 👉導入前の話



### WebAuthN 導入のモチベーションは何か
## 💵

> セキュリティ キーや顔認証でログインしたい<br>
> ── Cacoo チーム [@shoozhoo](https://twitter.com/shoozhoo)

- 2018年4月
    - 社内ハッカソンで実装
    - [Google WebAuthnDemo](https://github.com/google/webauthndemo) を改造
    - 単要素ユーザネームレス認証に対応 (社内アカウント限定)
    - Credential ID は local storage にストア



<div class="tweet" data-src="https://twitter.com/kasecato/status/987311137719894017"></div>



### 2018年4月 WebAuthN をリリースしなかった理由

- ユーザ エージェントへの不安
    - ハッカソン発表当日に Chrome 65 => 66 の更新でエラー (April 17, 2018)
        - [WebAuthn 20 March 2018](https://www.w3.org/TR/2018/CR-webauthn-20180320/#dictdef-tokenbinding) 仕様で Token Binding のインタフェースが変わった
        - 今後も仕様変更が続くとメンテナンスに不安
    - Edge の WebAuthn API に Polyfill が必要だった
        - makeCredential/getAssertion <=> create/get
        - 今後も Web ブラウザの実装差分を追うのは不安



### 2018年4月 WebAuthN をリリースしなかった理由

- W3C WebAuthn 仕様への不安
    - 勧告候補から勧告に進むまで欠陥が発見されないか不安
    - ユーザが登録した公開鍵クレデンシャルが WebAuthn の仕様変更で動作しなくなる = 夜も眠れなくなる不安
    - 自分たちの WebAuthn の使い方が正しいのか確信が持てなかった
        - 単要素パスワードレス認証と 2FA OTPでは単要素では？
        - local storage ではなくないか？



### 2018年4月 WebAuthN をリリースしなかった理由

- 認証器の普及率への不安
    - セキュリティ キーをユーザが購入する方法では普及するとは思えなかった (U2F)
        - FIDO2 対応のクロスプラットフォーム セキュリティ キーも当時は存在しなかった
    - スマートフォンは理想のセキュリティ デバイスだった (UAF)
        - iPhone (Safari) の WebAuthn サポート予定が当時は存在しなかった
        - Android は…



### 2019年10月 WebAuthN の状況は

- W3C WebAuthn 仕様
    - 2019年3月に勧告
- ユーザ エージェント
    - Brave, Chrome, Chromium, Edge, Firefox, Opera, Safari
    - Android (>= 7.0), Windows 10 (>= Version 1903)
- 認証器
    - FIDO2
        - Android 指紋認証, Touch ID (Chrome for macOS)
        - BioPass FIDO2, SoloKeys, YubiKey 5
    - FIDO1
        - Titan セキュリティ キー



<img src="img/fido_october_2019.svg" width="80%">



## 👈導入前の話
## 👉導入中の話



### ログインをどのような設計に落としたか
- 認証フローの設計
    - 2要素認証
        - ユーザは本当にうれしいのか？
        - 利便性の真の改善にはつながらない
    - 単要素パスワードレス認証
        - 多要素を強制したいフローで困る
    - 多要素パスワードレス認証
        - 利便性と安全性を両立
        - FIDO2 認証器が少しだけ普及してきた
    - ユーザネームレス認証
        - Windows 10 以外で普及したとは思えなかった (2019年5月)



### ログインをどのような設計に落としたか
- 多要素パスワードレス認証を採用
    - 登録
        - 無心でコーディング…
    - 認証
        - Email をキーに allowCredentials を取得
        - [Email First Sign-in Flow](https://www.chromium.org/developers/design-documents/form-styles-that-chromium-understands) を参考にした
    - ライブラリ
        - たくさんある



<img src="img/nulab_beta_registration_before.png" width="70%">



<img src="img/nulab_beta_authentication_username.png" width="50%">



<img src="img/nulab_beta_authentication_credential.png" width="50%">



<div class="tweet" data-src="https://twitter.com/kasecato/status/1138441427468161024"></div>



<div class="tweet" data-src="https://twitter.com/kasecato/status/1138442950617096192"></div>



### ログインをどのような設計に落としたか
- 強力なベータ テストのレビュワに参加してもらった
    - [@ritou](https://twitter.com/ritou)
    - [@agektmr](https://twitter.com/agektmr)
    - 匿名



#### 🤔 サイド メニューへのフィードバック
<img src="img/nulab_beta_feedback_ritou_security_sidemenu_before.png" width="70%">



#### 🎉 サイド メニューをネスト化して改善
<img src="img/nulab_beta_feedback_ritou_security_sidemenu_after.png" width="70%">



#### 🤔 名称と登録機能へのフィードバック
<img src="img/nulab_beta_feedback_agektmr_security_key_before.png" width="70%">



#### 🎉 セキュリティ デバイスに名称変更して改善
#### 🎉 生体認証のユーザ ヘルプを画面に追加して改善
#### 🎉 パスワードレス認証には ISUVPAA による制限を設けて改善
<img src="img/nulab_beta_feedback_agektmr_security_key_after.png" width="50%">



#### 🤔 サポート環境へのフィードバック
<img src="img/nulab_beta_feedback_anonymous_support_browsers_before.png" width="70%">



#### 🎉 サポート環境は外部のヘルプ デスクに用意
<img src="img/nulab_beta_feedback_anonymous_support_browsers_after.png" width="50%">



### アカウント リカバリの設計
- 認証器を盗難／損傷／紛失したユーザ
    - 従来のパスワード認証でログインして認証器の登録を削除
- パスワードを削除したユーザ
    - パスワード認証を復活させる
        - パスワード リセットを E メールで送信



### 仕様の網羅 (どこまで対応したのか)
- Attestation Statement
    - Basic, Self, AttCA, ECDAA, None
    - packed, tpm, android-key, android-safetynet, fido-u2f, none
- FIDO Metadata Service (MDS)
- Resident Key
- Token Binding
- Extensions (WebAuthn)
- HMAC Secret Extension (CTAP2)
- [RS256 (-257)](https://www.iana.org/assignments/cose/cose.xhtml#algorithms) expires 2020-04-19



### 対応端末や実際の認証器は
- Windows 10 バージョン 1903 以降
    -  Windows Hello 顔認証は素早く便利 (PC の性能による)
    - プラットフォーム認証器 => ローミング認証器の切替えに注意
- Chrome for macOS  Touch ID
    - 社内では概ね好評
- Android 7.0 以上
    - Chrome WebView に対応すると指紋認証が使えるので便利
- FIDO2 セキュリティ キーは NFC/BLE が便利
    - ISUVPAA に注意



### 実装が困難だった点
- Chrome for macOS の Touch ID 複数登録の後勝ち問題
    1. パスワードレス認証で Touch ID を登録
    2. 2FA で Touch ID を登録
        - 2FA では Touch ID は使える (後勝ち)
        - パスワードレス認証で Touch ID が使えなくなる
        - [Demo](https://webauthntest.azurewebsites.net/)
    - Chromium は rp.id + user.id (user handle) をキーに上書き?
        - https://chromium.googlesource.com/chromium/src/+/refs/tags/80.0.3951.2/device/fido/mac/make_credential_operation.mm#112



### 実装が困難だった点
- エラー ハンドリング
    - 詳細を忘れてしまった 🙇
    - DOMException が Web ブラウザ毎に解釈が違った?
    - or DOMException から的確な警告を出せなかった?



### 実装が困難だった点
- アカウント新規作成でパスワードレス
    - アカウント作成の入り口が backlog.com，cacoo.com と typetalk.com に分かれている
        - FIDO AppID Extension (appid) で解決できそうでは?



### 実装が困難だった点 - Confidential 🚫
- 新しいサインイン画面への移行 🙇

<img src="img/nulab_design_new_signin_flow.png" width="70%">



## 👈導入中の話
## 👉導入後の話



### 指紋が変わって生体認証が使えなくなったらどうすれば
- PIN で認証器を解除して生体情報を再登録
    - 生体情報が RP サーバ側にある印象を持たれる
    - ローカル認証のメンタル モデルがまだ浸透していない



### パスワード削除でモバイルが使えない
- パスワード削除機能と 2FA の強制
    1. 管理者が 2FA を強制
    2. ユーザが多要素パスワードレス認証を設定
        - パスワード認証が残っているため，2FA 強制は NG
    3. ユーザがパスワード認証を削除 (2FA も自動削除)
        - パスワード認証が無効化されたため，2FA 強制は OK
        - ユーザは全端末から強制サインアウト
            - ユーザがモバイルからも強制サインアウト
            - ユーザはモバイルでアプリが使えない
    7. ユーザはパスワード認証を再設定



### プラットフォーム WebAuthn API と ローミング認証器
- プラットフォーム セキュリティ モジュールが未搭載のデバイス上で，FIDO2 ローミング認証器が使えない
    - ISUVPAA

<a href="https://developers.yubico.com/WebAuthn/WebAuthn_Developer_Guide/Overview.html">
    <img src="img/fido2_app_architecture.png" style="max-width: 600px">
</a>



### Touch ID が使えなくなった
- 2FA で登録した Chrome for macOS の Touch ID が使えない
    - 公開鍵クレデンシャル登録時の Chrome の Profile に紐付く
    - [Demo](https://webauthntest.azurewebsites.net/)



### 使用可能な認証器がわからない
- 使用可能なデバイスやセキュリティ キーを書くとよさそう
    - すべての認証器をすべての環境で試す環境は持っていないのでヘルプ化し難い



### まとめ
- ✅ 標準化
- ✅ ユーザ エージェント
- ✅ 認証器
- ✅ RP サーバ ライブラリ
- □ Web サービスに実装！
- @ritou さん [builderscon](https://ritou.hatenablog.com/entry/2019/08/30/215734) で Nulab をありがとうございました 🎉



### 逆質問
1. Relying Party は[何に Relying している](https://developers.yubico.com/WebAuthn/WebAuthn_Developer_Guide/Overview.html#_fido2_webauthn_application_architecture)のか?
1. user.id (user handle) にはなぜ [email を設定してはならない](https://www.w3.org/TR/webauthn/#user-handle)のか?
1. RP サーバが Store する最小限の[公開鍵クレデンシャル](https://www.w3.org/TR/webauthn/#fig-attStructs)とは?
1. [ECDAA](https://ja.wikipedia.org/wiki/WebAuthn#%E6%89%B9%E5%88%A4) は本当に安全なのか?
1. [appid extension](https://www.w3.org/TR/webauthn/#sctn-appid-extension) は ドメインが異なる空間で利用できるのか?
1. rpId は [DNS Spoofing](https://ja.wikipedia.org/wiki/DNS%E3%82%B9%E3%83%97%E3%83%BC%E3%83%95%E3%82%A3%E3%83%B3%E3%82%B0) に耐性があるのか?
1. [HMAC Secret](https://fidoalliance.org/specs/fido-v2.0-ps-20190130/fido-client-to-authenticator-protocol-v2.0-ps-20190130.html#sctn-hmac-secret-extension) とは何か?
1. [Token Binding](https://tools.ietf.org/html/rfc8471) とは何か? なぜ Chrome から削除?
1. [BLE](https://ftsafe.co.jp/news/feitian_fido_k13/) は安全なのか?
1. [ebay の UAF オープンソース](https://github.com/eBay/UAF)は使えるのか?
