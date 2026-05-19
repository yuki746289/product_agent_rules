# smaple.pine v1.0 コード確認

## 確認対象

- コード: `smaple.pine_v1.0.pine`
- 実装用JSON: `smaple.pine_v1.0_5_detail.json`
- 参照ルール: `.rules_v6/dev/batch/rules_batch_6_code.md`, `.rules_v6/dev/batch/rules_batch_7_code_confirm.md`

## プレ・フライト・チェック

ターゲットJSONのバージョン: `1.0`

実装対象:

| ID | 種別 | 実装ファイル | 実装箇所 |
|---|---|---|---|
| L101 | ADD | `smaple.pine_v1.0.pine` | `indicator` 宣言 |
| L102 | ADD | `smaple.pine_v1.0.pine` | 入力設定 |
| L103 | ADD | `smaple.pine_v1.0.pine` | `refreshEmaSeries` |
| L104 | ADD | `smaple.pine_v1.0.pine` | `plot` |
| F101 | ADD | `smaple.pine_v1.0.pine` | `refreshEmaSeries` |
| T101 | ADD | `smaple.pine_v1.0.pine` | `selectEmaValue` |
| I101-I108 | ADD | `smaple.pine_v1.0.pine` | 入力定義、関数引数 |
| O101-O107 | ADD | `smaple.pine_v1.0.pine` | EMA 値、plot |
| V101-V111 | ADD | `smaple.pine_v1.0.pine` | 入力変数、EMA 値 |

## 1. トレーサビリティ・チェック

- [x] 実装用JSON内の [ADD] ID をコードコメントとして記述しています。
- [x] `F101` と `T101` は同一関数に併記していません。
- [x] コード内の関数定義には `F101` または `T101` を割り当てています。
- [x] 詳細設計にない売買シグナル、アラート、strategy 処理は追加していません。
- [x] 初版のため KEEP 保護対象はありません。

## 2. 品質・仕様チェック

- [x] JSON の機能、入力、出力、変数をコードへ反映しています。
- [x] 入力形式と出力形式は基本設計と整合しています。
- [x] 関数名、変数名はキャメルケースです。
- [x] PowerShell スクリプトではないため UTF-8 with BOM 要件の対象外です。

## 3. 動作検証

- [x] TradingView 公式ドキュメントで Pine Script v6 の `indicator`, `input`, `plot`, `ta.ema` の使用例を確認しました。
- [x] `request.*()` は使用していません。
- [x] 分岐内タプル代入は使用していません。
- [x] `:=` による再代入は使用していません。
- [ ] TradingView Pine Script エディタでの実コンパイルは未実施です。

## Pine 追加ルール確認

| 項目 | 判定 | 備考 |
|---|---|---|
| `pine/.rules/pine_checklist.md` 確認 | 保留 | ローカルにファイルが存在しませんでした。 |
| `pine/.rules/pine_checklist_log.md` 記録 | 保留 | 親フォルダとチェックリストが未配置のため作成していません。 |
| `:=` 確認 | OK | 未使用 |
| 分岐内タプル代入 | OK | 未使用 |
| `request.*()` 件数 | OK | 0件 |
| external elements | OK | 外部要素なし |

## F/T割当確認レポート

| 検証項目 | 件数 | 判定 | 備考 |
| :--- | ---: | :---: | :--- |
| named function 総数 | 2 | OK | `selectEmaValue`, `refreshEmaSeries` |
| `[Fxxx]` 割当 | 1 | OK | `refreshEmaSeries` |
| `[Txxx]` 割当 | 1 | OK | `selectEmaValue` |
| F/T両方併記 | 0 | OK | 問題なし |
| F/T未割当 | 0 | OK | 問題なし |
| ID重複 | 0 | OK | 問題なし |

## 最終判定レポート

| 検証項目 | 判定 | 備考 |
| :--- | :---: | :--- |
| 設計IDの一致 (Traceability) | OK | JSON ID とコードコメントを照合 |
| JSON仕様の網羅 (Coverage) | OK | 実装範囲を網羅 |
| 不変領域の維持 (Invariants) | OK | 初版のため KEEP 対象なし |
| 動作確認 (Execution) | 保留 | TradingView エディタでの実コンパイルは未実施 |

**総合判定: 保留**

## 未確認事項

- TradingView Pine Script エディタでのコンパイル確認。
- `pine/.rules/pine_checklist.md` が未配置である点の扱い。
