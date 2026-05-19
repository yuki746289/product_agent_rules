# ルール実行確認

## 目的

  `.rules_v6` の app / batch / quick_fix ルールを、手順通りに実行できるか確認します。
  app / batch / quick_fix の各手順が正しく動作しているか確認します。
  ルール改修後に、参照順序、成果物、確認タイミング、仕様書出力先に矛盾がないことを確認します。

## 前提

  - 起点ルールは `.rules/dev/rules_dev.md` とする。
  - 共通ルールとして `.rules/rules_common.md` を参照する。
  - 追加ルールが必要な場合は `.rules/dev/rules_dev_other.md` を参照する。
  - 手順ごとに、参照ルールと参照ファイルを提示してから作業する。
  - 各手順が終わった時点でユーザ確認を取る。
  - 未承認の途中成果物、他工程の未使用ルールは参照しない。
  - 仕様書および仕様確認結果は、プロジェクト直下の `.document` フォルダに格納する。
  - `.document` フォルダが存在しない場合は、仕様書または仕様確認結果を作成する前に作成する。
  - `.document` フォルダはGit管理対象とし、空フォルダとして保持する必要がある場合は `.document/.gitkeep` を作成する。

## 共通確認

  - [ ] 作業開始前に、参照するルールファイルを提示できること。
  - [ ] 対象バージョンの格納フォルダを作成できること。
  - [ ] 成果物の格納先が、手順1〜7または手順1〜3と、仕様書系で分かれていること。
  - [ ] 各手順の完了時に、ユーザ確認を取る流れになっていること。
  - [ ] 各ファイル作成後、該当する `.rules/log/` 配下のログへ記述する流れになっていること。
  - [ ] 参照可 / 参照不可の制約に矛盾がないこと。
  - [ ] 処理中に矛盾が見つかった場合、作業を停止してユーザへ報告する流れになっていること。

## 確認工程の内容確認

  ここでいう確認工程とは、ルールファイル名または成果物ファイル名に `_confirm` を含む工程を指します。
  例: 基本設計書確認、詳細設計書確認、コード確認、仕様確認。
  これらの確認工程では、成果物の存在確認だけでなく、成果物の中身を確認します。

  - [ ] 成果物ファイルが存在すること。
  - [ ] 成果物の章構成が、該当ルールの指定に沿っていること。
  - [ ] 必須項目が存在し、空欄または未記入のまま放置されていないこと。
  - [ ] 前工程の成果物と内容が対応していること。
  - [ ] 変更内容、差分、確認結果が具体的に記述されていること。
  - [ ] 不要な追加、不要な省略、先祖返りがないこと。
  - [ ] 未確認事項、未実施の確認、推測している内容が区別されていること。
  - [ ] 該当する確認ルールに記載された全チェック項目を確認していること。
  - [ ] チェック漏れがないこと。
  - [ ] 未確認のチェック項目がある場合、未確認理由を明記していること。
  - [ ] NG のチェック項目がある場合、NG 理由、修正対象、再確認要否を明記していること。
  - [ ] OK / NG / 保留の判定が明記されていること。
  - [ ] NG または保留の場合、修正が必要なルールまたは成果物が明記されていること。
  - [ ] NG または未確認が残っている場合、OK 判定にしていないこと。

## app 手順確認

### 対象ルール

  - `.rules/dev/app/rules_app_1_request.md`
  - `.rules/dev/app/rules_app_2_base.md`
  - `.rules/dev/app/rules_app_3_base_confirm.md`
  - `.rules/dev/app/rules_app_4_detail.md`
  - `.rules/dev/app/rules_app_5_detail_confirm.md`
  - `.rules/dev/app/rules_app_6_code.md`
  - `.rules/dev/app/rules_app_7_code_confirm.md`
  - `.rules/dev/app/rules_app_8_spec.md`
  - `.rules/dev/app/rules_app_9_spec_confirm.md`

### 実行確認

  - [ ] 手順1で `{filename}_v{n}.{m}_1_request.md` を作成できること。
  - [ ] 手順2で `{filename}_v{n}.{m}_2_base.md` を作成できること。
  - [ ] 手順3で `{filename}_v{n}.{m}_3_base_confirm.md` を作成できること。
  - [ ] 手順3で基本設計書の中身を確認できること。
  - [ ] 手順4で `{filename}_v{n}.{m}_4_detail.md` と `{filename}_v{n}.{m}_4_detail.json` を作成できること。
  - [ ] 手順5で `{filename}_v{n}.{m}_5_detail.json` を作成できること。
  - [ ] 手順5で詳細設計書と詳細設計JSONの中身を確認できること。
  - [ ] 手順6で `{filename}_v{n}.{m}.{code_ext}` を作成できること。
  - [ ] 手順7で `{filename}_v{n}.{m}_7_code_confirm.md` を作成できること。
  - [ ] 手順7でコードと詳細設計JSONの対応、ID、実装範囲、動作確認結果を確認できること。
  - [ ] 手順8で `.document/仕様_差分_v{n}.{m}.md` と `.document/仕様_全体_v{n}.{m}.md` を作成できること。
  - [ ] 手順9で `.document/仕様確認_v{n}.{m}.md` を作成できること。
  - [ ] 手順9で仕様書の中身、前バージョンからの継承、実装結果との対応を確認できること。
  - [ ] 手順8〜9で `.document` フォルダが存在しない場合に作成できること。
  - [ ] app 追加ルールが必要な工程で `.rules/dev/rules_dev_other.md` を参照できること。

## batch 手順確認

### 対象ルール

  - `.rules/dev/batch/rules_batch_1_request.md`
  - `.rules/dev/batch/rules_batch_2_base.md`
  - `.rules/dev/batch/rules_batch_3_base_confirm.md`
  - `.rules/dev/batch/rules_batch_4_detail.md`
  - `.rules/dev/batch/rules_batch_5_detail_confirm.md`
  - `.rules/dev/batch/rules_batch_6_code.md`
  - `.rules/dev/batch/rules_batch_7_code_confirm.md`
  - `.rules/dev/batch/rules_batch_8_spec.md`
  - `.rules/dev/batch/rules_batch_9_spec_confirm.md`

### 実行確認

  - [ ] 手順1で `{filename}_v{n}.{m}_1_request.md` を作成できること。
  - [ ] 手順2で `{filename}_v{n}.{m}_2_base.md` を作成できること。
  - [ ] 手順3で `{filename}_v{n}.{m}_3_base_confirm.md` を作成できること。
  - [ ] 手順3で基本設計書の中身を確認できること。
  - [ ] 手順4で `{filename}_v{n}.{m}_4_detail.md` と `{filename}_v{n}.{m}_4_detail.json` を作成できること。
  - [ ] 手順5で `{filename}_v{n}.{m}_5_detail.json` を作成できること。
  - [ ] 手順5で詳細設計書と詳細設計JSONの中身を確認できること。
  - [ ] 手順6で `{filename}_v{n}.{m}.{code_ext}` を作成できること。
  - [ ] 手順7で `{filename}_v{n}.{m}_7_code_confirm.md` を作成できること。
  - [ ] 手順7でコードと詳細設計JSONの対応、ID、実装範囲、動作確認結果を確認できること。
  - [ ] 手順8で `.document/仕様_差分_v{n}.{m}.md` と `.document/仕様_全体_v{n}.{m}.md` を作成できること。
  - [ ] 手順9で `.document/仕様確認_v{n}.{m}.md` を作成できること。
  - [ ] 手順9で仕様書の中身、前バージョンからの継承、実装結果との対応を確認できること。
  - [ ] 手順8〜9で `.document` フォルダが存在しない場合に作成できること。

## quick_fix 手順確認

### 対象ルール

  - `.rules/dev/quick_fix/rules_quick_fix_1_request.md`
  - `.rules/dev/quick_fix/rules_quick_fix_2_code.md`
  - `.rules/dev/quick_fix/rules_quick_fix_3_code_confirm.md`
  - `.rules/dev/quick_fix/rules_quick_fix_4_spec.md`
  - `.rules/dev/quick_fix/rules_quick_fix_5_spec_confirm.md`

### 実行確認

  - [ ] 手順1で `{filename}_v{n}.{m}_1_request.md` を作成できること。
  - [ ] 手順2で `{filename}_v{n}.{m}.{code_ext}` を作成できること。
  - [ ] 手順3で `{filename}_v{n}.{m}_3_code_confirm.md` を作成できること。
  - [ ] 手順3でコード修正内容、影響範囲、動作確認結果、仕様書作成の要否を確認できること。
  - [ ] 手順3で仕様への影響の有無を確認できること。
  - [ ] 手順3で仕様書作成の要否を記述できること。
  - [ ] 仕様影響がある場合、手順4で `.document/仕様_差分_v{n}.{m}.md` と `.document/仕様_全体_v{n}.{m}.md` を作成できること。
  - [ ] 仕様影響がある場合、手順5で `.document/仕様確認_v{n}.{m}.md` を作成できること。
  - [ ] 仕様影響がある場合、手順5で仕様書の中身、前バージョンからの継承、実装結果との対応を確認できること。
  - [ ] 仕様影響がない場合、手順3に仕様書作成を省略する理由を明記できること。
  - [ ] 仕様影響がない場合、手順3で仕様影響なしの根拠を確認できること。
  - [ ] 仕様影響がない場合、手順4および手順5を省略することをユーザへ報告できること。
  - [ ] 仕様影響が不明な場合、仕様書作成を省略せず手順4へ進む判断ができること。
  - [ ] quick_fix の範囲を超える場合、通常開発へ切り替える必要があることをユーザへ報告できること。

## 判定

  以下をすべて満たす場合、実行確認OKとします。

  - [ ] app の手順1〜9を順番に実行できる。
  - [ ] batch の手順1〜9を順番に実行できる。
  - [ ] quick_fix の手順1〜3を順番に実行できる。
  - [ ] quick_fix で仕様影響がある場合、手順4〜5を実行できる。
  - [ ] quick_fix で仕様影響がない場合、手順4〜5を省略できる。
  - [ ] `.document` フォルダの作成とGit管理ルールに矛盾がない。
  - [ ] 参照ルール、成果物、ユーザ確認、ログ出力の流れに矛盾がない。
  - [ ] すべての確認工程でチェック漏れがない。
  - [ ] NG が 0 件である、または NG の修正と再確認が完了している。
  - [ ] 未確認項目が 0 件である。
  - [ ] 保留項目がある場合、OK ではなく保留判定にしている。

## 確認結果

  - 判定: OK / NG / 保留
  - 確認日:
  - 確認者:
  - チェック漏れ: なし / あり
  - NG 件数:
  - 保留件数:
  - 未確認件数:
  - 再確認要否: 要 / 不要
  - 修正が必要なルールファイル:
  - 未確認事項:
  - ユーザ確認事項:
