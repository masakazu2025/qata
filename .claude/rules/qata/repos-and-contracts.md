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

### 子の持ち方

三つの子で同じ形にする（D54）。

| もの | 形 |
| --- | --- |
| 写し | 親のタグ `contracts-v<版>` の `contracts/` を、子のリポジトリの直下の `qata-contracts/` に丸ごと写してコミットする。`VERSION` と `CHANGELOG.md` も含む |
| 読む版 | 写しの中の `qata-contracts/VERSION`。版をほかのファイルに書かない |
| 写す道具 | 子のスクリプト `scripts/sync_contracts`（拡張子はその子の言語）。版を引数に取り、GitHub からそのタグの tarball（`https://github.com/masakazu2025/qata/archive/refs/tags/contracts-v<版>.tar.gz`）を取って `qata-contracts/` を置き換える。版を上げるときは、これを回してから型を生成し直す |
| 読む場所 | 型の生成・テスト・実行のどれも `qata-contracts/` を読む。`QATA_PARENT_DIR` と `../qata/contracts/` は契約には使わない |
| 手で直していない確かめ | CI で、写す道具を `qata-contracts/VERSION` の版で一時の場所に回し、`qata-contracts/` と同じことを確かめる。違えば落とす。手元では、写す道具に `--check` を付けると同じことをする |
| 最新の版の知り方 | `git ls-remote --tags https://github.com/masakazu2025/qata 'contracts-v*'` で親のタグを並べ、一番大きい版を取る。認証は要らない |
| 古いときの警告 | CI で、読む版が最新より古ければ `::warning::` で知らせる。関門は落とさない。タグが取れないとき（つながらないなど）も、警告だけにする |
| 警告の確かめ | 版を比べる部分を、引数（読む版・タグの並び）を取る関数にし、テストで確かめる。版は数として比べる（`1.10.0` は `1.9.0` より大きい） |

- web の e2e が立ち上げる server と runner は、それぞれ自分の `qata-contracts/` を読む。web から契約の場所を渡さない。
- web のデモのデータ（親の例の写し）は、`qata-contracts/examples/` と同じことを確かめる。

## server

- server は LLM を呼ばない。テストケースを考えるのは MCP の向こうの AI で、server は道具と関門だけを持つ。
