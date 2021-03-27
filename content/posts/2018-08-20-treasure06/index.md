---
templateKey: blog-post
title: "Treasure2018 6日目 セキュリティ"
date: 2018-08-20
slug: treasure06
published: true
unlisted: false
language: ja
cover: ../2018-09-03-summer-internship-2018/all.jpg
imageShare: ../2018-09-03-summer-internship-2018/all.jpg
tags:
  - internship
  - treasure
---

#### 前の記事

[Treasure2018 5 日目](https://yoshikawa.dev/treasure05/)

## セキュリティ

前半で中間課題の発表会

後半でセキュリティ

### やったこと

セキュリティ対策のお勉強をした。

### 分からなかったこと

Vue や React での攻撃の書き方は結構独特だったので、後日試したい。

### memo

#### 情報セキュリティ

- 機密性 (confidentiality): 情報へのアクセスを認められた者だけが、その情報にアクセスできる状態を確保すること
- 完全性 (integrity): 情報が破壊、改ざん又は消去されていない状態を確保すること
- 可用性 (availability): 情報へのアクセスを認められた者が、必要時に中断することなく、情報及び関連資産にアクセスできる状態を確保すること

#### 機密性が損なわれる状態

特定の URL を叩くと見れてしまう

#### 完全生が損なわれる状態

サーバーへの大量アクセスによってデータが壊れてしまった

#### 可用性が損なわれる状態

自分のアカウントに対する不正ログインはされなかったものの、攻撃によってアカウントロックされる。

セキュリティホールを塞ぐために、**セキュリティ対策**をする。
リスク対応。

#### リスク評価

リスク = 脅威の発生可能性 × 脆弱性の影響度

#### 脆弱性対策

「安全なウェブサイトの作り方」では脆弱性対策を以下の 2 種類に分類

- 根本的解決
- 保険的対策

#### 脆弱なサービスやソフトウェアは意外と多い

2018 年の 1Q,2Q だけで 296 件

しかし、この件数は報告されているだけの件数なのでもっと脆弱性のあるサイトは多い

#### CVE 番号がアサインされた脆弱性の件数

2018 年だけで、11302 件

#### 情報セキュリティに関する法律

**何人も、不正アクセス行為をしてはならない**

#### 脆弱性情報をみだりに公開しない

IPA の設ける脆弱性報告窓口を利用するのが良い。

同時に開発者自信にも報告するのも良い。

#### XSS とは

- 攻撃者は、XSS 脆弱性のある Web サイトにユーザを誘導することにより、ユーザ環境で不正スクリプトを実行させることができます
- 攻撃者は、脆弱性利用のために、不正スクリプトを含んだ攻撃用 URL を作りこむ必要があります

反射型

持続型

#### エスケープ

#### 文字参照

HTML において、表現したい文字が文脈上表現できない場合

#### テンプレート

テンプレートエンジンを用いて、エスケープ処理をする。

#### HTML5 における開始タグの構文

`<タグ名 属性名=属性値 属性名=属性値>`

常に 2 重引用符で囲むようにしよう

一重引用符でもよいが、たとえば PHP の htmlspecialchars はデフォルトで一重引用符の処理がなされないので注意

#### same-origin policy

リソースの origin によってアクセスを制御するためのブラウザのセキュリティ機能

「スキーム+ホスト+ポート」

[same-origin policy](https://developer.mozilla.org/ja/docs/Web/Security/Same-origin_policy)

#### HTML 構造の変更を伴わない XSS

HTML は HTML 内に

- JS
- CSS
- SVG

などを含む

#### XSS URI

HTML に埋め込むパラメータが HTTP, HTTPS かどうかを判断する必要がある

#### XSS 対策

- JavaScript を動的に生成しない
- Unicode エスケープシーケンス
- バックスラッシュによるエスケープ

##### JavaScript を動的に生成しない

たとえば、`data-*`属性値として HTML 内に出力したうえで、JavaScript からその値を参照する

##### Unicode エスケープシーケンス

`-uHHHH`のような形式でコードポイントを指定して文字を表現できる

##### バックスラッシュによるエスケープ

推奨しない

#### Go の html/template はクソ強い

値が属性値の文脈に埋め込まれるのか、要素の内容として埋め込まれるのかを自動判別してエスケープする。

`localhost:8000/?msg={{this.$el.ownerDocumetn.defalutView,alert(0)}}`

`localhost:8000?prev_pager[dangerouslySetInnerHTML][__html]=%3Csvg%20onload%3Dalert(0)%3E`

以上が攻撃法の例

#### SQL インジェクション

アプリケーションのセキュリティ上の不備を意図的に利用し、アプリケーションが想定しない SQL 文を実行させることにより、データベースシステムを不正に操作する攻撃方法

DB 内データに対する操作

SQL の実行結果の操作

DB サーバ上ファイルの読み書き、プログラムの実行

**プリペアドステートメントを使う**

[SQLinjection 対策](http://d.hatena.ne.jp/ajiyoshi/20100409/1270809525)

database/sql は組み込みでプリペアドステートメントをサポートしている。

#### プリペアドステートメント自体の動的構築は避ける

#### セッション管理

HTTP における「セッション」

HTTP はステートレスなプロトコル

一方で「セッション」はステートフルな概念

#### セッションハイジャック

- セッション ID が推測可能な状態
- セッション ID が漏洩しうる状態

CSPRNG により生成された値を用いる

Go なら単に crypto/rand を使えば良い

通常の疑似乱数生成器は使うべきでない

シードが弱い恐れ有り

過去の結果から未来の生成結果を予測可能である可能性がある

#### 認証

認証の「要素」

- 記憶
- 所有
- 生体

##### 認証とは呼べないもの

メールアドレスだけによる「ログイン」

いわゆる携帯 ID を用いた「簡単ログイン」

##### 多要素認証とは呼べないもの

- パスワード + CAPTCHA
- パスワード + 秘密の質問
- パスワード + 暗証番号
- パスワード + 第 2 パス

##### 総当たり攻撃

長くて複雑なパスワードを作成させる。

##### 逆総当たり攻撃

弱いパスワードを拒否する。

IP アドレスベースでロックする。

サイト全体でのログイン失敗率が上昇した場合、CAPTCHA の入力を求める。

##### 類推攻撃

一部プロフィール情報については機械的にそういったパスワードをつけさせないようにする対策が可能

[https://www.teamsid.com/worst-passwords-of-2014/](https://www.teamsid.com/worst-passwords-of-2014/)

[最悪なパスワード集 2017](https://s13639.pcdn.co/wp-content/uploads/2017/12/Top-100-Worst-Passwords-of-2017a.pdf)

##### 辞書攻撃

よく用いられがちなパスワードを登録しようとした場合は拒否する

##### オフラインでの事前計算攻撃

- 一方向ハッシュ関数によるダイジェスト値が格納されているパスワード DB を盗み出し、オフラインにおいて総当たり攻撃や辞書攻撃を行う
- オフラインでの攻撃であるため止めることが出来ない上、極めて高速かつ並列で計算が行える
- 予め考えうるダイジェスト値の組み合わせを準備しておけば瞬時にパスワードを割り出せてしまう

ソルト値と組み合わせる。

[https://godoc.org/golang.org/x/crypto/bcrypt](https://godoc.org/golang.org/x/crypto/bcrypt)

##### パスワードリスト攻撃

利用者側の対策としてはパスワードを使い回さない。

#### CSRF

攻撃者はブラウザなどのユーザ・クライアントを騙し、意図しないリクエスト（たとえば HTTP リクエスト）を Web サーバに送信させる。

Cookie や HTTP 認証による認証継続用トークンを用いる。

POST/PUT/DELETE は大事な処理。

#### 次の記事

[Treasure2018 7 日目](https://yoshikawa.dev/treasure07/)