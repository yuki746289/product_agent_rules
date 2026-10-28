# app SQLコード実装規約

## 1. 目的・適用

appの手順8でSQL文字列を新規作成または変更する場合に適用します。`.rules/dev/app/rules_app_8_code.md` と本規約を両方全文確認して下さい。

SQL文字列の書き方は、本ファイルを手順8のSQL規約として適用します。Java規約のDB操作と併用して下さい。

実装前に本規約の全章を確認し、実装後は同じフォルダの確認ファイルへ全項目の適用有無、判定および根拠を記録して下さい。

## 2. 優先順位と適用原則

- ユーザの明示指示、承認済み仕様・設計、本規約、app手順8の共通規約、周辺コードの順に優先して下さい。
- app手順8のcamelCase例、`//` の概念例およびC系header例と本規約が異なる場合は、本規約を優先して下さい。
- 対象ファイルまたは同一分類の周辺コードで規約と異なる書き方が複数の関数にわたり確立されている場合は、既存コードとの統一を優先して下さい。単発の記述だけで判断しないで下さい。
- 規約適用だけを目的として、変更対象外のコードを一括整形、改名または移動しないで下さい。
- 画面のレイアウト、id、class、共通枠、読込順およびレスポンシブは、HTML規約とCSS規約を詳細規約とします。汎用的なUI手順と重なる場合も、この詳細規約を優先して下さい。

## 3. 承認済み規約

## 8. DB操作

- DB処理は `Library`、`LibDatabase`、`LibSubPreparedStatement`、`OperationTable*` の既存責務に合わせる。
- SQL文字列、パラメータ、戻り値のMapキーは既存APIレスポンスと画面JSの参照名を確認する。
- 登録、更新、削除、件数更新、関連データ削除は副作用を伴うため、関数カタログの副作用と状態変更対象を確認する。
- 実DBとの差分がある場合は、`11_DB定義・ER図.md` とローカルDB確認結果を優先して判断する。

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

## 4. カスタムルールの追加事項

## OperationTableのSQL・Connection規約

- OperationTableのSELECT、INSERT、UPDATEおよびDELETEは、原則として`library.libDatabase`が公開するクエリ関数を使用する。
- SQL文字列、`LinkedHashMap<String, Object>`の条件、クエリ実行および戻り値の流れは、同じ処理種別の既存OperationTableを複数確認して合わせる。
- SQLでテーブルの略称を使用する場合、主テーブルは`tm`とする。
- 主テーブルへ直接結合するテーブルは、JOINの記載順に`ts_1`、`ts_2`、`ts_3`の連番とする。
- サブクエリ内でテーブルの略称を使用する場合は、サブクエリ内の記載順に`tt_1`、`tt_2`、`tt_3`の連番とする。
- SELECT句では`*`、`tm.*`、`ts_1.*`等を使用せず、取得するカラムを全て明示的に列挙する。
- SELECT句で列挙する全カラムおよび集計式には`AS`で列別名を付ける。
- テーブルから取得する全カラムの列別名は、同名カラムの重複有無に関係なく、`tm_{カラム名}`、`ts_1_{カラム名}`、`tt_1_{カラム名}`の形式とする。
- 主テーブルの取得カラムは`tm.column AS tm_column`、JOINテーブルの取得カラムは`ts_1.column AS ts_1_column`の形式とする。
- テーブルのカラムを使用する集計式は、対象テーブルの略称を付けた`tm_{集計値名}`等の列別名とする。テーブルを参照しない関数・定数の列別名には、テーブル略称を付けない。
- SQLの列別名を変更した場合は、Java、Servlet、JavaScript、APIおよびMapの参照キーを検索し、列別名と同じキーへ連動して修正する。
- `library.libDatabase.getConnection()`は新しいConnectionを生成せず、LibDatabaseが保持するConnectionを参照する関数として扱う。
- OperationTableでConnectionを直接参照するのは、AutoCommit状態の取得・設定・復元など、`library.libDatabase`の公開関数だけでは処理できない場合に限定する。
- Connectionを直接参照する場合も、通常のクエリは`library.libDatabase`経由で実行する。

### OperationTableのSQL・Connection確認チェックリスト

- [ ] 通常のクエリを`library.libDatabase`経由で実行しているか。
- [ ] SQLと条件Mapの書き方が既存OperationTableと一致しているか。
- [ ] 主テーブルの略称が`tm`になっているか。
- [ ] JOINテーブルの略称が記載順に`ts_1`、`ts_2`、`ts_3`となっているか。
- [ ] サブクエリ内のテーブル略称が記載順に`tt_1`、`tt_2`、`tt_3`となっているか。
- [ ] SELECT句で`*`、`tm.*`、`ts_1.*`等を使用せず、取得カラムを全て列挙しているか。
- [ ] SELECT句の全カラムおよび集計式に`AS`を付けているか。
- [ ] テーブルから取得する全カラムの列別名が、重複有無に関係なく`tm_`、`ts_1_`、`tt_1_`等のテーブル略称付きになっているか。
- [ ] 主テーブルが`tm.column AS tm_column`、JOINテーブルが`ts_1.column AS ts_1_column`の形式になっているか。
- [ ] テーブルのカラムを使用する集計式の列別名に、対象テーブルの略称を付けているか。
- [ ] SQLの列別名を変更した場合、Java、Servlet、JavaScript、APIおよびMapの参照キーも同じキーへ修正しているか。
- [ ] `getConnection()`を新規Connectionの生成処理として扱っていないか。
- [ ] Connectionの直接参照をAutoCommit状態等の必要な処理だけに限定しているか。
- [ ] Connectionを直接参照する理由をコメントまたは詳細設計に記載しているか。
