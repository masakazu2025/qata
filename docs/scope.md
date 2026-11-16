# いまのスコープ

**管理の拡張**（[roadmap.md](roadmap.md)）。

中身はやるときに吟味します。候補は roadmap.md の付録の機能の棚卸しにあります。やらなかったものは、あとに回します。

スコープの外のものに備えた抽象化・フォルダ・コードは作りません。思いついたことは [ideas.md](ideas.md) に書きます。

## 1 周目：タグと絞り込み

ボードの Next にある土台の入れ替え（保存の SQLAlchemy と Alembic、web の React Router・TanStack Query・Tailwind CSS など）が終わってから始めます（D49）。

届いたと言える状態：テストケースにタグを付けて登録でき、人は web で、AI は MCP で、タグで絞り込んだ一覧を見られる。

| やること | 付録の番号 | issue |
| --- | --- | --- |
| 契約にタグと絞り込みを足す | B2・B4 | [qata#60](https://github.com/masakazu2025/qata/issues/60) |
| server でタグを保存し、REST と MCP で絞り込む | B2・B4・B6 の一部 | [qata-server#22](https://github.com/masakazu2025/qata-server/issues/22) |
| web の一覧と詳細にタグを出し、タグで絞り込む | B2・B4 | [qata-web#22](https://github.com/masakazu2025/qata-web/issues/22) |

やらないもの（2 周目より後）：

- 有効な版（B3）。版の形式と比べ方（v2.1 と v2.10 の大小など）を決めることが多いため。
- クローン（B5）、必須のタグ（B7）。
- 登録したあとにタグを直すこと。web での手直し（A4）と一緒にやる。いまは登録し直す。

1 周目が終わったら振り返り、次に工程ごとの確かめ（[qata#59](https://github.com/masakazu2025/qata/issues/59)）を入れる（人、2026-11-16）。
