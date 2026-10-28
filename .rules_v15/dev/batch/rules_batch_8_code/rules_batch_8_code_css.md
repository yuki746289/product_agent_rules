# batch CSSコード実装規約

## 1. 目的・適用

batchの手順8でCSSファイルを新規作成または変更する場合に適用します。`.rules/dev/batch/rules_batch_8_code.md` と本規約を両方全文確認して下さい。

CSSの命名、レスポンシブおよび共通クラスは、本ファイルを手順8のCSS規約として適用します。

実装前に本規約の全章を確認し、実装後は同じフォルダの確認ファイルへ全項目の適用有無、判定および根拠を記録して下さい。

## 2. 優先順位と適用原則

- ユーザの明示指示、承認済み仕様・設計、本規約、batch手順8の共通規約、周辺コードの順に優先して下さい。
- batch手順8のcamelCase例、`//` の概念例およびC系header例と本規約が異なる場合は、本規約を優先して下さい。
- 対象ファイルまたは同一分類の周辺コードで規約と異なる書き方が複数の関数にわたり確立されている場合は、既存コードとの統一を優先して下さい。単発の記述だけで判断しないで下さい。
- 規約適用だけを目的として、変更対象外のコードを一括整形、改名または移動しないで下さい。
- 画面のレイアウト、id、class、共通枠、読込順およびレスポンシブは、HTML規約とCSS規約を詳細規約とします。汎用的なUI手順と重なる場合も、この詳細規約を優先して下さい。

## 3. 承認済み規約

## 1. 目的

- 仕様ID: APP-CODE-CSS-001

CSS修正時に、既存の共通部品、画面別スタイル、レスポンシブ境界、JavaScript生成DOMとの関係を壊さないための規約を定義する。

## 2. ファイル配置と責務

|配置|責務|規約|
|---|---|---|
|`WebContent/USER/css/importCss.css`|CSS入口|jQuery UI、共通CSSの読込入口として扱う。|
|`WebContent/USER/css/common/clsCommon.css`|共通CSS本体|全画面影響があるため、変更時は広めに確認する。|
|`WebContent/USER/css/common/clsCommonImport.css`|共通class CSS読込|共通部品CSSの読込順を管理する。|
|`WebContent/USER/css/common/class/*.css`|共通部品|SNS、ユーザー一覧、チャット、モーダル、レイアウト等の部品単位。|
|`WebContent/USER/css/main/**/*.css`|画面別CSS|単一画面の表示差分やページ固有調整を記載する。|
|`WebContent/USER/plugin/**`|第三者CSS|原則変更しない。|

## 3. セレクタ命名

|対象|規約|例|
|---|---|---|
|共通class|`clsCommon` + 部品名。|`clsCommonArticleBase`, `clsCommonChatList`|
|画面固有class|`cls` + 画面/部品名。|`clsBoxLogin`, `clsBoxSearchCondition`|
|idセレクタ|JSとの結合が強いため、CSSだけの都合で新設・変更しない。|`#idOpenLayersMap`|
|子要素指定|原則として空白による子孫セレクタではなく、`>` による直接子セレクタを使用する。DOM順序依存を明記・確認する。|`.clsCommonUserList > tbody > tr > td:nth-child(1)`|
|既存例外|`table.clsCommonTableTab th` など、少数の既存空白セレクタは周辺形式を維持する。新規追加時は `>` を優先する。|`table.clsCommonTableTab th`|

## 4. 記述スタイル

|項目|規約|
|---|---|
|コメント|既存の大きな区切りコメント、部品階層コメントに合わせる。|
|プロパティ整列|周辺ファイルの `property : value;` の間隔に合わせる。|
|単位|既存に合わせて `px`, `%`, `rem`, `calc()` を使う。|
|色|既存色を優先し、同じ意味の色を不用意に増やさない。|
|短縮記法|周辺の書き方に合わせる。全体整形目的の置換はしない。|

## 5. CSSファイルテンプレート

CSSファイルの先頭は、既存共通CSSに合わせて次の大型コメントを基本にする。

```css



/***************************************************************************************************
*
*
*  {対象名}
*
*    更新日 : yyyy/MM/dd
*
*
***************************************************************************************************/
```

機能分類は次の形式で区切る。

```css

/****************************************************************************/
/*  {分類名}                                                                */
/****************************************************************************/

/* {セレクタ説明} */
.clsCommonXxx > tbody > tr > td {
  /* サイズ・位置 */
  position         : relative;
  left             : 0px;
  top              : 0px;
  width            : auto;
  height           : auto;
  margin           : 0px;
  /* 装飾 */
  padding          : 0px;
  border           : 0px solid #0f0;
}
```

|項目|規約|
|---|---|
|分類コメント|`/*  user list                                                               */` のように既存幅の区切りコメントを使用する。|
|セレクタコメント|`/* user list -> tbody -> tr -> td */` のようにDOM階層または対象部品を書く。|
|プロパティ分類|長い定義では `/* サイズ・位置 */`、`/* 装飾 */` を分ける。|
|1行定義|`nth-child` の幅、表示切替など短い定義は既存に合わせて1行で書いてよい。|
|レスポンシブ分類|`/*  レスポンシブ                                                                */` の分類を作り、`width <= 800px`、`width >= 800px` のコメントを置く。|

## 6. レスポンシブ規約

|項目|規約|
|---|---|
|主境界|現行の主なブレークポイントは `800px` とする。|
|境界確認|`799px`, `800px`, `801px` を確認し、`max-width : 800px` と `min-width : 800px` の競合を確認する。|
|基本確認幅|`320px`, `375px`, `414px`, `799px`, `800px`, `801px`, `1024px`, `1366px`。|
|横スクロール|スマートフォン幅で意図しない横スクロールを発生させない。|
|固定高|`100vh`, `calc()` を使う画面は、ヘッダー/フッター/入力欄の高さと重なりを確認する。|
|表|スマートフォンで非表示にする列は、thead/tbodyの同じ列番号を揃える。|

### 6.1 連続可変化の優先

- 固定幅による横超過は、最初に `max-width: 100%`、`min()`、`max()`、`clamp()`、`box-sizing: border-box`、flex/gridの折り返しで解消できるか確認する。
- PC表示と狭幅表示を同じDOM・同じレイアウトの連続変化で両立できる場合、`@media screen and (max-width : 800px)` を追加しない。
- PC用／スマートフォン用DOMの表示切替、列の表示切替、要素順序変更など、段階的な切替が必要な場合のみメディアクエリを使用する。
- メディアクエリを使用する場合は理由を記録し、799px、800px、801pxで境界を確認する。

## 7. 共通CSSと画面別CSSの使い分け

|状況|配置|
|---|---|
|1画面だけの崩れ|画面別CSSまたは対象HTMLの既存style近辺。|
|同じ部品を使う全画面で崩れている|`common/class/*.css`。|
|レイアウト全体の崩れ|`clsCommonLayout_*.css`。|
|SNS記事/コメント/返信表示の崩れ|`clsCommonArticleBase*.css`, `clsCommonArticleLike.css`, `clsCommonArticleFavorite.css`。|
|記事一覧の表示形式切替・詳細カード・写真一覧の崩れ|`clsCommonArticleView.css`, `clsCommonArticleDetail.css`, `clsCommonArticlePhoto.css`（いいね・お気に入りは`clsCommonArticleLike.css`, `clsCommonArticleFavorite.css`）。|
|チャット表示の崩れ|`clsCommonChat*.css`。|
|ユーザー一覧/ブックマーク一覧の崩れ|該当する `clsCommon*List.css`。|

## 8. JS生成DOMとの関係

- `articleList.js`, `ClassArticle.js`, `ClassArticleBase.js`, `ClassArticleDetail.js`, `ClassArticlePhoto.js`, `ClassChat.js`, `ClassUser.js`, `ClassSearchCondition.js` などがDOMを生成するため、CSS修正時は静的HTMLだけで完了判断しない。
- `nth-child` 指定を変更する場合、生成順序と対象DOMの子要素数を確認する。
- 新規セレクタは直接の親子関係が明確な範囲で `>` を連結し、意図しない深い階層へスタイルが波及しないようにする。
- `display : none`, `position : fixed`, `position : absolute`, `z-index`, `overflow`, `height : calc(...)` は表示崩れを起こしやすいため、修正後に実画面で確認する。

## 8.1 共通CSS/埋め込みCSSの追加規約

|項目|規約|
|---|---|
|共通CSS利用例|`common/class/*.css` では、必要に応じて先頭コメントにHTML利用例とJS利用例を記載する。モーダルなどDOM順序に依存する部品では特に記載する。|
|直接子セレクタ|共通部品では原則として `>` と `nth-child` を使い、対象DOMの階層と順序を固定する。|
|フッター表記|既存コードに `fotter` 表記がある場合は、対象id/class名として維持し、コメント上も既存ファイルと整合させる。|
|画面別CSSが空の場合|画面別CSSファイルが空でも、HTML内の `css -> embeded` に実体がある場合があるため、CSS仕様化時はHTMLの埋め込みCSSも確認する。|
|レスポンシブ切替|PC用/スマートフォン用の要素切替は、`max-width : 800px` と `min-width : 800px` の両方を確認し、同一境界で両方が効く箇所の表示優先を確認する。|

## 9. 禁止・注意事項

- 第三者CSSを直接変更しない。
- 共通CSSを、単一画面の都合だけで変更しない。
- 新規CSSで空白による広い子孫セレクタを安易に追加しない。
- 既存のid/class名をCSS都合だけでリネームしない。
- 文字サイズ、余白、幅を一括置換しない。
- `!important` は既存競合の原因調査後、他に方法がない場合のみ使用する。

## 10. 参照した主なファイル

- `WebContent/USER/css/importCss.css`
- `WebContent/USER/css/common/clsCommon.css`
- `WebContent/USER/css/common/clsCommonImport.css`
- `WebContent/USER/css/common/class/clsCommonLayout_4.css`
- `WebContent/USER/css/common/class/clsCommonArticleBase.css`
- `WebContent/USER/css/common/class/clsCommonUserList.css`
- `WebContent/USER/css/common/class/clsCommonChatContentsList.css`
- `WebContent/USER/css/main/contents/articlePost.css`
- `WebContent/USER/css/main/contents/articleList.css`
- `WebContent/USER/css/main/login/login.css`

## 4. カスタムルールの追加事項

## レスポンシブCSSの実装ルール

- レスポンシブ修正では、固定幅の要素へ直ちにメディアクエリを追加せず、最初に `max-width`、`min()`、`max()`、`clamp()`、`box-sizing`、flex/gridの折り返しによる連続可変化を検討する。
- 既存CSSを連続可変化するだけでPC表示と狭幅表示を両立できる場合、`@media screen and (max-width : 800px)` を追加しない。
- PC用／スマートフォン用DOMの表示切替、表の列表示切替、要素順序変更など、段階的なレイアウト切替が必要な場合のみメディアクエリを使用してよい。
- メディアクエリを追加または維持する場合は、連続可変化だけでは対応できない理由を仕様書または確認記録へ記載し、799px、800px、801pxで競合と表示優先を確認する。
- 既存メディアクエリは一括削除せず、対象DOMと表示切替の意図を確認してから、維持、置換、削除を判断する。
