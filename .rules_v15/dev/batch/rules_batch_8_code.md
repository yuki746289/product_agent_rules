## コード設計書

詳細設計書（`_6_detail.md / .json`）および実装用統合JSON（`_7_detail.json`）に基づき、以下のルールを厳守してコードを実装して下さい。
開発方針やロジックに矛盾・不明点が見つかった場合は、実装を進める前に必ずユーザに報告し、指示を仰いで下さい。


### 1. 実装開始時の宣言（プレ・フライト・チェック）
実装を開始する直前に、以下の内容を回答として提示して下さい。
- **ターゲットJSONの確認:** 参照する `{filename}_v{n}.{m}_7_detail.json` のバージョン。
- **変更箇所のリストアップ:** JSON内の `[ADD]` および `[MOD]` タグが付いた全項目（L/F/T/I/O/V ID）を列挙し、それらを「どのファイル・どの関数」で実装するかを宣言して下さい。
- **Python規約の確認:** Pythonファイルを作成または変更する場合は、`.rules/dev/batch/rules_batch_8_code/rules_batch_8_code_python.md`を全文確認して下さい。
- **Python確認ファイルの使用:** Pythonファイルを作成、変更またはレビューする場合は、`.rules/dev/batch/rules_batch_8_code/rules_batch_8_code_python_confirm.md`を案件の成果物フォルダへコピーし、全項目の適用有無、判定および根拠を記録して下さい。
- **HTML・CSS・JavaScript・Java・SQL規約の確認:** 対象ファイルを作成または変更する場合は、同じフォルダの `rules_batch_8_code_html.md`、`rules_batch_8_code_css.md`、`rules_batch_8_code_javascript.md`、`rules_batch_8_code_java.md`、`rules_batch_8_code_sql.md` のうち該当する規約を全文確認して下さい。
- **言語別確認ファイルの使用:** 該当する `rules_batch_8_code_*_confirm.md` を案件の成果物フォルダへコピーし、全項目の適用有無、判定および根拠を記録して下さい。
- **性能追加分類の確認:** `performance`を適用する場合は、`.rules/dev/batch/other/performance/rules_batch_performance_optimization.md`と案件へコピーしたチェックリストを確認して下さい。

### 2. IDコメントの埋め込みルール
詳細設計との整合性を確保するため、コード内の適切な箇所に以下のコメントを**必須**で記述して下さい。

表中の`//`は概念例です。実装では対象言語のコメント記法を使用し、Pythonでは`#`を使用して下さい。

| ID種別 | 記述場所 | 記述例 |
| :--- | :--- | :--- |
| **[Lxxx]** | 処理ロジック・アルゴリズムの各ステップの開始位置 | `// [L101] 入力ファイル読込ロジック開始` |
| **[Fxxx]** | 主関数の**定義箇所（宣言部分）**の直前 | `// [F101] バッチ全体制御関数` |
| **[Txxx]** | 補助関数の**定義箇所（宣言部分）**の直前 | `// [T101] CSV変換補助関数` |
| **[Ixxx]** | 引数の**宣言・初期化箇所** | `// [I101] 入力ファイルパス` |
| **[Oxxx]** | 戻り値の**宣言・初期化・返却箇所** | `// [O101] 処理結果リスト` |
| **[Vxxx]** | メイン変数・定数の**宣言・初期化・参照または更新箇所** | `// [V101] ログ出力フラグを参照` |

### 3. コード品質・安全性の担保
- **F/T分類の維持:** 全ての関数定義直前に、対象言語のコメント記法で`[Fxxx]`または`[Txxx]`のどちらか一方を必ず記述して下さい。同一関数にF/Tを併記してはいけません。Pythonでは`# [Fxxx]`または`# [Txxx]`を使用して下さい。
- **不変条件（Invariants）の保護:** `[KEEP]` タグが付いた既存ロジックを修正・削除してはいけません。実装完了後、既存の挙動が維持されているか再確認して下さい。
- **ログ出力制御:** 共通変数 `isOutputLog` （詳細設計で定義されている場合）に従い、関数単位で処理の開始・終了・エラーログを出力できるようにして下さい。
- **エラーハンドリング:** 異常系が定義されている場合、詳細設計のステップ（[Lxxx]）に沿って例外処理を実装して下さい。

### 4. ヘッダー情報の記述
各ソースファイルの先頭には、必ず以下の情報を詳細に記載して下さい。

以下はC系言語の例です。PythonではPythonコード実装規約のファイルヘッダー形式を使用し、`/** ... */`を使用してはいけません。

```text
/**
 * 更新日時: yyyy/mm/dd
 * 処理概要: [Sxxx] に対応する処理の概要
 * 前回バージョンとの違い:
 * - [ADD]: 新規追加した機能
 * - [MOD]: 変更したロジック
 * - [DEL]: 廃止・コメントアウトした箇所
 */
```


### 5. 環境・言語固有の制約

  - 原因調査で分からないことなどがあれば、web検索で調べること。

  - 日本語の文字化けについて
    - PowerShell スクリプト（`.ps1`）に日本語を含める場合は、必ず UTF-8 with BOM で保存すること。
    - Windows PowerShell 5.x では BOM なし UTF-8 の `.ps1` が既定コードページで解釈され、文字化けすることがあるため、BOM なしで保存しないこと。
    - 日本語をファイルへ自動追記する処理を追加または修正した場合は、出力先ファイルで文字化け有無を必ず確認すること。

  - 記法と命名規則は対象言語のコード実装規約を優先すること。言語別規約がない言語では、既存コードと詳細設計を確認した上で本項のキャメルケース例を使用すること。
  - Pythonではsnake_case、PascalCaseおよびUPPER_SNAKE_CASEをPythonコード実装規約に従って使い分け、下記のcamelCase例を適用しないこと。
  - HTML、CSS、JavaScript、JavaおよびSQLでは、各言語のコード実装規約を優先すること。本項のcamelCase例、コメント例またはheader例と異なる場合は、言語別規約を適用すること。

  - 命名規則は下記に沿うこと
    - 変数の命名規則
      |種類|プレフィックス|例|
      |------|-----|----|
      |リスト|list|listData|
      |マップ|map|mapUser|
    - 関数の命名規則
      |プレフィックス|意味|例|
      |-------|-----|----|
      |select |データを取得する|selectData()|
      |regist |データを登録する|registData()|
      |delete |データを削除する|deleteData()|
      |update |データを更新する|updateData()|
      |refresh|画面などを更新する|refreshUserTable()|

### Pythonコード実装

Pythonファイルを新規作成または変更する場合は、`.rules/dev/batch/rules_batch_8_code/rules_batch_8_code_python.md`を適用して下さい。性能改善を伴う場合は、Python規約だけで完了せず、batch性能最適化ルールとチェックリストも適用して下さい。
Pythonコードの確認では、同じフォルダの`rules_batch_8_code_python_confirm.md`を使用し、未確認またはNGが1件でもある場合は手順8を完了してはいけません。

### HTML・CSS・JavaScript・Java・SQLコード実装

HTML、CSS、JavaScript、JavaまたはSQLを新規作成または変更する場合は、同じフォルダの言語別コード実装規約を適用して下さい。
確認では同じフォルダの言語別確認ファイルを使用し、未確認またはNGが1件でもある場合は手順8を完了してはいけません。

- HTML: `rules_batch_8_code_html.md` / `rules_batch_8_code_html_confirm.md`
- CSS: `rules_batch_8_code_css.md` / `rules_batch_8_code_css_confirm.md`
- JavaScript: `rules_batch_8_code_javascript.md` / `rules_batch_8_code_javascript_confirm.md`
- Java: `rules_batch_8_code_java.md` / `rules_batch_8_code_java_confirm.md`
- SQL: `rules_batch_8_code_sql.md` / `rules_batch_8_code_sql_confirm.md`

画面のレイアウト、id、class、共通枠、読込順およびレスポンシブは、HTML規約とCSS規約を優先して下さい。
