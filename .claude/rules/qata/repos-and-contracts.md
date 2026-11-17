# qata のリポジトリと契約

構成の全体は `docs/architecture.md` を見る。ここには、作業で守ることだけを書く。

## リポジトリ

- 親 `qata` と、部品 `qata-runner`・`qata-server`・`qata-web` は、それぞれ別のリポジトリにする。作業フォルダはリポジトリにしない。
- 共通のルールや手順は、親の中に置く。部品のリポジトリや作業フォルダの直下には置かない。
- 親は `qata`、子は `qata-runner`・`qata-server`・`qata-web`（親と子の決まりは `general/parent-and-children.md`）。
- 部品の CLAUDE.md には「契約と全体の要件は `../qata/` を見る」と書く。全体に関わる issue は親のリポジトリの issue に、idea は `../qata/docs/ideas.md` に上げる。

## 契約

- 部品の間でやり取りするデータの形は、親に JSON Schema で置く。正は親だけ。
- 部品は契約から型（pydantic / TypeScript）を生成し、生成物もコミットする。手で写さない。
- 生成物が、読む版の契約と一致しているかを、テストで確かめる。

## 契約の版

版の上げ方は `general/git.md` の「版の上げ方」に従う。親が正、子（runner・server・web）が読む側。

- 版は `contracts/VERSION` に書き、マージしたら `contracts-v<版>` のタグを付ける（例：`contracts-v1.5.0`）。変更の記録は `contracts/CHANGELOG.md`。
- 子は、読む版を自分のリポジトリに持ち、型の生成・テスト・CI・実行のどれも、その版の契約を読む。隣に並べた親（`../qata/`）の、その時の中身は読まない。
- web の端から端までのテストは server と runner を動かすので、server と runner を先に上げる。
- 版の固定を入れる作業は [qata#69](https://github.com/masakazu2025/qata/issues/69)。入るまでは、`general/git.md` の「版の固定がまだ入っていない読む側では」に従う。

## server

- server は LLM を呼ばない。テストケースを考えるのは MCP の向こうの AI で、server は道具と関門だけを持つ。
