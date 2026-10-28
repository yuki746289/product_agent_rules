# app Javaコード実装規約

## 1. 目的・適用

appの手順8でJavaファイルを新規作成または変更する場合に適用します。`.rules/dev/app/rules_app_8_code.md` と本規約を両方全文確認して下さい。

Javaの実装、Servlet、コメントおよび整列は、本ファイルを手順8のJava規約として適用します。SQL文字列の書き方はSQL規約も併用して下さい。

実装前に本規約の全章を確認し、実装後は同じフォルダの確認ファイルへ全項目の適用有無、判定および根拠を記録して下さい。

## 2. 優先順位と適用原則

- ユーザの明示指示、承認済み仕様・設計、本規約、app手順8の共通規約、周辺コードの順に優先して下さい。
- app手順8のcamelCase例、`//` の概念例およびC系header例と本規約が異なる場合は、本規約を優先して下さい。
- 対象ファイルまたは同一分類の周辺コードで規約と異なる書き方が複数の関数にわたり確立されている場合は、既存コードとの統一を優先して下さい。単発の記述だけで判断しないで下さい。
- 規約適用だけを目的として、変更対象外のコードを一括整形、改名または移動しないで下さい。
- 画面のレイアウト、id、class、共通枠、読込順およびレスポンシブは、HTML規約とCSS規約を詳細規約とします。汎用的なUI手順と重なる場合も、この詳細規約を優先して下さい。

## 3. 承認済み規約

## 1. 目的

- 仕様ID: APP-CODE-JAVA-001

Servlet、Operation、OperationTable、library、WebSocketを修正・追加する際に、既存の責務分離、命名、例外処理、レスポンス形式、DB操作形式を維持するための規約を定義する。

## 2. ファイル配置と責務

|配置|責務|
|---|---|
|`src/servlet/restApi/**/Sv*.java`|HTTP API入口。リクエスト取得、認証/入力チェック呼び出し、Operation実行、JSON出力を行う。|
|`src/servlet/page/**/Sv*.java`|HTML/XMLページ入口。GETパス取得、公開可否確認、Operation実行、HTMLまたはXML出力を行う。|
|`src/servlet/webSocket/**/*.java`|WebSocket入口。|
|`src/operation/OperationServlet.java`|Servlet共通初期化、設定読込、Library/Operation生成、終了処理。|
|`src/operation/OperationExport.java`|レスポンス、ファイル、エラー出力。|
|`src/operation/OperationWebSocket.java`|WebSocket共通処理。|
|`src/operation/operationServlet/Operation*.java`|ログイン、セッション、チェック、ファイル、SNS、管理などの業務処理。|
|`src/operation/operationServlet/OperationTable.java`|テーブル操作クラスの集約。|
|`src/operation/operationServlet/operationTable/OperationTable*Select.java`|DB参照系処理。|
|`src/operation/operationServlet/operationTable/OperationTable*Update.java`|DB登録、更新、削除、件数更新、存在確認などの更新系処理。|
|`src/library/**/*.java`|DB、ファイル、HTTP、画像、メール、正規表現、乱数、時刻などの汎用処理。|

## 3. クラス命名

|種類|規約|例|
|---|---|---|
|Servlet|`Sv` + 操作 + 対象。|`SvSelectUserList`, `SvRegistArticle`|
|Operation|`Operation` + 対象。|`OperationCheck`, `OperationFile`|
|OperationTable|`OperationTable` + 対象 + `Select`/`Update`。|`OperationTableArticleSelect`, `OperationTableUserUpdate`|
|Library|`Lib` + 対象。|`LibDatabase`, `LibFile`, `LibImage`|
|WebSocket|`Ws` + 対象、または共通処理は `OperationWebSocket`。|`WsChat`|

## 4. Servlet実装パターン

|順序|規約|
|---|---|
|1|`OperationExport operationExport = new OperationExport("クラス名", req, res);` を作成する。|
|2|`try` ブロック内で `OperationServlet operationServlet = new OperationServlet(req, res);` を作成する。|
|3|`req.getParameter(...)` からリクエスト値を取得し、必要に応じて `Long.parseLong`, `Double.parseDouble`, `Boolean.parseBoolean` を使用する。|
|4|`operationServlet.operationCheck.xxx(...)` でログイン、権限、存在、上限、連続操作などを確認する。|
|5|`blSuccess` と `mapResponse` を取得する。|
|6|`if(blSuccess)` の中でOperationまたはOperationTableを呼び出す。|
|7|戻り値を `mapResponse.put("キー", 値)` で設定する。|
|8|`operationServlet.finishOperation();` を呼ぶ。|
|9|`operationExport.export(mapResponse);` でJSON出力する。|
|10|`catch(Exception e)` では `operationExport.error(e);` を呼ぶ。|

既存APIのレスポンスキーを変更する場合は、`ClassServerMethod.js` と画面JSの参照箇所を必ず確認する。

HTML/XMLページ用Servletも`Sv`＋操作＋対象で命名し、参照系は`SvSelect...`を使用する。論理URLが`/station/{id}`等の業務URLであっても、Javaクラス名から操作を省略しない。ページ用ServletはJSON APIではないため`src/servlet/restApi`へ配置せず、`src/servlet/page/{分類}/{対象}`へ配置する。

## 4.1 Servletファイル構成

Servletは、既存ファイルに合わせて次の構成を基本にする。

|順序|構成|内容|
|---|---|---|
|1|`package`|API分類に合わせたパッケージ。|
|2|`import`|Java標準、Servlet、operation等を既存順に配置する。|
|3|クラスヘッダー|APIの対象処理名を書く。|
|4|`@WebServlet`|URLパターンを定義する。|
|5|class定義|`Sv... extends HttpServlet`。|
|6|`post` / `get` コメント|HTTPメソッド単位の区切りコメントを置く。|
|7|`doPost` / `doGet`|リクエスト処理本体。|
|8|`try` ブロック|初期化、パラメタ取得、処理、終了を入れる。|
|9|`error` ブロック|`catch(Exception e)` で `operationExport.error(e)` を呼ぶ。|

## 4.2 Servletメソッド内構成

|順序|ブロック|内容|
|---|---|---|
|1|エクスポート|`OperationExport` を作成する。|
|2|`try`|主処理を開始する。|
|3|`初期化`|`OperationServlet` を作成する。|
|4|`パラメタ取得`|`req.getParameter()` で値を取得し、型変換する。|
|5|`処理`|`operationCheck`、`blSuccess` 取得、Operation呼び出しを行う。|
|6|`処理終了`|`finishOperation()` と `operationExport.export(mapResponse)` を行う。|
|7|`error`|例外時に `operationExport.error(e)` を行う。|

短いAPIでも、この大枠は崩さない。

## 4.3 Servletファイルテンプレート

Servletは次の形式を基本にする。

```java
package servlet.restApi.{分類}.{対象};

import java.util.List;
import java.util.Map;

import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import operation.OperationExport;
import operation.OperationServlet;


///////////////////////////////////////////////////////////////////
//
//  {対象 + 処理内容}
//
///////////////////////////////////////////////////////////////////
@WebServlet(urlPatterns={"/{apiName}"})
public class Sv{処理}{対象} extends HttpServlet {


    ///////////////////////////////////////////////////////////////////
    //
    //  post
    //
    ///////////////////////////////////////////////////////////////////
    @Override
    public void doPost (HttpServletRequest req, HttpServletResponse res) {

        //エクスポート
        OperationExport operationExport = new OperationExport("Sv{処理}{対象}", req, res);


        ///////////////////////////////////////////////////////////////////
        //
        //  try
        //
        ///////////////////////////////////////////////////////////////////
        try {


            ///////////////////////////////////////////////////////////////
            //  初期化
            ///////////////////////////////////////////////////////////////

            //操作クラス
            OperationServlet operationServlet = new OperationServlet(req, res);


            ///////////////////////////////////////////////////////////////
            //  パラメタ取得
            ///////////////////////////////////////////////////////////////

            //パラメタ取得
            String userIdLogin = req.getParameter("userIdLogin");


            ///////////////////////////////////////////////////////////////
            //  処理
            ///////////////////////////////////////////////////////////////

            //リクエスト確認 -> {対象 + 処理内容}
            operationServlet.operationCheck.selectXxx();

            //値取得
            boolean             blSuccess   = operationServlet.operationCheck.blSuccess;
            Map<String, Object> mapResponse = operationServlet.operationCheck.mapResponse;


            ///////////////////////////////////////////////////////////////
            //  処理終了
            ///////////////////////////////////////////////////////////////

            //操作終了
            operationServlet.finishOperation();

            //エクスポート
            operationExport.export(mapResponse);


        ///////////////////////////////////////////////////////////////////
        //
        //  error
        //
        ///////////////////////////////////////////////////////////////////
        } catch(Exception e) {
            //エラー
            operationExport.error(e);
        }
    }
}
```

|項目|規約|
|---|---|
|import|Java標準、Servlet、operationの順に空行で分ける。使用しないimportは追加しない。|
|`@WebServlet`|URLは既存APIと同じく小文字開始の操作名を基本にする。例: `/selectUserList`。|
|`doPost`|既存Servletに合わせて `public void doPost (HttpServletRequest req, HttpServletResponse res)` を基本にする。|
|パラメタ整列|複数の `req.getParameter` は型、変数名、代入、引数の縦位置を可能な範囲で揃える。|
|レスポンスキー|既存クライアントが参照する `listUser`, `blSuccess`, `mapResponse` などを変更しない。|

参照系Servletでは、`operationTable.xxxSelect.selectXxx(...)` の戻り値を `List<Map<String, String>> listXxx` で受け、`mapResponse.put("listXxx", listXxx)` へ登録する。

ページ用Servletでは`doGet`を使用し、URL検証、公開可否判定、DB参照、HTML/XML生成をOperationへ委譲する。HTMLは`OperationExport.exportHtml(String html)`、XMLは対象仕様で定めた出力関数から応答し、Servletで`PrintWriter`処理を重複実装しない。HTML応答は`text/html;charset=UTF-8`を設定し、初期HTMLへtitle、description、canonical、robots、本文を含める。

登録系Servletでは、登録順序が意味を持つ場合がある。順序変更禁止や連続投稿対策などの意図は、`if(blSuccess)` の直前に複数行コメントで残す。

```java
            //処理
            //  順番変えない
            //  最初に日時更新を行う
            //  連続投稿対策
            if(blSuccess) {
                //記事日時更新
                operationServlet.operationTable.articleUpdate.updateArticleDatetime(
                      userId
                    , datetimePost
                );
                //記事登録
                String articleId = operationServlet.operationTable.articleUpdate.registArticle(
                      userId
                    , datetimePost
                    , articleContents
                );
                //登録
                mapResponse.put("articleId", articleId);
            }
```

パラメタ取得では、型、変数名、`=`、型変換関数、`req.getParameter`、キー文字列の幅を揃える。

```java
            String userId              =                    req.getParameter("userId"             );
            String articleContents     =                    req.getParameter("articleContents"    );
            double latitude            = Double.parseDouble(req.getParameter("latitude"           ));
            long   numLikeArticle      =     Long.parseLong(req.getParameter("numLikeArticle"     ));
```

## 5. メソッド命名

|操作|規約|例|
|---|---|---|
|取得|`selectXxx`|`selectArticleListForSns`|
|登録|`registXxx`|`registArticleBookmark`|
|更新|`updateXxx`|`updateUserIcon`|
|削除|`deleteXxx`|`deleteUserFollow`|
|存在確認|`existXxx`|`existUser`|
|入力/状態確認|`checkXxx`|`checkArticleBookmarkLimit`|
|変換|`transXxx`|`transQueryString`|
|初期化|`initXxx`|`initOperation`|
|終了|`finishXxx`|`finishOperation`|

API呼び出し経路の命名は次を基本とする。

|層|規約|
|---|---|
|`ClassServerMethod` → `ClassServer` → API → Servlet|関数名とAPI名は全て同一名とする。Servletクラス名には接頭語 `Sv` を付ける。|
|Operation層（`OperationCheck` / `OperationTable` 等）|メソッド名は原則として同一名とする（必須ではない）。|

## 5.1 引数と戻り値

- 業務上の意味を持つ値は、型と意味が分かる変数へ格納してからメソッドへ渡す。
- 呼び出し先の引数と意味が同じ場合は、定義側と同じ変数名を使用する。
- メソッド結果、フィールド・配列要素、演算式、リテラルは、原則として変数へ格納してから `return` する。
- 薄い委譲メソッドも、新規・変更コードでは戻り値を変数へ格納してから返す。
- 値を返さない早期終了の `return;` など、共通規約で定義した例外は直接記述してよい。

```java
        //画像サイズ
        int sizeMax = 1200;

        //記事画像登録
        boolean blSuccess = registArticleFileImagePc(
              pathFileImagePc
            , pathFileImagePcBase
            , fileNameImagePc
            , extensionTypeImagePc
            , sizeMax
        );

        return blSuccess;
```

## 6. 変数・型

|対象|規約|例|
|---|---|---|
|レスポンス|`Map<String, Object> mapResponse` を基本にする。|`mapResponse.put("bl", bl)`|
|DB行|`Map<String, String>` を基本にする。|`Map<String, String> mapUser`|
|DB一覧|`List<Map<String, String>>` を基本にする。|`listArticle`|
|JSON変換|Jackson `ObjectMapper` と `TypeReference` を既存形式で使用する。|`new TypeReference<Map<String, String>>() {}`|
|真偽値|`boolean bl...` を使用する。|`blSuccess`, `blRead`|
|件数・数量|件数、総数、現在数、表示数など、数値であること自体が意味になる場合は `num...` を使用する。|`numArticle`, `numLikeArticle`|
|数値パラメータ|取得数、開始位置、座標、日付、IDなど意味が明確な名前は `num` を付けない。|`limit`, `offset`, `latitude`, `longitude`, `dateTaken`, `articleId`|
|パス|`path...` を使用する。|`pathRoot`, `pathFileThumbnail`|

## 7. 例外処理

|場所|規約|
|---|---|
|Servlet|広く `Exception` をcatchし、`OperationExport.error(e)` へ渡す既存形式を維持する。|
|Operation/Library|呼び出し元へ `throws` で伝播する既存形式を維持する。|
|ファイル/DB/HTTP|例外握りつぶしを避け、呼び出し元でレスポンス化する。|
|標準出力|新規の `System.out.println` や `printStackTrace()` は原則追加しない。必要な場合はログ仕様を確認する。|

## 8. DB操作

- DB処理は `Library`、`LibDatabase`、`LibSubPreparedStatement`、`OperationTable*` の既存責務に合わせる。
- SQL文字列、パラメータ、戻り値のMapキーは既存APIレスポンスと画面JSの参照名を確認する。
- 登録、更新、削除、件数更新、関連データ削除は副作用を伴うため、関数カタログの副作用と状態変更対象を確認する。
- 実DBとの差分がある場合は、`11_DB定義・ER図.md` とローカルDB確認結果を優先して判断する。

## 8.1 Operation / Library の関数内構成

OperationTableやLibraryの関数内は、既存コードに合わせて次の順序を基本にする。

|順序|ブロック|内容|
|---|---|---|
|1|`初期化`|戻り値、Map、List、SQL、既定値を用意する。|
|2|`値取得`|設定値、引数、DB結果、ファイル情報を取得する。|
|3|`変換`|文字列、日時、JSON、HTML、ファイルパスなどを変換する。|
|4|`判定`|存在確認、上限、状態、権限、経過時間などを判定する。|
|5|`登録:where` / `登録`|DB条件Mapまたは登録Mapを作成する。|
|6|`クエリ実行`|`executeInsert`, `executeUpdate`, `executeDelete`, `selectRecordList` などを呼ぶ。|
|7|`レコードループ`|DB結果を走査して戻り値へ反映する。|
|8|`戻り値`|処理結果を返す。短い関数ではコメントなしで `return` してよい。|

## 8.1.1 Operation / OperationCheck テンプレート

Operation系クラスは、クラスヘッダーに更新日とメソッド一覧を置き、業務分類ごとに公開メソッドを並べる。`OperationCheck` は `blSuccess` を確認しながらレスポンス確認メソッドを連鎖させる。

```java
    ///////////////////////////////////////////////////////////////////
    //
    //  リクエスト確認 -> 記事登録
    //
    ///////////////////////////////////////////////////////////////////
    public void registArticle(String userId) throws JsonProcessingException, SQLException, ParseException {
        if(blSuccess) {responseCheckLogin          ();      }  //ログインチェック
        if(blSuccess) {responseExistUser           (userId);}  //ユーザの存在確認
        if(blSuccess) {responseCheckArticleDatetime(userId);}  //記事連続投稿確認（5分以内）
    }
```

## 8.1.2 OperationTable テンプレート

OperationTable参照系は、JSON変換、条件作成、SQL作成、条件Map登録、クエリ実行、戻り値返却の順を基本にする。

```java
        //オブジェクトマッパ
        ObjectMapper objectMapper = new ObjectMapper();

        //json -> List<String>
        List<String> listUserIdShowed = objectMapper.readValue(jsonUserIdShowed, new TypeReference<List<String>>(){});

        //クエリ
        String sql = " "
                   + " SELECT "
                   + "   tm.userId AS tm_userId "
                   + " , tm.name   AS tm_name "
                   + " FROM userList AS tm ";

        //初期化
        Map<String, Object> mapCondition = new LinkedHashMap<String, Object>();

        //レコードリスト取得
        List<Map<String, String>> listRecord = library.libDatabase.selectRecordList(sql, mapCondition);

        return listRecord;
```

|項目|規約|
|---|---|
|SQL文字列|複数行連結では `String sql = " "` から開始し、2行目以降は `+` の位置を揃える。|
|Map|条件順が意味を持つ場合は `LinkedHashMap` を使う。|
|レコード取得|戻り値名は `listRecord`, `mapRecord`, `listUser` など処理対象が分かる名前にする。|
|レコードループ|`for(Map<String, String> mapColumn : listRecord)` 内は空行を入れず、`mapColumn.get("...")` のキー幅を揃える。|

## 8.1.3 Library テンプレート

Library系は、変更履歴付きヘッダー、メソッド一覧、フィールド、コンストラクタ、薄い委譲メソッドを基本にする。単純な委譲メソッドでも、新規・変更コードでは戻り値を変数へ格納してから返す。過度なブロックコメントは追加しない。

```java
    ////////////////////////////////////////////////////////////////////////////////
    //
    //  レコードリスト取得 -> クエリ指定
    //
    ////////////////////////////////////////////////////////////////////////////////
    public List<Map<String, String>> selectRecordList(String sql, Map<String, Object> mapCondition) throws SQLException{
        //レコードリスト取得
        List<Map<String, String>> listRecord = libSubPreparedStatement.selectRecordList(sql, mapCondition);

        return listRecord;
    }
```

## 8.1.4 アップロードServlet / 更新OperationTable テンプレート

アップロードServletは、チャンク単位のパラメタを取得し、ログイン確認後に `operationFile` へ委譲して `bl` を返す。

```java
            //パラメタ取得
            String nameRandom =                  req.getParameter("nameRandom");
            String fileNo     =                  req.getParameter("fileNo"    );
            int    chunkCount = Integer.parseInt(req.getParameter("chunkCount"));
            String dataBase64 =                  req.getParameter("dataBase64");

            //リクエスト確認 -> ファイルアップロード
            operationServlet.operationCheck.uploadFile();

            //処理
            if(blSuccess) {
                //ファイルアップロード
                boolean bl = operationServlet.operationFile.uploadFile(
                      nameRandom
                    , fileNo
                    , chunkCount
                    , dataBase64
                );
                //登録
                mapResponse.put("bl", bl);
            }
```

更新系OperationTableは、`mapValue` を `登録:set`、`mapCondition` を `登録:where` として分ける。

```java
        //テーブル
        String table = "articleList";

        //初期化
        Map<String, Object> mapValue     = new HashMap<String, Object>();
        Map<String, Object> mapCondition = new HashMap<String, Object>();

        //登録:set
        mapValue.put("S@articleContents"    , articleContents    );
        mapValue.put("S@htmlArticleContents", htmlArticleContents);

        //登録:where
        mapCondition.put("S@articleId@=", articleId);

        //クエリ実行
        int n = library.libDatabase.executeUpdate(table, mapValue, mapCondition);
```

|項目|規約|
|---|---|
|アップロードAPI|`nameRandom`, `fileNo`, `chunkCount`, `dataBase64` を取得し、型変換と `req.getParameter` の位置を揃える。|
|更新API|更新対象ID、ログインユーザID、更新値を取得し、`operationCheck` で権限確認してから更新処理を呼ぶ。|
|更新Map|set対象は `mapValue`、where対象は `mapCondition` に分け、コメントも `登録:set`、`登録:where` とする。|
|戻り値|更新系APIは既存キー `bl` を返す。戻り値が使われない `int n` でも、既存形式に合わせてクエリ実行結果を受ける。|

## 8.2 if / for / try のコメント

```java
        //レコードループ
        for(Map<String, String> mapColumn : listRecord) {
            //値取得
            datetimePostLast = mapColumn.get("datetimePostLast");
            //
            break;
        }
```

|構文|規約|
|---|---|
|`if`|直前に条件の意味を書く。例: `//5分以内`, `//処理成功`, `//ログイン済み`。|
|`else if`|短い条件分岐は既存の縦揃え形式に合わせてよい。|
|`for`|走査対象を `レコードループ`, `ファイルループ`, `アドレスループ` のように書く。|
|`while`|読み込みや繰り返し条件を `行読み込みループ` などで書く。|
|`try`|Servletでは大区切りとして `try` を置く。Library内で使う場合は対象処理名を補う。|
|`catch`|Servletでは `error` 区切り、Library内では例外処理内容を短く書く。|

短い条件式では、既存コードのようにコメント、条件、代入を近接させてよい。ただし、処理が複数行に広がる場合は中括弧とブロックコメントを使う。

```java
        //クレジット変換
        Boolean blCredit = null;

        //
             if(credit.equals("f"))blCredit = false;
        else if(credit.equals("t"))blCredit = true;
```

## 9. コメントと文字コード

- Javaソースのコメントがコンソール表示上で文字化けする場合は、エンコードを変えて確認する。
- コメント内容を確認できない場合、推測で日本語を書き換えない。
- メソッド一覧コメントや大きな区切りコメントは、周辺クラスの形式に合わせる。
- メソッド定義直前コメントは、区切り線、空行、`//  対象 + 処理内容`、空行、区切り線の形式を基本にする。
- 新規コメントは、対象テーブル、返す一覧・登録するレコード・更新する状態・削除する行、処理内容、戻り値、副作用が分かる粒度にする。

```java
    ///////////////////////////////////////////////////////////////////
    //
    //  ユーザリスト取得
    //
    ///////////////////////////////////////////////////////////////////
    public List<Map<String, String>> selectUserList(...) {
    }
```

|項目|規約|
|---|---|
|説明粒度|`記事登録`, `記事リスト取得`, `チャットメッセージ状態更新`, `ランダムid取得` のように対象と処理内容を書く。|
|Servlet|`post`, `get` などHTTPメソッド名のコメントは既存Servlet形式に合わせる。|
|OperationTable|記事、ユーザ、チャットなど対象テーブル群と、登録/取得/更新/削除/確認を分かる形で書く。|
|メソッド一覧|ファイル冒頭のメソッド一覧コメントと実定義直前コメントの説明を揃える。|

## 10. 確認

|確認項目|内容|
|---|---|
|ビルド|Gradle正式手順で `.\gradlew.bat clean build` を確認する。|
|API整合|Servlet URL、`ClassServerMethod.js`、画面JSの呼び出し名を照合する。Operation層は原則として同一名とする。|
|認証|`OperationCheck.responseCheckLogin()` の要否を既存API分類と比較する。|
|レスポンス|既存画面が参照するキーを維持する。|
|副作用|DB更新、ファイル操作、メール送信、WebSocket送信は受け入れ条件を確認する。|

## 11. 参照した主なファイル

- `src/operation/OperationServlet.java`
- `src/operation/OperationExport.java`
- `src/operation/OperationWebSocket.java`
- `src/operation/operationServlet/OperationCheck.java`
- `src/operation/operationServlet/OperationFile.java`
- `src/operation/operationServlet/OperationSns.java`
- `src/operation/operationServlet/operationTable/OperationTableArticleSelect.java`
- `src/operation/operationServlet/operationTable/OperationTableArticleUpdate.java`
- `src/library/libDatabase/LibDatabase.java`
- `src/library/libFile/LibFile.java`
- `src/library/libImage/LibImage.java`
- `src/servlet/restApi/article/article/SvRegistArticle.java`
- `src/servlet/restApi/chat/chat/SvUpdateChatContentsOfStateRead.java`

## 4. カスタムルールの追加事項

## REST Servlet・Operation層の実装ルール

- 画面のJavaScriptからDB情報を取得するServletは、`src/servlet/restApi/{機能}/{サブ機能}/` に配置する。
- Servletの配置先とpackage名は、同じ機能分類にある既存Servletを複数確認して決定する。画面単位という理由だけで `src/servlet/page/` 等の新しいルートを作成しない。
- 画面データ取得の基本フローは、`JavaScript -> ClassServerMethod.js -> ClassServer.js -> REST Servlet -> OperationCheck.java -> OperationTable.java -> OperationTable{機能}{Select|Update}.java` とする。
- REST Servletは、リクエスト値の取得後に `OperationCheck.java` の対応するpublic関数を呼び、`blSuccess` がtrueの場合だけテーブル操作を呼び出す。
- REST ServletはREST窓口、リクエスト値取得、Operation呼出およびレスポンス出力に限定し、DB行の変換や業務データ組み立てを行うprivate関数を追加しない。
- DB行から画面・API用データへ変換する処理が既存のOperationTableの責務に収まる場合は、対応する`OperationTable{機能}Select.java`へ配置する。
- `OperationCheck.java`では、原則としてREST入力値を確認する。ただし、ログイン、対象データの存在、権限、操作間隔、登録上限など、後続のOperationを実行できるか判定する確認項目がない場合は、個別のprivateチェック関数を追加しない。
- Operation固有の検索条件、取得範囲およびSQL実行上の制約は、必要に応じて対応する`OperationTable{機能}{Select|Update}.java`内で確認する。
- 個別のprivateチェック関数がない場合も、Servletは`OperationCheck.java`のpublic関数を呼び、既存のレスポンス初期化フローを維持する。
- DBの参照・更新処理は、`src/operation/operationServlet/operationTable/OperationTable{機能}Select.java` または `OperationTable{機能}Update.java` に配置し、`OperationTable.java` から初期化する。
- 既存構成でServletとOperationTableだけで処理できる場合、画面機能ごとの中間Operationクラスを新規作成しない。中間Operationクラスが必要な場合は、既存の類似構成と責務を確認し、作成理由を詳細設計に記載してユーザ承認を得る。
- Servlet、OperationCheck、OperationTableの追加・変更時は、詳細設計のファイル構成、関数一覧、呼出関係、変更区分を実コードと一致させる。

## 新規ファイル・ディレクトリ配置ルール

- 新規ファイルの配置先を決める前に、同じ種類かつ同じ役割の既存ファイルを複数確認する。
- HTML、JavaScript、CSS、ServletおよびOperationは、それぞれ別の配置規則として既存構成を確認する。別種類のファイルにある中間階層をそのまま転用しない。
- 詳細設計に記載された配置先も現行プロジェクト構成と照合し、不要な中間ディレクトリを新規作成しない。
- 詳細設計と現行構成が異なる場合は、実装前に作業を止め、正しい配置先と設計書の修正対象をユーザへ提示して承認を得る。

### 新規ファイル・ディレクトリ配置確認チェックリスト

- [ ] 同じ種類・役割の既存ファイルを複数確認したか。
- [ ] HTML、JavaScript、CSS、ServletおよびOperationを種類別に確認したか。
- [ ] 他の種類のファイルだけに存在する中間階層を転用していないか。
- [ ] 詳細設計の配置先と現行プロジェクト構成が一致しているか。
- [ ] 新規ディレクトリに不要な中間階層がないか。
- [ ] 設計と現行構成が異なる場合、実装前にユーザ承認を得たか。

### REST Servlet・Operation層の確認チェックリスト

- [ ] 同じ機能分類の既存Servletを複数確認したか。
- [ ] Servletを`src/servlet/restApi/{機能}/{サブ機能}/`へ配置したか。
- [ ] `JavaScript -> ClassServerMethod.js -> ClassServer.js -> REST Servlet -> OperationCheck.java -> OperationTable.java -> OperationTable{機能}{Select|Update}.java`の流れになっているか。
- [ ] REST ServletにDB行の変換や業務データ組み立てを行うprivate関数を追加していないか。
- [ ] DB行から画面・API用データへの変換処理を、対応するOperationTableへ配置したか。
- [ ] 新規リクエスト値について、後続のOperationを実行できるか判定する確認項目の有無を確認したか。
- [ ] 実行可否の確認項目がある場合だけ、`OperationCheck.java`へ個別のprivateチェック関数を追加したか。
- [ ] 実行可否の確認項目がない場合、不要なprivateチェック関数を追加せず、public関数によるレスポンス初期化だけを行っているか。
- [ ] Operation固有の検索条件、取得範囲およびSQL実行上の制約を、必要に応じて対応するOperationTable内で確認しているか。
- [ ] DB処理を`OperationTable{機能}{Select|Update}.java`へ配置したか。
- [ ] 不要な中間Operationクラスを作成していないか。
- [ ] 詳細設計と実コードのファイル配置、関数一覧、呼出関係および変更区分が一致しているか。

## Javaのtry・catch・finallyコメント規約

- Javaの`try`、`catch`、`finally`には、各構文の直前にそれぞれ`//try`、`//catch`、`//finally`のコメントを記載する。
- `catch`または`finally`が閉じ波括弧と同じ行から始まる場合、対応するコメントを閉じ波括弧の直前に記載する。
- Javaの予約語とコメントは、`finally`の綴りに統一する。

```java
//try
try {
    //処理
//catch
} catch(Exception e) {
    //エラー処理
//finally
} finally {
    //終了処理
}
```

### Javaのtry・catch・finallyコメント確認チェックリスト

- [ ] 追加・変更した全ての`try`の直前に`//try`があるか。
- [ ] 追加・変更した全ての`catch`の直前に`//catch`があるか。
- [ ] 追加・変更した全ての`finally`の直前に`//finally`があるか。
- [ ] `finally`の綴りが予約語・コメントともに正しいか。

## Javaの関数・制御構文・整列規約

- Javaの関数は、原則として開始波括弧直後に1行の空行を入れてから、最初のコメントまたは処理を記載する。
- コメントと1処理程度で完結する短いJava関数は、開始波括弧直後の空行を省略する。
- Javaの`if`、`else`、`for`、`while`、`try`、`catch`および`finally`の内部では、原則として処理間に空行を入れない。
- Servletの`try`内で既存テンプレートに従って配置する大分類の区切りコメント間は、制御構文内の不要な空行として扱わない。
- 連続する代入、関数呼出および`Map.put`は、対応関係を確認できるように、変数名、引数、カンマ、閉じ括弧およびセミコロンの位置を可能な範囲で揃える。
- 1行に複数の`Map.put`を記載する場合は、同じ列のキー、値および`);`の位置を揃える。

### Javaの関数・制御構文・整列確認チェックリスト

- [ ] 追加・変更したJava関数について、短い関数を除き、開始波括弧直後に1行の空行があるか。
- [ ] コメントと1処理程度で完結する短いJava関数では、開始波括弧直後の不要な空行を省略したか。
- [ ] 追加・変更した`if`、`else`、`for`、`while`、`try`、`catch`および`finally`内に不要な空行がないか。
- [ ] Servletの大分類区切りと通常の不要な空行を区別して確認したか。
- [ ] 連続する代入・関数呼出・`Map.put`の対応する位置が揃っているか。
- [ ] 1行に複数記載した`Map.put`のキー、値および`);`の位置が揃っているか。
