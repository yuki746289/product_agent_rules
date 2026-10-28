# app HTMLコード実装規約

## 1. 目的・適用

appの手順8でHTMLファイルを新規作成または変更する場合に適用します。`.rules/dev/app/rules_app_8_code.md` と本規約を両方全文確認して下さい。

HTMLの構成、命名、読込順および共通枠は、本ファイルを手順8のHTML規約として適用します。

実装前に本規約の全章を確認し、実装後は同じフォルダの確認ファイルへ全項目の適用有無、判定および根拠を記録して下さい。

## 2. 優先順位と適用原則

- ユーザの明示指示、承認済み仕様・設計、本規約、app手順8の共通規約、周辺コードの順に優先して下さい。
- app手順8のcamelCase例、`//` の概念例およびC系header例と本規約が異なる場合は、本規約を優先して下さい。
- 対象ファイルまたは同一分類の周辺コードで規約と異なる書き方が複数の関数にわたり確立されている場合は、既存コードとの統一を優先して下さい。単発の記述だけで判断しないで下さい。
- 規約適用だけを目的として、変更対象外のコードを一括整形、改名または移動しないで下さい。
- 画面のレイアウト、id、class、共通枠、読込順およびレスポンシブは、HTML規約とCSS規約を詳細規約とします。汎用的なUI手順と重なる場合も、この詳細規約を優先して下さい。

## 3. 承認済み規約

## 1. 目的

- 仕様ID: APP-CODE-HTML-001

ユーザー向けHTML画面を修正・追加する際に、既存画面と同じ構成、命名、読込順、JavaScript/CSS連携を維持するための規約を定義する。

## 2. ファイル配置

|区分|配置|例|
|---|---|---|
|ホーム|`WebContent/USER/html/home/`|`home.html`|
|記事/地図/ユーザー検索|`WebContent/USER/html/contents/`|`articleList.html`, `articlePost.html`, `articleMap.html`|
|ログイン|`WebContent/USER/html/login/`|`login.html`, `regist.html`|
|マイページ|`WebContent/USER/html/myPage/`|`myPage.html`, `editUser.html`|
|チャット|`WebContent/USER/html/chat/`|`chatList.html`, `chatContentsList.html`|
|法務/その他|`WebContent/USER/html/law/`, `WebContent/USER/html/other/`|`privacyPolicy.html`, `mailUserAlert.html`|
|メイン配下の独立画面|`WebContent/USER/html/main/{対象}/`|ホーム画面と分離する画面を対象単位のフォルダへ配置する。|

同じ表示形式でDB取得値だけを切り替える画面は、対象フォルダに共通HTMLを1ファイルだけ配置する。URL識別子や表示種別ごとのHTMLファイルは作成せず、DB取得値を文脈別にエスケープして設定する。

## 3. 基本構造

|要素|規約|
|---|---|
|head|既存画面と同じくOGP、Google Analytics、広告、viewport、favicon、CSS、JSを配置する。|
|viewport|ユーザー向け画面は `width=device-width, initial-scale=1` を維持する。|
|CSS読込|`../../css/importCss.css` を先に読み込み、画面別CSSを後に読み込む。|
|JS読込|`../../js/importJs.js` を先に読み込み、画面別JSを後に読み込む。|
|body|共通レイアウトとして `idLayout` と `clsCommonLayout_4` を基本にする。|

## 4. 共通レイアウト

|ID/class|用途|規約|
|---|---|---|
|`idLayout`|ページ全体|`class="clsCommonLayout_4"` と組み合わせる。|
|`idLayoutHeader`|共通ヘッダー|JS共通処理がヘッダーを設定する前提を崩さない。|
|`idLayoutContents`|画面本文|画面固有DOMはこの中に配置する。|
|`idLayoutFotter`|共通フッター|既存名の `Fotter` を維持する。安易に `Footer` へ改名しない。|
|`idInitPosition`|初期位置ボタン|地図/一覧画面など既存利用箇所のCSS/JS依存を確認する。|

## 5. id/class命名

|対象|規約|例|
|---|---|---|
|id|`id` + 機能名/部品名。JSから参照されるため変更時はJSも確認する。|`idSearchCondition`, `idSnsArticleList`|
|class|`cls` + 部品名。共通部品は `clsCommon` を付ける。|`clsCommonArticleBase`, `clsCommonUserList`|
|hidden input|JSのパラメータ受け渡しに使う場合があるため、idと取得処理を対応させる。|`idUserId`|
|外部連携class|Google/Twitter/Ads等の指定classは外部仕様に従い、独自命名へ変更しない。|`g-recaptcha`, `g_id_signin`|

JSから取得・更新・イベント登録するDOMには、原則として `id...` を付与する。`class` は表示スタイルと共通部品識別に使用し、JSのDOM取得キーとして使用しない。

## 6. DOM構造とCSS依存

- 既存CSSは `nth-child` を多用するため、HTMLの子要素順を変更する場合は対象CSSを必ず確認する。
- JavaScriptが動的生成するDOMに対して共通CSSが適用される場合、HTML側の静的DOMだけで判断しない。
- `table`, `button`, `input`, `select`, `textarea`, `canvas`, `img`, `video` は、CSSとJSの両方で参照される可能性が高い。
- モーダル、SNS記事一覧、チャット、ユーザー一覧、地図は、id/class変更の影響が大きいため、修正前に使用箇所を検索する。
- JS操作対象のDOMを追加する場合は、CSS用classだけで済ませず、操作対象を示す `id...` を付与する。

## 7. head内リソース

|項目|規約|
|---|---|
|OGP|サイト正式名称を表す`og:site_name`は`レールビュー`とする。`og:title`は画面内容を表すページ固有タイトルとし、サイト名と同一文言へ固定しない。|
|外部script|Google Analytics、広告、Twitter widgets、Google Sign-In、reCAPTCHA等は既存画面の配置に合わせる。|
|favicon|`../../img/favicon/favicon.ico` を基本とする。|
|相対パス|階層に応じて既存画面と同じ `../../` 形式を維持する。|

## 8. HTMLファイルテンプレート

ユーザー向けHTMLを新規作成する場合は、対象画面に応じて次の先頭構成を基本にする。

```html
<!DOCTYPE html>
<html>
<head>
<!-- header -->
<!--
<head prefix="og: http://ogp.me/ns# fb: http://ogp.me/ns/fb# website: http://ogp.me/ns/website#">  <!-- top page   -->
<head prefix="og: http://ogp.me/ns# fb: http://ogp.me/ns/fb# article: http://ogp.me/ns/article#">  <!-- other page -->


  <!------------------------------------------------------------------------>
  <!-- google                                                             -->
  <!------------------------------------------------------------------------>

  <!-- google adsense -->
  <!-- Google tag (gtag.js) -->


  <!------------------------------------------------------------------------>
  <!-- meta                                                               -->
  <!------------------------------------------------------------------------>

  <!-- meta -->
  <meta charset="UTF-8">

  <!-- 説明 : 一般  -->
  <meta name="description" content="{ページ説明}">

  <!-- 説明 : SNS用 -->
  <meta property="og:title"       content="{ページタイトル} | レールビュー" />
  <meta property="og:description" content="{ページ説明}" />
  <meta property="og:type"        content="article" />
  <meta property="og:site_name"   content="レールビュー" />
  <meta property="og:locale"      content="ja_JP"  />

  <!-- ビューポート -->
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <!-- キーワード -->
  <meta name="keywords" content="鉄道, 写真, フォト, 投稿, 投稿サイト, レールビュー">

  <!-- IE互換性のモードを設定 -->
  <meta http-equiv="X-UA-Compatible" content="IE=edge">


  <!------------------------------------------------------------------------>
  <!-- title                                                              -->
  <!------------------------------------------------------------------------>

  <!-- title -->
  <title>{ページタイトル} | レールビュー</title>


  <!------------------------------------------------------------------------>
  <!-- favicon                                                            -->
  <!------------------------------------------------------------------------>

  <!-- favicon -->
  <link rel="icon" href="../../img/favicon/favicon.ico">


  <!------------------------------------------------------------------------>
  <!-- css                                                                -->
  <!------------------------------------------------------------------------>

  <!-- user : common -->
  <link rel="stylesheet" href="../../css/importCss.css">

  <!-- user : main -->
  <link rel="stylesheet" href="../../css/main/{分類}/{画面}.css">


  <!------------------------------------------------------------------------>
  <!-- javascript                                                         -->
  <!------------------------------------------------------------------------>

  <!-- import js -->
  <script type="text/javascript" src="../../js/importJs.js"></script>

  <!-- user : main -->
  <script type="text/javascript" src="../../js/main/{分類}/{画面}.js"></script>
</head>
```

HTML内に画面別styleを置く場合は、CSS読込とJavaScript読込の後に次の区切りで配置する。

```html
  <!------------------------------------------------------------------------>
  <!-- css -> embeded                                                     -->
  <!------------------------------------------------------------------------>
  <style type="text/css">
  <!--
  -->
  </style>
```

`body` は次の共通レイアウトを基本にする。`Fotter` は既存id名を維持する。

```html
<body>
<!------------------------------------------------------------------------------------>
<!-- layout                                                                         -->
<!------------------------------------------------------------------------------------>
<div id="idLayout" class="clsCommonLayout_4">
  <!---------------------------------------------------------------------------------->
  <!-- header                                                                       -->
  <!---------------------------------------------------------------------------------->
  <div id="idLayoutHeader"></div>

  <!---------------------------------------------------------------------------------->
  <!-- contents                                                                     -->
  <!---------------------------------------------------------------------------------->
  <div id="idLayoutContents">
  </div>

  <!---------------------------------------------------------------------------------->
  <!-- footer                                                                       -->
  <!---------------------------------------------------------------------------------->
  <div id="idLayoutFotter"></div>
</div>
</body>
</html>
```

## 8.1 フォーム/外部JS/埋め込みCSSの追加規約

|項目|規約|
|---|---|
|外部サービスJS|Googleログイン、reCAPTCHA、広告などの外部scriptは、既存画面の読み込み位置と `async` / `defer` を維持する。|
|画面JS|`../../js/importJs.js` を先に読み、画面固有JSを後に読む。|
|埋め込みCSS|画面固有の上書き、PC/スマートフォン切替、1画面専用のtable/input/textarea調整は既存の `<style type="text/css">` に置いてよい。|
|DOM順序|CSSが `>`、`:nth-child`、`button:eq(n)` で参照する画面では、div、table、button、textareaの順序を仕様に含め、順序だけを理由なく変えない。|
|フォーム|入力欄は `idMail`、`idPassword`、`idFileList` のように `id + 対象名` とし、JSの `checkFormat`、イベント登録、CSS参照と一致させる。|
|モーダル|共通モーダルは背景div、コンテンツdiv、main div、footer div、buttonの順序を維持する。|
|JS取得対象|JSで値取得、表示更新、イベント登録を行う要素は `id...` を持たせる。CSS classのみでJS対象を識別しない。|

## 9. 修正時の確認

|確認項目|内容|
|---|---|
|JS参照|変更したid/classを `WebContent/USER/js/**` で検索する。|
|CSS参照|変更したid/classを `WebContent/USER/css/**` で検索する。|
|レスポンシブ|`23_フロントエンド構成・レスポンシブ仕様.md` の確認viewportで表示する。|
|外部連携|reCAPTCHA、SNSログイン、広告、地図を含む画面は外部scriptの読み込み順を変えない。|

## 10. 参照した主なファイル

- `WebContent/USER/html/home/home.html`
- `WebContent/USER/html/contents/articleList.html`
- `WebContent/USER/html/contents/articlePost.html`
- `WebContent/USER/html/contents/articleMap.html`
- `WebContent/USER/html/login/login.html`
- `WebContent/USER/html/myPage/myPage.html`
- `WebContent/USER/html/chat/chatContentsList.html`
- `WebContent/USER/html/layout/layout.html`

## 4. カスタムルールの追加事項

## HTML共通形式の実装ルール

- ユーザー向けHTMLを新規作成または修正する前に、同じ階層かつ同じ役割の既存HTMLを複数確認する。
- HTMLの基本構成は、対象バージョンで承認済みのHTMLコーディング規約を正とする。既存HTMLと規約に差異がある場合は、差異をそのまま転記せず、実装前にユーザへ報告する。
- `<head>`内は、Google、meta、title、favicon、CSS、JavaScript、埋め込みCSSの順序と要否を確認する。
- `<head>`内の区切り線、区分コメント、短い処理コメント、インデントおよび空行は、承認済みHTMLテンプレートと類似HTMLの形式に合わせる。
- `<body>`内は、`layout`、`header`、`contents`、`footer`の共通フレームワークを維持し、各区分の区切り線とコメントを記載する。
- 画面固有DOMには、既存HTMLと同じ形式で機能名、id一覧、name一覧およびCSS一覧のうち該当する項目をコメントで記載する。
- 画面固有DOMの意味を持つ開始タグには、既存HTMLと同じく、タグの直前に対象と役割を示す短いコメントを記載する。閉じタグには同じコメントを重複して記載しない。
- CSSまたはJavaScriptの読込を追加する場合は、共通ファイルを先、画面固有ファイルを後にし、既存の相対パスと読込順を維持する。
- 画面別JavaScriptにイベント登録がある場合は、既存形式に従って`importJs(callback)`完了後に`init()`、`setEvent()`の順で呼び出し、イベント登録を`setEvent()`へ集約する。
- 画面別JavaScriptに複数の初期化関数がある場合は、`setEvent()`の後に大分類の`初期化`見出しを配置してから、`init()`および関連する初期化関数を配置する。
- HTML文字列を組み立てる場合、組み立て式の中で変換関数や取得関数を直接呼び出さず、先に意味の分かる変数へ結果を格納してから使用する。
- 同じ変換関数を複数の値へ適用する場合は、処理ブロック内で変換処理を連続してまとめ、その後にHTML文字列を組み立てる。
- 同一処理ブロックで変数宣言または関数呼び出しを複数行に並べる場合は、可読性を保つため`=`と終端の`);`の位置を揃える。
- 同一コメント配下には同じ意味の処理だけをまとめる。取得、変換、初期化、削除、追加、設定など処理の意味が変わる場合は、対象行または処理単位ごとにコメントを記載する。
- `if`文、`for`文などの制御ブロック外では、意味の異なる処理コメントとコードの組の間に空行を1行入れる。制御ブロック内では処理のまとまりを優先し、意味が異なる処理間にも不要な空行を入れない。

### HTML共通形式の確認チェックリスト

- [ ] 同じ階層・役割の既存HTMLを複数確認したか。
- [ ] 対象バージョンのHTMLコーディング規約を全文確認したか。
- [ ] `<head>`内のGoogle、meta、title、favicon、CSS、JavaScriptおよび埋め込みCSSの順序と要否を確認したか。
- [ ] 区切り線、区分コメント、短い処理コメント、インデントおよび空行が既存形式と一致しているか。
- [ ] `<body>`内が`layout`、`header`、`contents`、`footer`の共通フレームワークになっているか。
- [ ] 画面固有DOMの機能名、id一覧、name一覧およびCSS一覧のうち必要なコメントを記載したか。
- [ ] 画面固有DOMの意味を持つ開始タグ直前に、対象と役割を示す短いコメントを記載したか。
- [ ] CSSとJavaScriptの共通・画面固有ファイルの読込順および相対パスが正しいか。
- [ ] 画面固有イベントがある場合、`init()`の後に`setEvent()`を呼び、イベント登録を同関数へ集約したか。
- [ ] 複数の初期化関数がある場合、`setEvent()`の後に大分類の`初期化`見出しを配置したか。
- [ ] HTML文字列の組み立て式に変換関数や取得関数を直接記述せず、事前に変数へ格納したか。
- [ ] 同じ変換関数による複数値の変換を処理ブロック内で連続してまとめたか。
- [ ] 同一処理ブロックで複数行に並ぶ変数宣言または関数呼び出しの`=`と`);`の位置を揃えたか。
- [ ] 取得、変換、初期化、削除、追加、設定など処理の意味が変わる行または処理単位に、それぞれ適切なコメントを記載したか。
- [ ] `if`文、`for`文などの制御ブロック外にある意味の異なる処理間へ空行を1行入れ、制御ブロック内には不要な空行を入れていないか。
- [ ] HTMLのidとJavaScriptのDOM取得・イベント登録、CSSの参照先が一致しているか。
