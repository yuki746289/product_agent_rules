# smaple.pine v1.0 詳細設計書

## S001 インジケーター定義

### 1. 処理概要

<span style="color:green">[ADD]</span> [S001] Pine Script v6 のインジケーターとして、価格チャート上に重ねて表示するための宣言を行います。

### 2. メイン関数

<span style="color:green">[ADD]</span> [F101] `refreshEmaSeries` は、EMA 算出の主処理を統括します。

### 3. 引数

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [I101] | sourceInput | series float | EMA 計算に使用する価格ソース |
| <span style="color:green">[ADD]</span> [I102] | ema1LengthInput | int | EMA1 の期間 |
| <span style="color:green">[ADD]</span> [I103] | ema2LengthInput | int | EMA2 の期間 |
| <span style="color:green">[ADD]</span> [I104] | ema3LengthInput | int | EMA3 の期間 |

### 4. 戻り値

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [O101] | ema1Value | series float | EMA1 の値 |
| <span style="color:green">[ADD]</span> [O102] | ema2Value | series float | EMA2 の値 |
| <span style="color:green">[ADD]</span> [O103] | ema3Value | series float | EMA3 の値 |

### 5. メイン変数

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [V101] | sourceInput | series float | 入力価格ソース |

### 6. 処理フロー

- <span style="color:green">[ADD]</span> [L101] `//@version=6` と `indicator(..., overlay=true)` を宣言します。 (S001)

### 7. 特記事項

<span style="color:green">[ADD]</span> Pine Script の仕様上、インジケーター宣言はグローバルスコープに配置します。

## S002 入力設定

### 1. 処理概要

<span style="color:green">[ADD]</span> [S002] EMA 表示に必要な価格ソース、期間、色、線幅を入力として受け取ります。

### 2. メイン関数

<span style="color:green">[ADD]</span> [F101] `refreshEmaSeries` が入力値を受け取って EMA 算出に使用します。

### 3. 引数

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [I105] | ema1ColorInput | color | EMA1 の表示色 |
| <span style="color:green">[ADD]</span> [I106] | ema2ColorInput | color | EMA2 の表示色 |
| <span style="color:green">[ADD]</span> [I107] | ema3ColorInput | color | EMA3 の表示色 |
| <span style="color:green">[ADD]</span> [I108] | lineWidthInput | int | EMA ラインの太さ |

### 4. 戻り値

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [O104] | inputSettings | inputs | TradingView 設定画面で編集可能な入力群 |

### 5. メイン変数

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [V102] | ema1LengthInput | int | EMA1 期間 |
| <span style="color:green">[ADD]</span> [V103] | ema2LengthInput | int | EMA2 期間 |
| <span style="color:green">[ADD]</span> [V104] | ema3LengthInput | int | EMA3 期間 |
| <span style="color:green">[ADD]</span> [V105] | ema1ColorInput | color | EMA1 色 |
| <span style="color:green">[ADD]</span> [V106] | ema2ColorInput | color | EMA2 色 |
| <span style="color:green">[ADD]</span> [V107] | ema3ColorInput | color | EMA3 色 |
| <span style="color:green">[ADD]</span> [V108] | lineWidthInput | int | 線幅 |

### 6. 処理フロー

- <span style="color:green">[ADD]</span> [L102] `input.source`, `input.int`, `input.color` で設定項目を定義します。 (S002)

### 7. 特記事項

<span style="color:green">[ADD]</span> 期間と線幅は `minval=1` を指定し、異常値入力を抑制します。

## S003 EMA 計算

### 1. 処理概要

<span style="color:green">[ADD]</span> [S003] 入力された価格ソースと期間から3本の EMA を計算します。

### 2. メイン関数

<span style="color:green">[ADD]</span> [F101] `refreshEmaSeries` が3本分の EMA を返却します。

### 3. 引数

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [I101] | sourceInput | series float | EMA 計算に使用する価格ソース |
| <span style="color:green">[ADD]</span> [I102] | ema1LengthInput | int | EMA1 の期間 |
| <span style="color:green">[ADD]</span> [I103] | ema2LengthInput | int | EMA2 の期間 |
| <span style="color:green">[ADD]</span> [I104] | ema3LengthInput | int | EMA3 の期間 |

### 4. 戻り値

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [O101] | ema1Value | series float | EMA1 の値 |
| <span style="color:green">[ADD]</span> [O102] | ema2Value | series float | EMA2 の値 |
| <span style="color:green">[ADD]</span> [O103] | ema3Value | series float | EMA3 の値 |

### 5. メイン変数

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [V109] | ema1Value | series float | EMA1 計算結果 |
| <span style="color:green">[ADD]</span> [V110] | ema2Value | series float | EMA2 計算結果 |
| <span style="color:green">[ADD]</span> [V111] | ema3Value | series float | EMA3 計算結果 |

### 6. 処理フロー

- <span style="color:green">[ADD]</span> [L103] [T101] `selectEmaValue` を3回呼び出し、EMA1/EMA2/EMA3 を計算します。 (S003)

### 7. 特記事項

<span style="color:green">[ADD]</span> [T101] `selectEmaValue` は `ta.ema` のラッパー関数です。

## S004 EMA 表示

### 1. 処理概要

<span style="color:green">[ADD]</span> [S004] 計算した EMA を3本のラインとしてチャート上に表示します。

### 2. メイン関数

<span style="color:green">[ADD]</span> [F101] `refreshEmaSeries` の戻り値を `plot` に渡します。

### 3. 引数

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [I105] | ema1ColorInput | color | EMA1 の表示色 |
| <span style="color:green">[ADD]</span> [I106] | ema2ColorInput | color | EMA2 の表示色 |
| <span style="color:green">[ADD]</span> [I107] | ema3ColorInput | color | EMA3 の表示色 |
| <span style="color:green">[ADD]</span> [I108] | lineWidthInput | int | EMA ラインの太さ |

### 4. 戻り値

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [O105] | plotEma1 | plot | EMA1 ライン |
| <span style="color:green">[ADD]</span> [O106] | plotEma2 | plot | EMA2 ライン |
| <span style="color:green">[ADD]</span> [O107] | plotEma3 | plot | EMA3 ライン |

### 5. メイン変数

| ID | 名称 | 型 | 説明 |
|---|---|---|---|
| <span style="color:green">[ADD]</span> [V109] | ema1Value | series float | EMA1 表示値 |
| <span style="color:green">[ADD]</span> [V110] | ema2Value | series float | EMA2 表示値 |
| <span style="color:green">[ADD]</span> [V111] | ema3Value | series float | EMA3 表示値 |

### 6. 処理フロー

- <span style="color:green">[ADD]</span> [L104] `plot` を3回呼び出し、EMA1/EMA2/EMA3 を表示します。 (S004)

### 7. 特記事項

<span style="color:green">[ADD]</span> `plot` は Pine Script の制約に従い、グローバルスコープで呼び出します。

## 補助関数

<span style="color:green">[ADD]</span> [T101] `selectEmaValue` は、指定された価格ソースと期間から EMA を1本計算します。

## コンパイル・ダイジェスト

| タグ | 件数 | 内容（要約） |
|---|---:|---|
| [ADD] | 54 | S/L/F/T/I/O/V の全項目を新規追加 |
| [MOD] | 0 | なし |
| [DEL] | 0 | なし |
| [KEEP] | 0 | 初版のためなし |
