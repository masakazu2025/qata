# 契約の変更の記録

契約の版ごとに、何を変えたかを書きます。新しいものを上に足します。

- 版の上げ方は [git.md](../.claude/rules/general/git.md) の「版の上げ方」、版の置き場所とタグは [repos-and-contracts.md](../.claude/rules/qata/repos-and-contracts.md) の「契約の版」にあります。
- 一項目に、版・何を変えたか・変わり方（壊す／足す／説明だけ）・読む側がすることを書きます。

---

## 1.0.0

- **何を変えたか：** 最初の版。版を付ける前（2026-11-18 の時点）の契約を、そのまま 1.0.0 とする。テストケースのタグとタグでの絞り込み（[qata#60](https://github.com/masakazu2025/qata/issues/60)）までを含む。
- **変わり方：** —（最初の版）
- **読む側がすること：** 版の固定に移る（runner は [qata-runner#18](https://github.com/masakazu2025/qata-runner/issues/18)、server は [qata-server#27](https://github.com/masakazu2025/qata-server/issues/27)、web は [qata-web#28](https://github.com/masakazu2025/qata-web/issues/28)）。中身は 1.0.0 になる前と同じなので、型の作り直しは要らない。
