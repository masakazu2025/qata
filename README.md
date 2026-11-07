# qata

Windows のデスクトップアプリ（最終的には POS 端末）に対して、日本語で書いた手順からテストを作り、自動で再生し、結果を確認できるツールです。

個人の OSS として開発しています。

## 目的

1. **ツール本体**：日本語の手順と確認事項からテストケースを作り、登録・実行し、結果を閲覧できるようにする。
2. **開発の仕組み**：AI に要件 → 設計 → 実装 → テストを回させる仕組み（rules・skills・関門）を作る。ツール本体と同じくらい、これも成果物です。

## いまの段階

PoC です。**Windows の電卓に対して、日本語の手順から、操作の自動実行、結果のスクショ表示まで、一本通るデモ** を目指しています。

## リポジトリ

| リポジトリ | 役割 | 技術 |
| --- | --- | --- |
| `qata`（ここ） | 目的・スコープ・デモの完了条件・契約・共通の作り方（rules / skills）・ideas | Markdown、JSON Schema |
| `qata-runner` | Windows 上で操作を実行し、結果とスクショを返す | Python、pywinauto、FastAPI |
| `qata-server` | 部品の辞書の提供、テストケースの検証・登録、実行の指揮、結果の保存。入口は REST と MCP | Python、FastAPI、pydantic、MCP の Python SDK、SQLite |
| `qata-web` | テストケースの確認・管理、実行、結果の閲覧 | TypeScript、React（Vite） |

部品のリポジトリはまだありません。部品の間の契約と全体の要件は、このリポジトリが正です。

## 文書

- [docs/vision.md](docs/vision.md)：構想（全体像）
- [docs/scope.md](docs/scope.md)：今回のスコープとデモの完了条件
- [docs/architecture.md](docs/architecture.md)：構成と境目（契約）
- [docs/process.md](docs/process.md)：開発の進め方
- [docs/decisions.md](docs/decisions.md)：決めたことと、その理由
- [docs/log.md](docs/log.md)：作業の記録
- [docs/ideas.md](docs/ideas.md)：あとで考えること
