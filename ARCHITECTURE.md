# システム構成

対象: mainのv9。文書整備前のプログラム構成は変更していない。

## ディレクトリ

```text
InediaInfectedAI_Config_Editor_v9.html
README.md
AGENTS.md
SPEC.md
ARCHITECTURE.md
TEST_PLAN.md
TEST_RESULTS.md
CHANGELOG.md
ROADMAP.md
docs/reference/
```

docs/referenceには引き継ぎ書と外部v9.2の記録を置く。v9.2の実行HTML・辞書・試験コードはmainへ導入していない。

## 主要要素（すべてHTML内）

| 要素 | 主な責務・依存 |
|---|---|
| DEFAULT_CONFIG / data | 内蔵値・編集データ。structuredClone/JSON使用 |
| JP_META | 一部キーの日本語メタデータ |
| render / renderObject / field / makeControl | dataからDOMを作り、イベントを設定 |
| getAt / setAt / commitPath | パス経由でdataを参照・更新、保存を呼ぶ |
| saveLocal / restoreLocal | localStorageへの保存と復元 |
| file changeハンドラー | File.text/JSON.parse、data入替、再描画 |
| saveLocalボタン / export | JSON→Blob→Object URL→ダウンロード |
| decorateJapaneseGuides | DOMテキストから説明の挿入位置を探索 |
| enhanceEditableLists / pathForPrimitiveArray | textareaからリストUIを後付け。配列内容一致探索にD01 |
| applySearch | キー・パスを使ったフィールド表示の切替 |

## 依存関係とデータの流れ

renderは後段で2回ラップされ、0msと10msの遅延処理で説明・リストUIを追加する。後付けUIは同じdataとlocalStorageを共有するため、再描画・保持参照・イベント順序が相互に影響する。

設定入力→setAtまたはcommitPath→data→saveLocal→localStorage。
JSON読込→data→保存→render。復元→data→render。出力→data→JSON→Blobダウンロード。

## 起動・ビルド・配布

ビルドなし。HTMLをブラウザで直接開く。サーバーやDBは不要。外部ライブラリ・npm依存・依存管理ファイルは現mainにない。Browser Web APIを実行環境として使う。

出力設定JSONはエディターと別のユーザーデータ。サーバー適用は利用者が行う。mainへ含めない。

## 外部v9.2との差分

v9.2は単一HTMLを維持しつつ、同期描画・パス別説明・言語JSONを追加した別成果物。19件の模擬DOM試験はNode標準モジュール（fs/vm/assert）による。v9.2のコードを導入するまで、mainの起動手順にnpm testがあるとは記載しない。
