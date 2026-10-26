# batch Pythonコード実装規約

## 1. 目的・適用

batchの手順8でPythonファイルを新規作成または変更する場合に適用します。`.rules/dev/batch/rules_batch_8_code.md`と本規約を両方全文確認して下さい。

本規約はPython固有の実装方法を定めます。性能改善を目的とする場合は、さらに`.rules/dev/batch/other/performance/rules_batch_performance_optimization.md`とチェックリストを適用して下さい。

## 2. 優先順位と適用原則

- ユーザの明示指示、承認済み仕様・設計、本規約、batch手順8の共通規約、周辺コードの順に優先して下さい。
- batch手順8のcamelCase、`//`および`/** ... */`の例はPythonへ適用せず、本規約のsnake_case、`#`およびPython用ヘッダーを使用して下さい。
- 既存コードに旧形式があることだけを互換要件とせず、承認済みの外部契約だけを維持して下さい。
- 規約適用だけを目的として変更対象外のコードを一括整形、改名または移動してはいけません。
- 実装前に本規約の全章を確認し、実装後は専用チェックリストへ全項目の適用有無、判定および根拠を記録して下さい。

## 3. ファイル構成と空白行

### 3.1 ファイルヘッダー

新規または版管理対象のPythonファイルは、原則として次の形式で開始して下さい。

```python
# 作成日時: YYYY-MM-DD JST
# 更新日時: YYYY-MM-DD JST
# 設計ID: Snnn / Fnnn / Tnnn-Tnnn
# 処理概要: ファイルの責務を日本語1文で記載する。
# 前回バージョンとの差異: [ADD]/[MOD]/[KEEP]/[DEL]と変更点を記載する。
"""moduleの責務、主要な入力・出力および副作用を記載する。"""
```

### 3.2 記述順と入口

moduleは、module docstring、標準library import、第三者library import、project内import、logger、定数・型、`main()`、sub関数・class、実行guardの順を基本として下さい。

- module・package名は小文字のsnake_caseとし、実行順を持つ工程fileは設計で確定した`{工程prefix}_{NN}_{処理名}.py`を使用して下さい。
- classを主定義とするmoduleは`class_process_NN_処理名.py`を基本とし、工程内の`class_process/`等、`class_`で始まる専用folderへ配置して下さい。関数主体の補助folderへ混在させてはいけません。
- 実行可能moduleでは、module-level定義後の最初の関数を`main()`とし、sub関数・sub classは最初に呼び出される順に記載して下さい。
- import専用moduleへ空の`main()`や実行guardを追加してはいけません。
- `__init__.py`で重いI/O、計算、暗黙登録または互換aliasを実行してはいけません。
- 実行可能moduleの末尾は次の固定形式とし、`main()`は`int`を返して下さい。

```python
if __name__ == "__main__":
    raise SystemExit(main())
```

### 3.3 空白行と位置揃え

- top-levelの関数・class間およびclass内method間は空白行2行、class宣言直下と関数・method本体の先頭は空白行1行として下さい。
- 関数見出し、設計IDコメント、decoratorおよび定義の間には空白行を入れないで下さい。
- 入力検証、値取得、初期化、変換、計算、登録、出力および検証の処理block間は空白行1行で分けて下さい。
- 代入blockの後に`if`、`for`または`while`へ進む場合は、制御文コメントの直前へ空白行1行を入れ、コメントと制御文の間には空白行を入れないで下さい。
- 同一処理コメント配下の連続代入、同種のkeyword引数およびmapping要素は、行長上限内で`=`または区切り位置を揃えて下さい。
- indentationは空白4文字とし、tab、行末空白、不要な連続空白行を使用しないで下さい。

## 4. 命名

|対象|規約|例|
|---|---|---|
|module・package|snake_case|`condition_catalog.py`|
|関数・method|処理種別prefix＋snake_case|`select_list_resource`|
|class・dataclass・例外|PascalCase|`ResourceConfig`|
|定数|UPPER_SNAKE_CASE|`SHA256_HEX_LENGTH`|
|変数・引数・属性|業務役割名または構造prefix＋snake_case|`type_condition`, `key_canonical`|
|private関数・method|`_`＋処理種別prefix|`_select_columns`|

- 関数・methodは、`select_`、`regist_`、`update_`、`delete_`、`trans_`、`calculate_`、`create_`、`import_`、`export_`、`extract_`、`predict_`、`validate_`または`check_`等、実処理と一致するprefixで開始して下さい。
- 単一状態の真偽判定は`is_`、`has_`または`exist_`を使用できます。`main`、Python特殊method、test、framework固定名および承認済み外部APIは例外です。
- 変数の主な構造prefixは、`bl_`、`num_`、`type_`、`key_`、`list_`、`dict_`、`set_`、`tuple_`、`df_`、`lf_`、`sr_`、`nd_`、`path_`、`date_`、`datetime_`および`obj_`として下さい。
- `str_`は使用せず、文字列も`type_condition`、`key_canonical`、`condition_id`、`definition_sha1`等の内容または役割で命名して下さい。
- 1文字名、`tmp`、`data2`、`new_data`等の意味が不明な恒久名を使用しないで下さい。

## 5. 関数、classおよびコメント

### 5.1 class

class直前には次の見出しを使用して下さい。状態や不変条件をまとめる必要がある場合だけclass化し、namespace目的のclassを作らないで下さい。

```python
#########################################################################################
#
#
#  クラス：○○
#
#
#########################################################################################
class ClassName:

    # 最初のclass変数、instance変数またはmethod
```

- private methodは原則として`_method()`とし、name manglingが必要な場合だけ`__method()`を使用して下さい。
- 継承よりcompositionを優先し、共有不要なmutable class変数を使用しないで下さい。

### 5.2 関数見出し

所有する全関数・methodの定義上側へ、public/privateや長短を問わず次の見出しを付けて下さい。

```python
#########################################################################################
#
#  対象銘柄一覧取得
#
#  説明
#    有効なresourceから対象銘柄を固定順で取得する。
#
#  引数
#    path_resource: 検証済みresource定義のpath。
#
#  戻り値
#    有効な対象銘柄を固定順で格納したlist。
#
#  例外
#    ValueError: resource定義が不正な場合。
#
#########################################################################################
# [T101] [L101-L102] 対象銘柄一覧を取得する。
def select_list_target_symbol(path_resource: Path) -> list[str]:

    #値取得 : resource定義
```

- 見出しには該当する説明、引数、戻り値、例外および副作用を記載し、該当しない項目だけを省略して下さい。
- 関数説明は定義上側へ集約し、関数内へ同じ説明のdocstringを重複記載しないで下さい。module・class docstring、またはframeworkが必須とする関数docstringは例外です。
- decoratorがある場合は、関数見出し、設計IDコメント、decorator、関数定義の順に配置して下さい。

### 5.3 処理コメント

- 代入直前には、`#初期化`、`#値取得`、`#値設定`、`#変換`、`#計算`、`#登録`、`#更新`または`#パス作成`等、目的を表す日本語コメントを必ず記載して下さい。
- 同じ目的の連続代入は1つのコメントへまとめ、目的の異なる代入や分岐内の代入は分けて下さい。
- `for`・`while`直前には対象を示す`#○○ループ`、`if`・`elif`・`else`直前には業務条件を示す`#○○の場合`を必ず記載して下さい。
- コメント規約を理由として1行ずつのDataFrame処理、深いnestまたは過度な関数分割を行ってはいけません。

### 5.4 引数、戻り値および責務

- 業務上の意味を持つliteral、演算式、属性・要素または関数結果を引数へ直接渡さず、意味の分かる変数へ格納して下さい。
- 値を返す関数・methodでは、関数結果、属性・要素、演算式、条件式またはliteralを直接`return`せず、意味の分かる変数を返して下さい。値なしreturn、generatorおよびframework固定記述は例外です。
- 1関数1責務とし、計算関数の中でファイル出力、状態確定または環境変更を暗黙実行しないで下さい。

## 6. 型、安全性および確認

- 公開関数、工程関数、外部I/O関数および複雑な補助関数には、引数型と戻り値型を記載して下さい。
- `Any`、mutable default引数、実行時`sys.path`変更、CWD依存import、`eval`、`exec`、未信頼pickleおよびshell文字列連結を避けて下さい。
- 旧関数名、旧class、旧moduleまたは旧importのalias、wrapper、fallbackおよび二重実装は、承認済みsource/API互換契約がある場合だけ追加し、対象、理由および削除条件を記録して下さい。
- パスは`pathlib.Path`で扱い、外部入力から作るパスが許可された基準directory内にあることを検証して下さい。
- 例外を成功扱いで握りつぶさず、変換時は`raise ... from exc`で原因を保持して下さい。`except Exception`は原則として入口でのログ・終了code変換に限定して下さい。
- module loggerは`logging.getLogger(__name__)`を使用し、library内部でroot loggerを設定しないで下さい。
- 入出力のschema、型、null、順序、文字code、timezone、丸め、件数およびhashを明示し、同じ入力・設定・依存版から同じ結果を生成して下さい。
- 正常系、境界値、異常系、0件、欠落、重複、順序、再実行、再開および回帰を確認して下さい。
- 変更した全差分と関数・method・class全体を読み直し、呼出元、呼出先、型、例外、副作用およびtestを確認して下さい。

## 7. 実装方式の選択

ライブラリ名だけで方式を決めず、データ量、処理内容、出力契約、実行環境、メモリ上限および実測結果から選択して下さい。

|方式|優先して検討する用途|主な確認事項|
|---|---|---|
|Polars|大量表データのscan、抽出、結合、group_by、window、streaming|lazy、filter pushdown、列削減、null、型、順序|
|NumPy|大量の配列演算、ブールマスク、反復数値計算、bincount、共有配列|dtype、NaN、inf、精度、行位置|
|pandas|pandas固有機能が必要な処理、小規模データ、外部APIがpandasを要求する処理|行単位処理、index依存、copy、groupby、型|
|Python標準処理|制御、設定、manifest、少量データ、外部I/Oの調停|計算量、メモリ、再実行性|

- 既存コードとの互換性だけを理由にpandasを選択しない。
- 同じ入力、同じ出力契約、同じ環境で候補を比較する。
- pandas、NumPy、Polars間の変換回数とコピー量を含めて評価する。
- 最速でも結果が不一致となる方式は採用しない。

## 8. ループと反復計算

- DataFrameの行ごとの`for`、`iterrows()`、`itertuples()`および`apply(axis=1)`は原則として使用しない。
- 列式、ベクトル演算、join、group_by、window式、ブールマスクまたは配列演算を優先する。
- 状態が前行に依存する時系列処理等でループが必要な場合は、必要性、計算量、対象件数および計測結果を記録する。
- 総当たりや反復計算では、不変な特徴量、マスク、検索表、index、期間境界および共通配列をループ外で一度だけ計算する。
- 内側ループでDataFrameのfilter、join、copy、文字列解析またはファイル読込を繰り返さない。
- 組合せを無制限にmaterializeせず、generator、分割、chunkまたはstreamingを使用する。

## 9. Polars

- 大量ファイルは可能な限り`scan_csv`、`scan_parquet`等のLazy APIから検討する。
- 必要な列選択とfilterを早い段階へ置き、不要な全列・全行をcollectしない。
- Python UDF、`map_elements`および行単位変換よりPolars式を優先する。
- `collect()`の位置と回数を明示し、同じLazyFrameを無目的に複数回collectしない。
- streamingの適用可否と制限を実データで確認する。
- `maintain_order`やsortは出力契約に必要な場合だけ使用し、必要な順序は明示する。

## 10. NumPy

- dtypeを意図せず拡大せず、精度、欠損、値域およびメモリ量から選択する。
- 同じDataFrame列を反復ごとにNumPy配列へ変換せず、必要な配列を事前に作成する。
- 一時配列の生成回数を抑え、可能な場合は再利用または出力先指定を検討する。
- ブールマスク、broadcast、`where`、`bincount`等は、境界、NaN、infおよび行対応を確認して使用する。
- 配列位置と業務キーの対応を別々に検証する。

## 11. pandas

- pandas固有の要件がない場合、表形式の大量変換ではPolarsまたはNumPyを比較する。
- `iterrows()`、行単位`apply()`、ループ内`concat()`および過剰な`copy()`を使用しない。
- `concat()`は原則として対象を収集後に一度だけ実行する。
- indexが契約でない場合、暗黙のindex整列へ依存しない。
- object dtypeの増加、暗黙の型変換および不要な中間DataFrameを確認する。

## 12. I/Oとメモリ

- 読込時に必要な列、行、型および期間を限定する。
- 大量CSVを何度も走査せず、要件に応じてParquet、Arrow IPC、データベースまたは安全な中間形式を検討する。
- 中間成果物には入力ハッシュ、範囲、件数、schema、作成条件および再利用条件を持たせる。
- 全件をPythonのlist、dictまたはオブジェクトへ変換しない。
- chunkまたはstreamingでは、chunk境界で結果が変わらないことを確認する。
- 一時ファイルと正式成果物を分離し、異常終了時に不完全な正式成果物を残さない。

## 13. 分割・並列・再開

- ベクトル化、不要計算削減、スキャン削減およびI/O改善を先に検討する。
- shardは計算結果を変えないキーまたは範囲で分割し、重複と欠落を検証する。
- process数、thread数、ライブラリ内部threadを同時に増やして過剰割当しない。
- shardごとに入力ハッシュ、範囲、件数、状態および出力ハッシュをmanifestへ記録する。
- 成功済みshardを再実行せず、失敗または未処理だけを再開できるようにする。
- 統合処理で順序、重複、欠落、型および件数を再検証する。

## 14. データ契約と同値

- 列名、型、キー、順序、null、NaN、inf、丸め、タイムゾーンおよび文字コードを明示的に扱う。
- Polarsのnull、NumPyのNaNおよびpandasの欠損表現を同一視しない。
- 集約順または並列順が変わる場合、浮動小数点差を事前承認された許容誤差で比較する。
- 0件、境界、最大件数、重複キーおよび欠損を確認する。
- 高速化を理由に未来データ混入防止、入力検証、例外処理、監査ログまたは終了コードを変更しない。

## 15. 性能確認

性能分類を適用する場合、次を満たして下さい。

- 同一環境、入力ハッシュ、出力契約および測定条件で変更前後を比較する。
- 原則3回以上測定し、中央値、ばらつき、stage別時間、ピークメモリおよびI/Oを記録する。
- 小標本、代表データ、最大データおよび全件を段階的に検証する。
- 関数単体の改善をbatch全体の改善として報告しない。
- 全件同値、再実行、再開、冪等性および復旧に合格しない実装を採用しない。

## 16. 禁止事項

- 根拠なく「Polarsが最速」「NumPyが最速」「pandasで十分」と判断すること。
- 性能測定前に全面的なライブラリ置換を行うこと。
- タイムアウト延長だけを最初の性能対策にすること。
- 成功済み大規模計算を性能検証だけのために無断で再実行すること。
- 検証前の成果物を正式出力または`latest`へ切り替えること。
- privateデータ、秘密情報または巨大な中間データをログや公開artifactへ出すこと。
