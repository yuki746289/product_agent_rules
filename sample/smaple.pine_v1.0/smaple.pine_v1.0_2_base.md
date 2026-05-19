# smaple.pine v1.0 基本設計書

## 仕様継承テーブル

| 仕様ID | 状態 | 変更内容 | ソース元 |
|---|---|---|---|
| S001 | 新規 | Pine Script インジケーター定義を追加 | v1.0_request |
| S002 | 新規 | 入力設定を追加 | v1.0_request |
| S003 | 新規 | EMA 計算処理を追加 | v1.0_request |
| S004 | 新規 | EMA 表示処理を追加 | v1.0_request |

## 1. 処理概要

<span style="color:green">[ADD]</span> TradingView のチャート上に EMA を3本表示する Pine Script インジケーターを作成します。

<span style="color:green">[ADD]</span> EMA の価格ソース、3本分の期間、色、線幅を入力設定として持たせます。

<span style="color:green">[ADD]</span> 売買シグナル、アラート、バックテスト処理は実装しません。

## 2. 処理フロー

| ID | 内容 |
|---|---|
| <span style="color:green">[ADD]</span> S001 | Pine Script v6 のインジケーターとして `overlay=true` で宣言します。 |
| <span style="color:green">[ADD]</span> S002 | 価格ソース、EMA1/EMA2/EMA3 の期間、色、線幅を入力として受け取ります。 |
| <span style="color:green">[ADD]</span> S003 | 入力された価格ソースと期間から、3本の EMA を算出します。 |
| <span style="color:green">[ADD]</span> S004 | 算出した EMA をチャート上へ3本のラインとして表示します。 |

## 3. 入力形式

| 項目 | 型 | 初期値 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> sourceInput | series float | close | EMA 計算に使用する価格ソース |
| <span style="color:green">[ADD]</span> ema1LengthInput | int | 20 | EMA1 の期間 |
| <span style="color:green">[ADD]</span> ema2LengthInput | int | 50 | EMA2 の期間 |
| <span style="color:green">[ADD]</span> ema3LengthInput | int | 200 | EMA3 の期間 |
| <span style="color:green">[ADD]</span> ema1ColorInput | color | color.teal | EMA1 の表示色 |
| <span style="color:green">[ADD]</span> ema2ColorInput | color | color.orange | EMA2 の表示色 |
| <span style="color:green">[ADD]</span> ema3ColorInput | color | color.purple | EMA3 の表示色 |
| <span style="color:green">[ADD]</span> lineWidthInput | int | 2 | EMA ラインの太さ |

## 4. 出力形式

| 項目 | 型 | 説明 |
|---|---|---|
| <span style="color:green">[ADD]</span> ema1Value | series float | EMA1 の表示値 |
| <span style="color:green">[ADD]</span> ema2Value | series float | EMA2 の表示値 |
| <span style="color:green">[ADD]</span> ema3Value | series float | EMA3 の表示値 |
| <span style="color:green">[ADD]</span> plot | chart line | チャート上に表示される EMA ライン |

## 5. 設定

| 項目 | 値 |
|---|---|
| <span style="color:green">[ADD]</span> Pine Script version | 6 |
| <span style="color:green">[ADD]</span> indicator title | Sample 3 EMA |
| <span style="color:green">[ADD]</span> overlay | true |
| <span style="color:green">[ADD]</span> max input length | 1以上 |

## 6. 特記事項

<span style="color:green">[ADD]</span> `pine/.rules/pine_checklist.md` はローカルに存在しないため、コード確認工程で未配置として記録します。

<span style="color:green">[ADD]</span> TradingView エディタでの実コンパイルはローカル環境では実施できないため、静的確認と公式ドキュメント照合を実施します。

## コンパイル・ダイジェスト

| タグ | 件数 | 内容（要約） |
|---|---:|---|
| [ADD] | 27 | EMA 3本表示用の入力、計算、表示、制約を追加 |
| [MOD] | 0 | なし |
| [DEL] | 0 | なし |
| [KEEP] | 0 | 初版のためなし |
