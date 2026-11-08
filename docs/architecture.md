# 構成と境目

## リポジトリ

| リポジトリ | 役割 | 技術 |
| --- | --- | --- |
| `qata`（親） | 目的・スコープ・デモの完了条件・契約・共通の作り方（rules / skills）・ideas | Markdown、JSON Schema |
| `qata-runner` | Windows 上で操作を実行し、結果とスクショを返す | Python、pywinauto、FastAPI |
| `qata-server` | 部品の辞書の提供、テストケースの検証・登録、実行の指揮、結果の保存。入口は REST と MCP | Python、FastAPI、pydantic、MCP の Python SDK、SQLite |
| `qata-web` | テストケースの確認・管理、実行、結果の閲覧 | TypeScript、React（Vite） |

- runner と server は、動く場所と依存が違うので分ける。
- **server は LLM を呼ばない。** テストケースを考えるのは MCP の向こうの AI で、server は道具と関門だけを持つ（内製ツールとして、使う環境の AI に考える部分を任せる方針）。
- server の機能は application 層に置き、REST（web 用）と MCP（AI 用）の二つの入口を interface 層に薄く置く。

部品のつながり：

```
AI（PoC では Claude）──MCP──┐
                            ├─→ qata-server ──命令──→ qata-runner ──操作──→ 電卓
qata-web ─────────REST──────┘                ←─結果・スクショ─┘
```

## デモの流れ

1. Claude に日本語で頼む
2. Claude が MCP で部品の辞書を読み、テストケースを作って登録する
3. 実行する
4. 電卓が動く
5. web で結果とスクショを見る

## ローカルのフォルダ

```
作業フォルダ/             ← ただのフォルダ（リポジトリではない）。名前は自由
├── qata/                 ← 親
├── qata-runner/
├── qata-server/
└── qata-web/
```

- 共通のルールや手順は、必ず親リポジトリの中に置く（作業フォルダ直下には置かない）。
- 各部品の CLAUDE.md には「契約と全体の要件は `../qata/` を見る」と書く。

## 境目（契約）

部品の間でやり取りするデータの形を契約とし、親の [contracts/](../contracts/) に JSON Schema で置く。**正は親だけ。** 中身と流れは [contracts/README.md](../contracts/README.md) を見る。

契約の一覧（PoC の間は仮に確定）：

- テストケースの形式（手順・確認事項）
- 画面の部品の辞書の形式
- runner への命令と、runner からの結果（スクショを含む）
- server の API（親の `openapi.yaml` が正。web はここから型を生成し、server は FastAPI が出す OpenAPI がこれと合うことをテストで確かめる）
- server の MCP の道具（名前・入力・出力）

各部品は契約から型（pydantic / TypeScript）を生成し、生成物もコミットする。生成物が契約と一致しているかをテストで確かめる。

## 未決事項

- `qata` の名前が GitHub / PyPI / npm で使われていないかの確認
- デモで使う MCP のクライアント（Claude Desktop か Claude Code か）
