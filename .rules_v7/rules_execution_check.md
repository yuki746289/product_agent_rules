# ルール実行確認

## 目的

`.rules_v7` の app / batch / quick_fix ルールを、定義された順序で実行できるか確認します。
ルール改修後に、参照順序、成果物、確認タイミングおよび格納先に矛盾がないことを確認します。

## 前提

- 起点ルールは `.rules/dev/rules_dev.md` とする。
- 共通ルールとして `.rules/rules_common.md` を参照する。
- プロジェクト固有ルールとして `.rules/dev/rules_dev_custom.md` を参照する。ファイルが空の場合は追加ルールなしとして扱う。
- 追加ルールが必要な場合は `.rules/dev/rules_dev_other.md` を参照する。
- 手順ごとに参照ルールと参照ファイルを提示し、完了時にユーザ確認を取る。
- 未承認の途中成果物および他工程の未使用ルールは参照しない。
- 通常開発の仕様書と仕様確認結果は、プロジェクト直下の `.document/v{n}.{m}/` フォルダに格納する。

## 共通確認

- [ ] 対象バージョンの格納フォルダを作成できること。
- [ ] 通常開発の手順1〜2と手順3〜10で成果物の格納先が正しく分かれていること。
- [ ] quick_fixの手順1〜3の成果物が対象バージョンの格納フォルダに作成されること。
- [ ] 各手順の完了時にユーザ確認を取ること。
- [ ] 各成果物の作成後に該当するログへ記録すること。
- [ ] 矛盾が見つかった場合、作業を停止してユーザへ報告すること。

## app 手順確認

### 対象ルール

- `.rules/dev/app/rules_app_1_spec.md`
- `.rules/dev/app/rules_app_2_spec_confirm.md`
- `.rules/dev/app/rules_app_3_request.md`
- `.rules/dev/app/rules_app_4_base.md`
- `.rules/dev/app/rules_app_5_base_confirm.md`
- `.rules/dev/app/rules_app_6_detail.md`
- `.rules/dev/app/rules_app_7_detail_confirm.md`
- `.rules/dev/app/rules_app_8_code.md`
- `.rules/dev/app/rules_app_9_code_confirm.md`
- `.rules/dev/app/rules_app_10_final_confirm.md`

### 実行確認

- [ ] 手順1で `.document/v{n}.{m}/`、`00_index.md`、`01_差分仕様.md` およびapp用の全項目別仕様書を作成できること。
- [ ] 前バージョンの確認結果を除く全仕様ファイルをコピーし、未変更仕様も継承できること。
- [ ] 手順2で仕様書だけを対象に、矛盾、欠落、仕様継承および仕様書間の整合性を確認できること。
- [ ] 手順2でインデックスのリンク、仕様ID、全項目別仕様書を確認し、総合判定を記載した `99_仕様確認.md` を出力できること。
- [ ] 手順2で仕様書変更一覧と実際のファイル差分、および `01_差分仕様.md` が一致していることを確認できること。
- [ ] 手順2で設計書やコードとの突合せを行わないこと。
- [ ] 手順3で `{filename}_v{n}.{m}_3_request.md` を作成できること。
- [ ] 手順4で `{filename}_v{n}.{m}_4_base.md` を作成できること。
- [ ] 手順5で `{filename}_v{n}.{m}_5_base_confirm.md` を作成できること。
- [ ] 手順6で `_6_detail.md` と `_6_detail.json` を作成できること。
- [ ] 手順7で `_7_detail.json` を作成できること。
- [ ] 手順8でコードを作成できること。
- [ ] 手順9で `_9_code_confirm.md` を作成できること。
- [ ] 手順10で `_10_final_confirm.md` を作成できること。
- [ ] 手順10で仕様書、要件定義書、設計書、コードおよび各確認結果を突き合わせられること。
- [ ] 手順10で仕様書変更一覧を起点に、変更仕様の実装漏れと `KEEP` 仕様の意図しない変更を確認できること。

## batch 手順確認

### 対象ルール

- `.rules/dev/batch/rules_batch_1_spec.md`
- `.rules/dev/batch/rules_batch_2_spec_confirm.md`
- `.rules/dev/batch/rules_batch_3_request.md`
- `.rules/dev/batch/rules_batch_4_base.md`
- `.rules/dev/batch/rules_batch_5_base_confirm.md`
- `.rules/dev/batch/rules_batch_6_detail.md`
- `.rules/dev/batch/rules_batch_7_detail_confirm.md`
- `.rules/dev/batch/rules_batch_8_code.md`
- `.rules/dev/batch/rules_batch_9_code_confirm.md`
- `.rules/dev/batch/rules_batch_10_final_confirm.md`

### 実行確認

- [ ] 手順1で `.document/v{n}.{m}/`、`00_index.md`、`01_差分仕様.md` およびbatch用の全項目別仕様書を作成できること。
- [ ] 前バージョンの確認結果を除く全仕様ファイルをコピーし、未変更仕様も継承できること。
- [ ] 手順2で仕様書だけを対象に、矛盾、欠落、仕様継承および仕様書間の整合性を確認できること。
- [ ] 手順2でインデックスのリンク、仕様ID、全項目別仕様書を確認し、総合判定を記載した `99_仕様確認.md` を出力できること。
- [ ] 手順2で仕様書変更一覧と実際のファイル差分、および `01_差分仕様.md` が一致していることを確認できること。
- [ ] 手順2で設計書やコードとの突合せを行わないこと。
- [ ] 手順3で `{filename}_v{n}.{m}_3_request.md` を作成できること。
- [ ] 手順4で `{filename}_v{n}.{m}_4_base.md` を作成できること。
- [ ] 手順5で `{filename}_v{n}.{m}_5_base_confirm.md` を作成できること。
- [ ] 手順6で `_6_detail.md` と `_6_detail.json` を作成できること。
- [ ] 手順7で `_7_detail.json` を作成できること。
- [ ] 手順8でコードを作成できること。
- [ ] 手順9で `_9_code_confirm.md` を作成できること。
- [ ] 手順10で `_10_final_confirm.md` を作成できること。
- [ ] 手順10で仕様書、要件定義書、設計書、コードおよび各確認結果を突き合わせられること。
- [ ] 手順10で仕様書変更一覧を起点に、変更仕様の実装漏れと `KEEP` 仕様の意図しない変更を確認できること。

## quick_fix 手順確認

### 対象ルール

- `.rules/dev/quick_fix/rules_quick_fix_1_request.md`
- `.rules/dev/quick_fix/rules_quick_fix_2_code.md`
- `.rules/dev/quick_fix/rules_quick_fix_3_code_confirm.md`

### 実行確認

- [ ] quick_fixが既存仕様を変更しない修正だけを対象としていること。
- [ ] quick_fixで仕様書を作成、変更、削除できないこと。
- [ ] 手順1で修正内容の要件を作成できること。
- [ ] 手順2で承認範囲内のコード修正を行えること。
- [ ] 手順3で要件、既存仕様、コード、影響範囲および動作確認結果を突き合わせられること。
- [ ] 手順3で `99_仕様確認.md` の総合判定が合格である最新バージョンの `00_index.md` と、リンク先の全項目別仕様書を参照できること。
- [ ] 手順3で仕様変更が発生していないことを確認できること。
- [ ] 仕様変更が判明した場合に作業を停止してユーザへ報告できること。
- [ ] 通常開発へ切り替える前にユーザ確認を取ること。
- [ ] ユーザが承認した場合のみ、通常開発を手順1から開始できること。

## 判定

- [ ] appの手順1〜10を順番に実行できること。
- [ ] batchの手順1〜10を順番に実行できること。
- [ ] quick_fixの手順1〜3を順番に実行できること。
- [ ] `.document/v{n}.{m}/` フォルダの作成、前バージョン継承、Git管理ルールに矛盾がないこと。
- [ ] 参照ルール、成果物、ユーザ確認およびログ出力の流れに矛盾がないこと。
- [ ] NGまたは未確認が残っている場合、OK判定にしていないこと。

## 確認結果

- 判定: OK / NG / 保留
- 確認日:
- 確認者:
- NG件数:
- 保留件数:
- 未確認件数:
- 修正が必要なルールファイル:
- ユーザ確認事項:
