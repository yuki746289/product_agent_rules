# batch JavaScriptコード実装規約

## 1. 目的・適用

batchの手順8でJavaScriptファイルを新規作成または変更する場合に適用します。`.rules/dev/batch/rules_batch_8_code.md` と本規約を両方全文確認して下さい。

JavaScriptの実装とClassCommonのURL管理は、本ファイルを手順8のJavaScript規約として適用します。HTML文字列の組み立てはHTML規約も併用して下さい。

実装前に本規約の全章を確認し、実装後は同じフォルダの確認ファイルへ全項目の適用有無、判定および根拠を記録して下さい。

## 2. 優先順位と適用原則

- ユーザの明示指示、承認済み仕様・設計、本規約、batch手順8の共通規約、周辺コードの順に優先して下さい。
- batch手順8のcamelCase例、`//` の概念例およびC系header例と本規約が異なる場合は、本規約を優先して下さい。
- 対象ファイルまたは同一分類の周辺コードで規約と異なる書き方が複数の関数にわたり確立されている場合は、既存コードとの統一を優先して下さい。単発の記述だけで判断しないで下さい。
- 規約適用だけを目的として、変更対象外のコードを一括整形、改名または移動しないで下さい。
- 画面のレイアウト、id、class、共通枠、読込順およびレスポンシブは、HTML規約とCSS規約を詳細規約とします。汎用的なUI手順と重なる場合も、この詳細規約を優先して下さい。

## 3. 承認済み規約

## 1. 目的

- 仕様ID: APP-CODE-JS-001

画面別JavaScript、共通クラス、APIラッパーを修正・追加する際に、既存の初期化、イベント登録、DOM更新、サーバ通信の書き方を維持するための規約を定義する。

## 2. ファイル配置

|配置|責務|
|---|---|
|`WebContent/USER/js/importJs.js`|共通JavaScript読込入口。|
|`WebContent/USER/js/main/**/*.js`|画面別の初期化、イベント、DOM更新、API呼び出し。|
|`WebContent/USER/js/myClass/server/ClassServer.js`|サーバ通信共通処理。|
|`WebContent/USER/js/myClass/server/ClassServerMethod.js`|Servlet API単位の呼び出しメソッド。|
|`WebContent/USER/js/myClass/lib/*.js`|Cookie、時刻、正規表現、サーバ通信補助などのライブラリ。|
|`WebContent/USER/js/myClass/main/*.js`|SNS、地図、検索条件、チャット、ユーザー、ファイル操作などの共通画面部品。|

## 3. 初期化パターン

|処理|規約|
|---|---|
|load|`window.addEventListener('load', (e) => { ... })` を基本にする。|
|共通JS読込|`importJs(callback)` のcallback内で `init()` と `setEvent()` を呼ぶ。|
|画面初期化|`init()` にクラス生成、ログイン状態取得、初期表示、初期データ取得をまとめる。|
|イベント登録|`setEvent()` にクリック、変更、スクロール等の登録をまとめる。|
|位置初期化|既存画面に合わせて `initPosition()` を使用する。|

## 3.1 画面JSのファイル構成

画面別JavaScriptは、既存ファイルに合わせて次の順序を基本にする。

|順序|構成|内容|
|---|---|---|
|1|ファイルヘッダー|ファイル名、更新日、メソッド一覧を書く。|
|2|メソッド一覧|`初期化`、業務機能、リンク移動などの大分類ごとに関数名を列挙する。|
|3|`load`|`window.addEventListener('load', ...)` と `importJs(callback)` を置く。|
|4|`class and variable`|クラスインスタンス、画面状態変数、表示済みID配列などを宣言する。|
|5|`function`|イベント設定、初期化、業務機能、リンク移動などを大分類ごとに配置する。|
|6|イベント設定|`setEvent()` を置き、クリック/変更/スクロール等の登録をまとめる。|
|7|初期化|`init()`, `initPosition()`, `initCombobox()` などを置く。|
|8|業務処理|一覧取得、一覧更新、登録、削除、表示更新などを置く。|
|9|リンク/パラメタ|画面遷移と遷移用パラメタ取得を置く。|

新規関数を追加する場合は、ファイル冒頭のメソッド一覧にも追加し、実定義直前コメントと説明文を揃える。

規約検証用の説明サンプルでは、実アプリの共通クラス生成や外部API連携を一部簡略化してよい。ただし、実装用コードでは既存画面と同じ `aryClassLib`、`ClassServer`、`ClassServerMethod`、`ClassCommon` 等の初期化構成を維持する。説明サンプルで簡略化した箇所は、サンプル確認書へ「サンプル都合」として明記する。

## 3.2 画面JSファイルテンプレート

画面別JavaScriptは、次のファイルヘッダーと章立てを基本にする。既存互換のため、現行コードにある章見出し `funtion` の表記は、既存画面へ追記する場合は周辺に合わせる。新規ファイルでは意味を補足したうえで同じ見出しを使用してよい。

```javascript
//////////////////////////////////////////////////////////////////////////////////
//
//  {画面}.js
//
//    更新日 : yyyy/MM/dd
//
//      メソッド一覧
//
//
//////////////////////////////////////////////////////////////////////////////////
//      初期化
//////////////////////////////////////////////////////////////////////////////////
//
//      初期化
//      init()
//
//      初期化 -> 位置
//      initPosition()
//
//
//////////////////////////////////////////////////////////////////////////////////
//      {機能分類}
//////////////////////////////////////////////////////////////////////////////////
//
//      {対象 + 処理内容}
//      selectXxx()
//
//////////////////////////////////////////////////////////////////////////////////
```

`load`、クラス/変数、関数定義の章は次の順序で配置する。

```javascript
  /////////////////////////////////////////////////////////////////////
  //
  //  load
  //
  /////////////////////////////////////////////////////////////////////

  //load
  window.addEventListener('load', (e) => {
    //初期化
    let callback = function(){
      //初期化
      init();
      //イベント設定
      setEvent();
    }
    //javascript読み込み
    importJs(callback);
  });


  /********************************************************************
  *
  *
  *  class and variable
  *
  *
  ********************************************************************/


  /********************************************************************
  *
  *
  *  funtion
  *
  *
  ********************************************************************/
```

共通クラスを使う画面では、`ClassLib*` を生成し、`aryClassLib` を作成してから `ClassServer`、`ClassServerMethod`、`ClassCommon`、画面部品クラスを生成する。

## 4. 変数と関数の書き方

|対象|規約|例|
|---|---|---|
|変数宣言|既存画面に合わせて `let` を基本とする。|`let classCommon;`|
|定数的な値|新規では `const` を使用してよいが、周辺の書き方と過剰に混在させない。|`const limit = 20;`|
|関数|画面別JSでは `let 関数名 = function(...) {}` を基本にする。|`let refreshChatList = function(){}`|
|クラス|共通部品は `class ClassXxx` とする。|`class ClassServer`|
|privateメソッド|既存クラスに合わせて `#methodName()` を使用する。|`#setParam()`|
|コールバック|`callback` または `callbackXxx` を使用する。|`callbackSnsAddArticleList`|
|リンク処理|`linkXxx` を使用する。|`linkMyPageMyPage`|
|取得/変換/更新|`select`, `trans`, `refresh`, `regist`, `update`, `delete`, `check`, `exist` を既存意味に合わせる。|`selectUserList`, `updateChatCheck`|
|DB列由来の値|DBカラムをそのまま扱う場合は `tm_`, `ts_` など既存のテーブル別接頭辞を維持する。|`tm_userId`, `ts_1_pathIcon`|
|動的DOM id|連番を使う場合は `idUserCanvas_` + `numRecordCurrent` のように対象idと現在行番号を連結する。|`idUserFollow_0`|

## 4.1 整列規約

現行JavaScriptは、値の対応関係を視覚的に追いやすくするため、連続する代入、Object、配列、引数、DB列キーの位置を揃える。

```javascript
    //class -> library
    classLibCookie     = new ClassLibCookie ();  //クッキーライブラリ
    classLibMath       = new ClassLibMath   ();  //数値処理
    classLibPattern    = new ClassLibPattern();  //正規表現ライブラリ

    //初期化
    let aryClassLib = {
        classLibCookie     : classLibCookie
      , classLibMath       : classLibMath
      , classLibPattern    : classLibPattern
    }
```

|対象|規約|
|---|---|
|代入|同じ意味の連続代入では、変数名、`=`、値、末尾コメントの位置を揃える。|
|Object|キー、`:`、値、末尾コメントの位置を揃える。2行目以降は `,` 始まりにする。|
|配列|複数行配列は、1行目を要素で開始し、2行目以降は `,` 始まりにする。Object要素内のキーも揃える。|
|複数行引数|関数定義・関数呼び出しの複数行引数は、1行目を引数で開始し、2行目以降は `,` 始まりにする。|
|DB列キー|`tm_`, `ts_1_`, `ts_2_` 等のDB列由来値は、変数名、`=`, `["..."]` 内のキー文字列幅を揃える。|
|jQueryイベント|イベント名、セレクタ、呼び出し関数名、末尾コメントを可能な範囲で揃える。|

```javascript
      //ユーザリスト
      let tm_userId             = user["tm_userId"            ];
      let tm_name               = user["tm_name"              ];
      let tm_datetimeLastAccess = user["tm_datetimeLastAccess"];

      //初期化
      let idUserCanvas   = "idUserCanvas_"   + numRecordCurrent;  //ユーザアイコン
      let idUserName     = "idUserName_"     + numRecordCurrent;  //ユーザ名
      let idUserProfile  = "idUserProfile_"  + numRecordCurrent;  //ユーザプロフィール
```

## 4.2 引数と戻り値

- 業務上の意味を持つ値は `let` 変数へ格納してから関数へ渡す。
- 呼び出し先の引数と意味が同じ場合は、定義側と同じ変数名を使用する。
- 関数結果、Objectプロパティ、配列要素、演算式、リテラルは、原則として変数へ格納してから `return` する。
- `return;` による値なしの早期終了、Objectのキーなどは例外とする。
- 対象ファイルまたは同一分類の周辺コードで規約と異なる書き方が確立されている場合は、既存コードとの統一を優先して周辺の書き方に合わせる。単独の関数だけで判断せず、前後を含む複数の関数で同じ形式が使われていることを確認する。
- 例えば `ClassServer.js` や `ClassServerMethod.js` に既存のAPIラッパーを追加する場合は、その分類内の既存ラッパーと引数・戻り値の書き方を統一し、同じファイル内へ異なる形式を混在させない。

```javascript
    //ファイルパス
    let pathFileImagePc     = pathDirArticle  + fileName;
    let pathFileImagePcBase = pathDirOriginal + fileName;

    //記事画像登録
    let blSuccess = classServerMethod.registArticleFileImagePc(
        pathFileImagePc
      , pathFileImagePcBase
    );

    return blSuccess;
```

## 4.3 関数定義コメント

関数定義直前のコメントは、既存ファイルに合わせて次の形式を基本にする。

```javascript
  /////////////////////////////////////////////////////////////////////
  //
  //  ユーザリスト取得
  //
  /////////////////////////////////////////////////////////////////////
  let selectUserList = function(){
  }
```

クラスメソッド、privateメソッドも同じ考え方で、対象と処理内容が分かる説明を付ける。

```javascript
  /////////////////////////////////////////////////////////////////////
  //
  //  sns -> comment リスト取得
  //
  /////////////////////////////////////////////////////////////////////
  #selectCommentList(objContents){
  }
```

|項目|規約|
|---|---|
|説明粒度|`初期化`, `記事登録`, `ユーザリスト取得`, `チャットリスト更新` のように対象と処理内容を書く。|
|階層表現|SNS、comment、replayなど親子関係がある処理は `sns -> comment リスト取得` のように書く。|
|メソッド一覧|ファイル冒頭のメソッド一覧コメントも、実定義直前コメントと同じ説明文に揃える。|
|避ける表現|関数名だけ、または `処理` だけのような説明不足のコメントを新規追加しない。|

## 5. 共通クラスインスタンス

|変数|用途|
|---|---|
|`classLibCookie`|Cookie操作。|
|`classLibMath`|数値処理。|
|`classLibPattern`|文字列、正規表現、本文HTML変換。|
|`classLibServer`|サーバ通信補助。|
|`classLibTime`|日時処理。|
|`classLibUtility`|汎用処理。|
|`classServer`|サーバ通信本体。|
|`classServerMethod`|APIメソッド呼び出し。|
|`classCommon`|ヘッダー、ログイン状態、共通画面処理。|
|`classArticle`, `classChat`, `classUser`, `classOpenLayers`|画面部品ごとの共通処理。|

既存画面と同じ初期化セットを使う場合は、`aryClassLib` によるまとめ渡しの形式を維持する。

## 5.1 共通クラスJSテンプレート

`WebContent/USER/js/myClass/**/*.js` の共通クラスは、画面JSとは異なり `class ClassXxx` を定義し、private変数、constructor、機能別公開メソッドを持つ。

```javascript
class ClassXxx {


  ////////////////////////////////////////////////////////////////////////////////
  //
  //  private 変数
  //
  ////////////////////////////////////////////////////////////////////////////////

  //クラス------------------------------------------------------------------------

  //ライブラリ
  #classLibCookie;      //クッキーライブラリ
  #classLibMath;        //数値処理

  //main
  #classServer;        //server
  #classServerMethod;  //server method


  ////////////////////////////////////////////////////////////////////////////////
  //
  // constructor
  //
  ////////////////////////////////////////////////////////////////////////////////
  constructor(aryClassLib, classServer, classServerMethod) {
    //ライブラリ
    this.#classLibCookie     = aryClassLib["classLibCookie"    ];  //クッキーライブラリ
    this.#classLibMath       = aryClassLib["classLibMath"      ];  //数値処理
    //main
    this.#classServer       = classServer;        //server
    this.#classServerMethod = classServerMethod;  //server method
  };
}
```

|項目|規約|
|---|---|
|private変数|`#` private fieldを使用し、グループごとに `//クラス--------`、`//変数--------` などで分ける。|
|constructor|`aryClassLib["..."]` のキー幅と代入位置を揃える。|
|公開メソッド|画面から呼ばれるメソッドは、引数、既定値、共通API呼び出し、戻り値の順にする。|
|補足説明|引数候補や表示順候補は、関数コメント内に複数行で記載する。|

## 5.2 クラス内コールバックの bind

共通クラスでイベント、非同期処理、ライブラリコールバック、WebSocket、OpenLayers等へローカル関数を渡す場合は、必要に応じて JavaScript の `Function.prototype.bind()` で `this` 相当の参照を保持する。

この `bind()` は jQueryイベント登録用の `.bind()` ではない。クラス内の `this` をコールバックへ渡すための関数束縛として扱う。

```javascript
    tempCallback = tempCallback.bind({thisTemp : this});

    $(document).on('click', '#idUpdateUser', function(){tempCallback();});
```

|項目|規約|
|---|---|
|用途|コールバック関数内からクラスインスタンスへ戻る必要がある場合に使用する。|
|書式|`tempXxx = tempXxx.bind({thisTemp : this});` を基本形にする。|
|参照名|束縛先オブジェクトのキーは既存コードに合わせて `thisTemp` とする。|
|変数名|束縛対象は `tempXxx` のローカル関数名を使用し、イベントや外部APIへ渡す前に束縛する。|
|配置|ローカル関数定義後、イベント登録や外部コールバック登録の直前にまとめる。|
|整列|複数行を連続して書く場合は、代入演算子、関数名、`.bind` の位置を既存コードに合わせて可能な範囲で揃える。|
|コメント|右側コメントには `//開く`、`//受信`、`//button -> 保存` のようにイベント元または処理内容を短く書く。|
|呼び出し側|束縛後の `tempXxx` を `$(document).on(...)`、OpenLayers `.on(...)`、WebSocketコールバック、ライブラリコールバック等へ渡す。|

複数コールバックを束縛する場合は、次のように関連する処理を固めて記述する。

```javascript
    tempWsOpen    = tempWsOpen   .bind({thisTemp : this});  //開く
    tempWsReceive = tempWsReceive.bind({thisTemp : this});  //受信
```

コールバック内部では、`this.thisTemp` からクラスインスタンスを参照する。

```javascript
    let tempCallback = function(){
      this.thisTemp.#classCommon.showMessageSuccess("更新しました。");
    }
```

## 5.3 ClassServerMethodテンプレート

`ClassServerMethod` のAPIラッパーは、request Object作成、`ClassServer` 呼び出し、`this.blSuccess` 判定、既定値またはレスポンスキー返却を基本にする。

```javascript
  ///////////////////////////////////////////////////////////////////
  //
  //  ユーザリスト取得
  //
  ///////////////////////////////////////////////////////////////////
  selectUserList(
      userIdLogin
    , keyword
    , orderBy
  ){
    //リクエスト
    let request = {
        userIdLogin : userIdLogin
      , keyword     : keyword
      , orderBy     : orderBy
    }
    //ユーザリスト取得
    let aryResult = this.#classServer.selectUserList(request);
    //レスポンス設定
    this.blSuccess = aryResult["blSuccess"];
    this.msg       = aryResult["msg"      ];
    //初期化
    let listUser = [];
    //http success
    if(this.blSuccess){listUser = aryResult["listUser"];}

    return listUser;
  }
```

## 5.4 共通クラスの親子（保持）

複数の共通クラスで同じ処理を使う場合は、共通処理を親クラスへ置き、個別処理を子クラスへ置く。子クラスは親クラス以外から呼び出さず、親クラスを通して呼び出す。このため継承（`extends`・`super`）は使わず、親クラスがconstructorで子クラスを生成して公開変数に保持し、子クラスはconstructorで受け取った親クラスのインスタンスを通して共通処理を呼び出す。

親クラス:

```javascript
class ClassXxx {


  ////////////////////////////////////////////////////////////////////////////////
  //
  //  public 変数
  //
  ////////////////////////////////////////////////////////////////////////////////

  // 子クラス（画面から classXxx.classXxxYyy.*** で呼び出す）
  classXxxYyy;  //子クラスの説明


  ////////////////////////////////////////////////////////////////////////////////
  //
  // constructor
  //
  ////////////////////////////////////////////////////////////////////////////////
  constructor(aryClassLib, classServer, classServerMethod) {

    //（ライブラリ・serverの保持）

    // 子クラス
    this.classXxxYyy = new ClassXxxYyy(aryClassLib, classServer, classServerMethod, this);  //子クラスの説明
  };
}
```

子クラス:

```javascript
class ClassXxxYyy {


  ////////////////////////////////////////////////////////////////////////////////
  //
  //  private 変数
  //
  ////////////////////////////////////////////////////////////////////////////////

  // 親クラス
  #classXxx;  //親クラスの説明


  ////////////////////////////////////////////////////////////////////////////////
  //
  // constructor
  //
  ////////////////////////////////////////////////////////////////////////////////
  constructor(aryClassLib, classServer, classServerMethod, classXxx) {

    // 親クラス
    this.#classXxx = classXxx;  //親クラスの説明
  };


  init(aryParam, callback1) {

    // 親クラス初期化
    this.#classXxx.init(aryParam, callback1);
  };
}
```

画面別JavaScript:

```javascript
let classXxx;  //親クラスの説明

function init() {
  classXxx = new ClassXxx(aryClassLib, classServer, classServerMethod);  //親クラスの説明
  classXxx.classXxxYyy.init(aryParam, callback1);
}
```

|項目|規約|
|---|---|
|配置|親クラスは`WebContent/USER/js/myClass/main/ClassXxx.js`、子クラスは親クラス名のフォルダ内の`WebContent/USER/js/myClass/main/ClassXxx/ClassXxxYyy.js`に置く。|
|クラス名|子クラス名は親クラス名で始める。親クラスが保持する公開変数名は、子クラス名の先頭を小文字にした名前（例: `classXxxYyy`）とする。|
|継承|子クラスに`extends`・`super`を使わない。親クラスのconstructorで子クラスを生成するため、継承すると子クラスの`super()`で親クラスのconstructorが再び動き、生成が繰り返される。|
|親クラスのconstructor|最後に子クラスを生成し、4番目の引数に`this`を渡して公開変数へ保持する。|
|子クラスのconstructor|4番目の引数で親クラスのインスタンスを受け取り、`#`付きの変数（例: `#classXxx`）へ保持する。|
|画面からの呼出し|画面別JavaScriptは親クラスだけを生成する。子クラスのメソッドは`classXxx.classXxxYyy.メソッド名(...)`、親クラスのメソッドは`classXxx.メソッド名(...)`で呼び出す。`new ClassXxxYyy`を書かない。生成したインスタンスを使わない画面では、宣言・生成を書かない（既存の画面ではコメントアウトする）。|
|子クラスからの呼出し|子クラスは親クラスのメソッドを`this.#classXxx.メソッド名(...)`で呼び出す。`this`が変わる関数の中では、既存の`thisTemp`等で退避したインスタンスを通して呼び出す。|
|読込順|`importJs.js`で親クラスを先、子クラスを後に並べる。子クラスをHTMLの`<script>`タグで直接読み込まない（読込を`importJs.js`へまとめるため）。|
|`#`の使い分け|親クラスの中だけで使う変数・メソッドは`#`を付ける。子クラスから使うメソッドは`#`を付けない（`#`付きのメソッドはクラスの外から呼び出せないため）。|
|子クラス用メソッド|`#`を付けない親クラスのメソッドのうち、画面から呼び出さないものは、ファイル先頭のメソッド一覧と関数定義コメントに「子クラス用（画面から呼ばない）」と記載する。画面別JavaScriptから呼び出さない。|
|親クラスの変数|親クラスの`#`変数は子クラスから参照できない。子クラスで必要な値は、子クラス自身で保持するか、親クラスのメソッドの引数・戻り値で受け渡す。|
|イベント登録|固定のidに対する`$(document).on(...)`は子クラスで行い、親クラスのconstructorでは行わない。複数の子クラスを持つ場合に、同じイベントが二重に登録されないようにするため。|
|init|子クラスの`init`は、親クラスの初期化に必要な引数を`this.#classXxx.init(...)`で親クラスへ渡す。|
|ファイル先頭のコメント|親クラスには子クラス名と保持の関係（例: `子クラス : ClassXxxYyy（公開変数classXxxYyyで保持する。継承はしない）`）、子クラスには親クラス名と保持の関係（例: `親クラス : ClassXxx（classXxx.classXxxYyyとして生成・保持される。継承はしない）`）を記載する。|

## 6. DOM操作

|項目|規約|
|---|---|
|jQuery|既存コードに合わせて `$()`、`.val()`, `.html()`, `.append()`, `.children()`, `.eq()` を使用してよい。|
|id参照|HTMLの `id...` と完全一致させる。DOM取得・更新・イベント登録は原則としてid名で行う。|
|class参照|CSS class名は表示スタイル用であり、JSのDOM取得キーとして使用しない。|
|動的id|連番やID連結は既存規則に合わせる。| 
|HTML挿入|ユーザー入力由来のHTMLはセキュリティ仕様を確認し、再実装時はエスケープ/サニタイズを必須にする。|
|CSS依存|DOM生成順序を変える場合は `nth-child` CSSを確認する。|

DOM取得の基本形は次の通りとする。

```javascript
    //値取得
    let mail = $("#idMail").val();

    //canvas取得
    let objCanvas = document.getElementById(idUserCanvas);
```

`$('.className')`、`$(".className")`、`document.querySelector('.className')`、`getElementsByClassName` は、確認できた現行コードの範囲ではDOM取得の基本形として使用していない。例外的にclass取得が必要な場合は、既存コードとの整合、対象範囲、CSSへの影響を設計書に記載する。

## 7. サーバ通信

|項目|規約|
|---|---|
|API呼び出し|画面JSから直接Ajax詳細を書かず、原則 `ClassServerMethod` または既存共通クラス経由で呼び出す。|
|命名統一|`ClassServerMethod.js` → `ClassServer.js` → 呼び出しAPI → 呼び出しAPIのservlet の順で使用される関数名とAPI名は全て同一名とする。servletクラス名には接頭語 `Sv` を付ける。Operation層（`OperationCheck` / `OperationTable` 等）のメソッド名は原則として同一名とする（必須ではない）。|
|認証ヘッダー|ログイン必須APIは `ClassCommon` が設定する `x-userid` / `x-tokenlogin` の前提を崩さない。|
|レスポンス|既存の `blSuccess`, `mapResponse`, `ary...`, `bl` などの戻り値キーを維持する。|
|同期/非同期|既存呼び出しが同期的に戻り値を扱う箇所は、非同期化で処理順を変えない。|
|エラー表示|既存の `showMessageSuccess`, `showMessageFailure`, `alert`, `confirm` の使い分けに合わせる。|

## 8. イベント登録

- `click`, `change`, `scroll`, OpenLayersの `.on(...)` は `setEvent()` または該当初期化関数にまとめる。
- 動的DOMへイベントを付与する場合も、原則として `#id...` を指定した委譲イベントまたは生成直後登録を選ぶ。
- 二重登録を避けるため、リスト再描画時にイベント登録処理を繰り返す場合は既存パターンを確認する。
- jQueryイベント登録では `.bind()` / `.unbind()` を使用せず、`$(document).on(...)` / `$(document).off(...)` を使用する。
- JavaScript関数の `tempXxx.bind({thisTemp : this})` は、クラス内コールバックの `this` 保持用途として扱い、jQueryイベント登録とは区別する。

## 8.1 関数内の標準構成

関数内は、既存コードに合わせて次のような順序で組み立てる。

|順序|ブロック|内容|
|---|---|---|
|1|`初期化`|ローカル変数、配列、DOM id、初期値を用意する。|
|2|`値取得`|DOM、共通クラス、Map、配列、URLパラメタから値を取得する。|
|3|`判定`|ログイン状態、空値、上限、表示条件を判定する。|
|4|`処理`|API呼び出し、DOM生成、変換、登録、更新、削除を行う。|
|5|`値設定`|DOM、配列、状態変数、hidden inputへ結果を反映する。|
|6|`イベント設定`|動的に生成したDOMへ必要なイベントを設定する。|
|7|`戻り値`|必要に応じて値を返す。|

すべての関数で全ブロックを置く必要はない。処理が短い場合は短いコメントだけでよい。

## 8.2 if / for / try のコメント

```javascript
    //チャットリストループ
    for(let chat of aryChat){
      /////////////////////////////////////////////////////////////////////
      //  値取得
      /////////////////////////////////////////////////////////////////////

      //チャット一覧
      let tm_chatId = chat["tm_chatId"];
    }
```

|構文|規約|
|---|---|
|`if`|条件の意味を直前に書く。例: `//ログイン済み`, `//表示対象あり`, `//スマートフォン表示`。|
|`else if` / `else`|条件が短い場合は既存形式に合わせる。複雑な場合は分岐前に `判定` ブロックを置く。|
|`for` / `for...of`|`チャットリストループ`、`表示順ループ` のように走査対象を書く。|
|`try` / `catch`|非同期処理や外部API処理で使う場合は、対象処理とエラー処理の意味が分かるコメントを置く。|
|コールバック|`callback` の中では、実行する処理順に `初期化`、`処理`、`値設定` の短いコメントを置く。|

`if` / `for` の中では、原則として空行を入れない。ブロックコメント直後に短いコメントと処理を続け、処理単位を詰めて書く。

```javascript
    //表示順ループ
    for(let typeOrder of aryTypeOrder){
      //値取得
      let value = typeOrder["value"];
      let text  = typeOrder["text" ];
      //値設定
      $('#idOrderBy').append($('<option>').val(value).text(text));
    }
```

短いガード句、単純な2択処理、ログイン未済時の即時returnは1行形式を使用してよい。

```javascript
    //リンク移動 -> login -> login
    if(!blLogin){classCommon.linkLoginLogin();return;}

    //
         if(obj.prop("nodeName") == "TD"    ){objTd = obj;}
    else if(obj.prop("nodeName") == "SPAN"  ){objTd = obj.parent();}
```

## 8.3 短い処理コメントの語彙

|コメント|用途|
|---|---|
|`//初期化`|変数や配列を初期化する。|
|`//値取得`|DOM、Map、配列、API結果から値を取得する。|
|`//値設定`|DOM、hidden input、Map、配列へ値を設定する。|
|`//登録`|DOM行、イベント、Map、配列へ登録する。|
|`//更新`|表示、状態、件数を更新する。|
|`//削除`|DOM、配列、状態を削除または初期化する。|
|`//リンク移動 -> ...`|画面遷移を行う。|
|`//パラメタ取得 : ...`|遷移先や処理対象のパラメタを取得する。|

## 8.4 入力フォーム/アップロード/OpenLayersの追加テンプレート

### 8.4.1 入力フォーム

|項目|規約|
|---|---|
|入力値取得|関数冒頭で `$("#idXxx").val()`、checkboxは `.prop("checked")` で取得する。|
|形式チェック|`classLibUtility.checkFormat(id, idMsg, {blank : false, min : n, max : n}, "表示名")` を使用し、失敗時は例外対象である値なしの1行 `return;` を許容する。|
|非同期変換|パスワードハッシュなどは `async function` と `await` を使用し、コメントに非同期関数であることを残す。|
|結果分解|サーバ戻り値は `aryResult["blExist"]` のようにキー幅を揃えて分解する。|

### 8.4.2 ファイルアップロード

|項目|規約|
|---|---|
|ファイルループ|ファイル配列は `for(let file of aryFile)` を使用する。`for...in` はファイル配列では使用しない。|
|ファイル情報|`fileName`, `fileSize`, `extensionType`, `extension` を分けて取得し、コメントで単位や用途を書く。|
|登録用配列|DB登録用、記事登録用、アップロード用を別配列に分け、Objectキーを縦揃えする。|
|JSON化|APIへ渡す配列は `JSON.stringify` し、コメントで対象テーブルや用途を書く。|
|Promise|複数手順の非同期処理は `aryMyFunc` に関数を積み、`classLibUtility.execPromise` で順次実行する。|
|完了コールバック|アップロード完了、クレジット追加、サムネイル登録、メッセージ表示の順序をコメントで明示する。|

### 8.4.3 OpenLayers

|項目|規約|
|---|---|
|初期化|`idOlMap`, `pathDirImg`, `longitude`, `latitude`, `zoom` を先に定義する。|
|地図範囲|ローカルストレージの地図範囲を使う場合、中心経度、中心緯度、拡大率を取得して初期値へ反映する。|
|スタイル|`aryOlStyleIcon` はObjectで作成し、`anchor`, `opacity`, `src`, `scale` を既存順で並べる。|
|コールバック|`callbackOlMapOnMoveEnd`, `callbackOlMapOnPointermove`, `callbackOlMapOnClick`, `callbackOlFeatureOnClick` を分類コメント付きで定義する。|
|外部コールバック|Google/reCAPTCHAなど外部JSから直接呼ばれる関数は、既存と同じく `var` を使用する場合がある。|

## 9. 参照した主なファイル

- `WebContent/USER/js/importJs.js`
- `WebContent/USER/js/main/contents/articleList.js`
- `WebContent/USER/js/main/contents/articlePost.js`
- `WebContent/USER/js/main/login/login.js`
- `WebContent/USER/js/main/myPage/myPage.js`
- `WebContent/USER/js/main/chat/chatList.js`
- `WebContent/USER/js/myClass/server/ClassServer.js`
- `WebContent/USER/js/myClass/server/ClassServerMethod.js`
- `WebContent/USER/js/myClass/main/ClassArticle.js`
- `WebContent/USER/js/myClass/main/ClassArticle/ClassArticleBase.js`
- `WebContent/USER/js/myClass/main/ClassArticle/ClassArticleDetail.js`
- `WebContent/USER/js/myClass/main/ClassArticle/ClassArticlePhoto.js`
- `WebContent/USER/js/myClass/main/ClassOpenLayers.js`

## 4. カスタムルールの追加事項

## ClassCommon URL管理ルール

- ユーザー画面のURLは`ClassCommon.js`で一括管理し、画面別JavaScriptで画面ファイルのパスを直接組み立てない。
- URL取得関数名は原則として`selectUrl{フォルダ名}{HTMLファイル名}`、リンク移動関数名は`link{フォルダ名}{HTMLファイル名}`とする。
- `typePage`は原則として`{フォルダ名}_{HTMLファイル名}`とし、URL取得関数名およびリンク移動関数名と同じ画面を示す名称にする。
- URL取得関数は`typePage`と`aryQuery`を定義し、共通関数`#selectUrl(typePage, aryQuery)`を呼び出してURLを取得する。
- リンク移動関数は対応する`selectUrl...()`を呼び出してURLを取得した後、`window.location.href`へ設定する。
- `TypeLike`、`TypeTag`など同一HTML画面のクエリ条件だけが異なる関数は、関数名へ条件を示す接尾辞を付け、共通の`typePage`を使用してよい。

### ClassCommon URL管理チェックリスト

- [ ] URL取得関数名が`selectUrl{フォルダ名}{HTMLファイル名}`の形式になっているか。
- [ ] リンク移動関数名が`link{フォルダ名}{HTMLファイル名}`の形式になっているか。
- [ ] `typePage`が`{フォルダ名}_{HTMLファイル名}`の形式で、関数名と同じ画面を示しているか。
- [ ] URL取得関数が`typePage`と`aryQuery`を使用して`#selectUrl()`を呼び出しているか。
- [ ] `#selectUrl()`に対象`typePage`とHTMLファイルパスの対応を登録したか。
- [ ] リンク移動関数が対応するURL取得関数を呼び出してから`window.location.href`を設定しているか。
- [ ] 関数名に`Type...`を付けた場合、その値がHTML画面ではなくクエリ条件を示す例外であることを確認したか。
