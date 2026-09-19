# InediaInfectedAI Config Editor

InediaInfectedAI の設定ファイル `InediaInfectedAIConfig.json` を、ブラウザ上で編集するための非公式GUIエディターです。

JSONを直接編集することなく、ON/OFF、数値、感染者ごとの個別設定などを画面上から変更できます。

> [!IMPORTANT]
> このツールは **InediaInfectedAI の非公式ツール**です。
> InediaInfectedAI本体およびその開発者とは直接の関係はありません。

---

## Features

### ブラウザだけで動作

HTMLファイル単体で動作します。

インストールやWebサーバーは必要ありません。

1. HTMLファイルをダウンロード
2. ブラウザで開く
3. 設定を変更
4. JSONを保存

基本的にはこれだけで使用できます。

---

### 公式設定をベースに編集

InediaInfectedAIの設定構造をGUI上に表示し、各パラメータを編集できます。

例えば、

```json
"SpeedLimitInCalmMode": {
    "all": 3.0
}
```

のような設定を、JSONを直接書き換えずに変更できます。

---

### 対象別設定 / Override

`all` だけでなく、感染者のクラスやグループごとに異なる値を指定できます。

例えば、

```json
"SpeedLimitInCalmMode": {
    "all": 3.0,
    "ZmbM__Runner_Base": 2.1,
    "ZmbF_Runner_Base": 2.1,
    "highstr": 3.0
}
```

のような設定をGUIから追加できます。

`all` は全対象に適用する基本値です。

必要に応じて、

* Class Name
* Group Name

などを追加し、個別の値を設定できます。

---

## 主な編集項目

InediaInfectedAIConfig.json に含まれる設定を編集できます。

代表的な項目：

* Search
* Speed
* Smells
* Long Vision
* Return To Spawn
* Jump
* Fall
* Injury
* Vehicle Attack
* Reactions
* Damage To Player
* Damage To Zombie
* Diseases
* Pain
* Dodge
* Jump Attack
* Blood
* Crawling
* Scream
* Search Body
* Butchering
* Backstab
* Vision
* Size
* Throwing Projectiles
* Players
* Vehicles

設定項目は英語名を残しながら、一部に日本語の補助説明を表示します。

---

## リスト編集

クラス名やアイテム名などを複数登録する設定では、JSON配列を直接編集する代わりに、GUIから項目を追加・変更・削除できます。

例えばWikiに、

> For example, you add "Ghillie Hood" to this list:

のように記載されているタイプの設定についても、リストとして編集できるようにしています。

---

## JSONの読み込み

既存の `InediaInfectedAIConfig.json` を読み込んで編集できます。

エディター上部の

**JSONを開く**

からファイルを選択してください。

読み込んだ内容がGUIへ反映されます。

---

## 設定の保存

編集後、

**設定を保存（JSONダウンロード）**

を押してください。

現在の設定から、

```text
InediaInfectedAIConfig.json
```

が生成され、ブラウザからダウンロードされます。

生成されたJSONを確認したうえで、DayZサーバー側の設定ファイルとして使用してください。

---

## ブラウザへの一時保存

編集中の設定はブラウザの `localStorage` に保存できます。

そのため、JSONとして書き出す前の編集状態をブラウザ内に保持することもできます。

ただし、ブラウザデータを削除すると保存内容も消える可能性があります。

重要な設定は必ずJSONとして保存してください。

---

## 公式初期設定に戻す

**公式初期設定に戻す**

を使用すると、エディターに内蔵されている初期設定へ戻すことができます。

誤って設定を変更した場合や、新しい設定を最初から作り直したい場合に使用してください。

---

## 使用例

例えば感染者の通常時速度設定が、

```json
"SpeedLimitInCalmMode": {
    "all": 3.0
}
```

となっている場合、GUIから対象を追加して、

```json
"SpeedLimitInCalmMode": {
    "all": 3.0,
    "ZmbM__Runner_Base": 2.1,
    "ZmbF_Runner_Base": 2.1
}
```

のようなJSONを作成できます。

同様に `all` を基本値として持つ対応設定では、対象別の値を追加できます。

---

## 対応ブラウザ

比較的新しいブラウザでの使用を想定しています。

* Google Chrome
* Microsoft Edge
* Firefox

HTML単体で動作するため、基本的にインターネット接続は必要ありません。

---

## 注意事項

このツールで生成した設定ファイルを実際のサーバーへ導入する前に、元の `InediaInfectedAIConfig.json` をバックアップしてください。

InediaInfectedAIのアップデートによって設定項目や仕様が変更された場合、このエディターが最新仕様に対応していない可能性があります。

また、設定値によってはゲームバランスやサーバーパフォーマンスに影響する可能性があります。

生成されたJSONの内容を確認してから使用してください。

---

## InediaInfectedAI

このツールはInediaInfectedAIの設定を編集しやすくするために作成した非公式エディターです。

InediaInfectedAI本体、最新情報、設定仕様については公式GitHubおよびWikiを確認してください。

### Official Project

InediaInfectedAI
https://github.com/ysaroka/InediaInfectedAI

### Official Wiki

https://github.com/ysaroka/InediaInfectedAI/wiki

---

## Credits

### InediaInfectedAI

Original Mod / Configuration System:

**ysaroka / InediaInfectedAI**

InediaInfectedAIの開発者に感謝します。

このConfig EditorはInediaInfectedAIの設定作業を簡単にすることを目的として作成された非公式ツールです。

---

## Disclaimer

This project is an unofficial configuration editor for InediaInfectedAI.

This project is not affiliated with or endorsed by the original InediaInfectedAI developer.

InediaInfectedAI and related names belong to their respective authors.

Always back up your original configuration files before using generated configuration files.

---

## License

このエディター自体のライセンスについては、リポジトリ公開時に設定してください。

InediaInfectedAI本体のコード・設定・名称等については、InediaInfectedAI側のライセンスおよび利用条件に従ってください。
