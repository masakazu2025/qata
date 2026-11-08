# 契約

部品の間でやり取りするデータの形です。**正はここだけ。** 部品はここから型を生成し、生成物もコミットします（[repos-and-contracts.md](../.claude/rules/qata/repos-and-contracts.md)）。

PoC の間は仮に確定しています。変えるときは [decisions.md](../docs/decisions.md) に残します。

## ファイル

| 契約 | ファイル | 使う部品 |
| --- | --- | --- |
| テストケースの形式 | [schemas/testcase.schema.json](schemas/testcase.schema.json) | server・web・AI（MCP） |
| 画面の部品の辞書の形式 | [schemas/part-dictionary.schema.json](schemas/part-dictionary.schema.json) | server・AI（MCP） |
| runner への命令 | [schemas/runner-command.schema.json](schemas/runner-command.schema.json) | server → runner |
| runner からの結果 | [schemas/runner-result.schema.json](schemas/runner-result.schema.json) | runner → server |
| server の API | [openapi.yaml](openapi.yaml)（形は [schemas/server.schema.json](schemas/server.schema.json)） | web → server |
| server の MCP の道具 | [mcp-tools.json](mcp-tools.json)（形は [schemas/server.schema.json](schemas/server.schema.json)） | AI → server |
| 共通の部品 | [schemas/common.schema.json](schemas/common.schema.json) | すべて |

- JSON Schema は draft 2020-12。ファイルの間は相対パスの `$ref` でつなぐ。
- 例は [examples/](examples/) に置く（電卓で 1 + 2 = 3 を確かめる流れ）。座標は仮の値。
- 部品の辞書の契約は、AI と web に見せる部分（名前・説明・使えるアクション）だけ。名前を具体的な操作に変える規則は server の変換の枠に置く（[D14](../docs/decisions.md)）。

## データの流れ

```
AI ──MCP──→ server：testcase（部品の名前で書く）
                │ 検証：形式（testcase.schema.json）＋ 部品の辞書との照合（部品があり、そのアクションに対応しているか）
                │ 実行：変換の枠で、部品の名前をキー・座標・AutomationId に変える
                ▼
            runner：runner-command → 操作・スクショ → runner-result
                │
                ▼
            server：読んだ文字から値を取り出し（変換の規則）、expected と比べて run を作る
                │   スクショは PNG のファイルに保存し、screenshot_id で指す
                ▼
web ──REST──→ server：testcase_record・run・スクショ
```

## 部品ごとの約束

### runner

- 入口は `POST /runs`（本文 runner-command、応答 runner-result）。実行が終わるまで待って返す。
- 実行のたびにアプリを起動し直し、命令に大きさがあればウィンドウをその大きさにそろえてから始める。
- ステップごとにスクショを撮る。範囲は命令の `screenshot.scope`（ウィンドウだけか、すべての画面か）。
- 判定はしない。読んだ文字をそのまま返す。文字の読み方は `read_text` の `source.method` で選ぶ（いまは `uia` だけ）。

### server

- 検証は二段：形式（JSON Schema）と、部品の辞書との照合。問題は `validation_error` の `code` で返す。
  - 部品がステップのアクションに対応していなければ（`actions` になければ）、`action_not_supported` ではじく。
- 名前を具体的な操作に変える規則は、差し込める枠にする。汎用の規則（キー、座標のタップ、UI Automation で読む）は server に置き、それ以外は内製のツールで足せるようにする。
- スクショの範囲は、アプリごとの設定で決める。
- `needs_review` が true のテストケースは `pending` で登録し、承認されるまで実行しない。false なら `not_required`。
- 確認事項が合わなくても、ステップは最後まで実行する（runner はまとめて実行するため）。runner が止まったら、残りは `skipped`。
- FastAPI が出す OpenAPI が [openapi.yaml](openapi.yaml) と合うこと、MCP で出す道具が [mcp-tools.json](mcp-tools.json) と合うことを、テストで確かめる。

### web

- [openapi.yaml](openapi.yaml) から型を生成する。
