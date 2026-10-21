# 正式仕様と実装状態

基準日: 2026-10-18。対象リポジトリのmainは **v9**、コミット `dbb470cf5be51aaee6b21d6189c1a5adaf83d600`。

## システム概要・基準

DayZ MOD「InediaInfectedAI」の設定JSONをブラウザで編集する非公式ツール。現行コードはInediaInfectedAI_Config_Editor_v9.htmlの単一ファイル。

この会話で作成したv9.2は別の開発成果物で、現mainへ未統合。今回の文書PRで導入しない。mainの現行機能、確定した要求仕様、外部成果物の状態を以下で分ける。実ブラウザ確認済みの正常基準は未確立。

## 状態定義

IMPLEMENTEDはコード上確認、VERIFIEDは明示された範囲で動作確認、PLANNEDは要求確定・対象コードでは未実装、PROPOSEDは検討案、DEFECTは既知不具合、UNKNOWNは確認できない事項。機能全体の合格と一部ロジックの合格は区別する。

## 機能一覧

| ID | 機能・要求 | main v9の状態 | 外部v9.2の状態・根拠 |
|---|---|---|---|
| F01 | 内蔵設定で起動、勝手に設定復元しない | IMPLEMENTED、ブラウザ未確認 | IMPLEMENTED、模擬DOMで確認 |
| F02 | JSON読込・数値/文字列/0・1編集 | IMPLEMENTED、一部DEFECT（D02/D05） | IMPLEMENTED、実ファイル操作は未確認 |
| F03 | 対象別設定の追加・値変更 | IMPLEMENTED、実画面未確認、D02あり | IMPLEMENTED、追加ロジックは模擬DOM確認 |
| F04 | 対象の名前変更・削除、all保護 | PLANNED、v9に専用UIなし | IMPLEMENTED、模擬DOM確認 |
| F05 | 配列の追加・編集・削除とJSON欄同期 | IMPLEMENTED、一部DEFECT（D01/D03） | IMPLEMENTED、一部代表ケースのみ確認 |
| F06 | 自動保存・明示復元・JSONダウンロード | IMPLEMENTED、一部DEFECT（D04） | IMPLEMENTED、実ブラウザ未確認 |
| F07 | 未知キー・複雑な入れ子の保持 | IMPLEMENTED、全構造の往復はUNKNOWN | IMPLEMENTED、代表データを確認 |
| F08 | 内蔵初期値に戻す | IMPLEMENTED、最新公式値との一致はUNKNOWN | IMPLEMENTED、同じく公式値は未照合 |
| F09 | 英語キーと日本語説明を表示 | IMPLEMENTED、一部DEFECT（D06）、全項目ではない | IMPLEMENTED、402設定/424定義 |
| F10 | 言語JSONでラベル・説明・単位差し替え | PLANNED、mainに言語読込なし | IMPLEMENTED、辞書読込等の模擬DOM確認 |
| F11 | 英語キー・パス検索 | IMPLEMENTED、日本語検索はUNKNOWN | IMPLEMENTED、日本語説明検索を含む |
| F12 | 実ブラウザ互換性・安全なサーバー適用 | UNKNOWN | UNKNOWN |

この表でv9.2の模擬DOM確認済みとした部分の試験証跡はTEST_RESULTS.md。main v9のUI機能全体にVERIFIEDは付けない。

## 画面・操作

ヘッダーにJSONを開く、設定を保存、復元、初期化、検証、JSON出力、検索。サイドナビでトップレベルのセクションへ移動。設定はdetailsの階層で表示。

単純な数値/真偽値オブジェクトにallがある場合は対象追加UIを表示。対象名の空文字と既存名は拒否。新規値を数値化して追加し再描画する。v9の既存対象行には名前変更・削除がない。

文字列/数値配列は後付けリストUI、複雑な値はJSON欄。生JSONとリストの双方向同期は要求だがv9にD03がある。対象別追加、再描画、言語機能の受入条件は引き継ぎ書を参照。

## データ構造

- DEFAULT_CONFIG: 内蔵設定。Zombies、Players、Vehiclesと全体設定。
- data: 現在の編集データ。JSON.parseして保持し、JSON.stringifyで出力。
- JP_META: v9のキー名に対する日本語ラベル・説明・単位。
- 対象別値: `Zombies.SpeedLimitInCalmMode = {all:3, highstr:2.1}` 等。
- ブラウザ保存キー: `InediaInfectedAI_Config_Editor_saved_v1`。
- 出力名: InediaInfectedAIConfig.json。サーバーへ直接書き込まない。

外部v9.2にはBUILTIN_LANGUAGEと外部言語JSONがある。現mainのJP_METAと同じ仕様として扱わない。外部辞書の仕様はdocs/reference/v9.2_LANGUAGE_FORMAT.md。

## 処理フロー

起動: 内蔵値を複製→render→日本語説明/リストUIを遅延付加。
編集: コントロール→完全パス更新またはリスト探索→data→localStorage。
読込: ファイル選択→JSON.parse→data入替→保存→再描画。
出力: data→JSON文字列→Blob/Object URL→ブラウザダウンロード→URL解放。
復元: localStorage→JSON.parse→data→再描画。

## 例外と制約

JSON構文エラーはalert等で通知。v9はルート形状を事前検証せず、空数値が0になり、localStorage取得の例外が捕捉範囲外。数値の桁精度・3.0等の字面はJavaScript Number/JSONに従う。ブラウザ保存はJSONのバックアップの代替ではない。

## 対応環境と依存

新しいブラウザを想定。既存READMEにChrome/Edge/Firefoxの記載があるが、対象バージョンでの実操作証拠はない。structuredClone、Array.at、Blob、Object URL、localStorage、File.textを利用。外部JS/CSS依存、ビルド処理、バックエンドは見当たらない。オフラインHTML起動を想定。

## 確定事項

対象別追加・編集・保存・再読込・復元、未知キー保持、リスト編集、日本語補助説明、外部言語差し替えはこの会話と引き継ぎ書で確定した要求。main v9で未達の部分を今回実装しない。

## 不具合と未確定事項

- D01 DEFECT: 同内容の配列を値一致探索し、別のパスを選ぶ。抽出関数で再現。
- D02 DEFECT: 数値を空にするとNumber('')で0を保存する。抽出コントロールで再現。
- D03 DEFECT: JSON欄編集でリストUIの保持参照を更新せず、後のリスト操作が古い配列を戻す経路がある（コード解析、ブラウザ再現は未実施）。
- D04 DEFECT: restoreLocalのgetItemがtryの外で例外を捕捉しない（コード解析）。
- D05 DEFECT: JSONルートの型検証なしでdataへ代入し、null等が描画を壊す経路がある（コード解析）。
- D06 DEFECT: SearchSphereRadiusIncreaseEverySecondsの日本語は時間間隔となっている。外部v9.2で増加距離/秒へ訂正した記録がある。今回Wiki再照合は未実施。
- UNKNOWN: 正常動作版、実ブラウザとMODサーバーの試験、現行公式デフォルトとの一致、配布ライセンス。
- PROPOSED: ソース分割・自動辞書同期・画面全体の多言語化。正式実装計画は未確定。

ユーザーの「正常に動いた」というv9/v9.2固有の報告は、今回参照できたこの会話・資料にはない。
