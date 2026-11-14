# 開発の進め方

作業で守る決まりはルール（[.claude/rules/](../.claude/rules/)）に、場面ごとの手順はスキル（[.claude/skills/](../.claude/skills/)）に置いています。この文書には、プロジェクト全体の進め方を書きます。

- 汎用のルール：[general/working.md](../.claude/rules/general/working.md)（原則）、[general/git.md](../.claude/rules/general/git.md)（ブランチとコミット）、[general/autonomy.md](../.claude/rules/general/autonomy.md)（AI の裁量、質問の出し方）、[general/rules-and-skills.md](../.claude/rules/general/rules-and-skills.md)（ルールとスキルの置き方）、[general/parent-and-children.md](../.claude/rules/general/parent-and-children.md)（親と子のリポジトリ、issue と idea）、[general/issues.md](../.claude/rules/general/issues.md)（やることの持ち方）
- qata 固有のルール：[qata/repos-and-contracts.md](../.claude/rules/qata/repos-and-contracts.md)
- 汎用のスキル：[session-start](../.claude/skills/session-start/SKILL.md)、[session-end](../.claude/skills/session-end/SKILL.md)、issue を 1 つやる流れ [work-issue](../.claude/skills/work-issue/SKILL.md) と、その工程の [requirements](../.claude/skills/requirements/SKILL.md)・[design](../.claude/skills/design/SKILL.md)・[implement](../.claude/skills/implement/SKILL.md)・[test](../.claude/skills/test/SKILL.md)
- サブエージェント：[reviewer](../.claude/agents/reviewer.md)（置き場所と名前だけ。中身は #23 のあと）

## 順番

1. 親：目的・スコープ・デモの完了条件・契約
2. 全部品を偽物でつなぎ、電卓のデモの流れを端から端まで一本通す
3. 偽物を一つずつ本物に置き換える。置き換えるたびに、端から端までのテストが通り続けることを関門にする

## 人と AI の分担

| 段階 | 担当 |
| --- | --- |
| 目的・ゴールの図式 | 人 |
| 分割・足りないものの洗い出し | AI が案を出す |
| スコープの決定 | 人（AI は案を出す） |
| 契約 | AI が案を出し、人が確認する |
| 契約から下の設計・実装・テスト | AI。関門で機械的に止める |
| 関門をすり抜けた問題の発見と、関門・指示への反映 | 人 |

範囲の中の細かいことは AI が決め、[decisions.md](decisions.md) に残します。人はそれをまとめてレビューします。

## ルールの改定

- 人が関門をすり抜けた問題を見つけたら、それをルールや関門に落とす。
- 落とすときは、ルールかスキルか、汎用か qata 固有かを決めて置く（[general/rules-and-skills.md](../.claude/rules/general/rules-and-skills.md)）。
- ルールを AI が自分で見直して改善する仕組みは、今回のスコープ外です（[scope.md](scope.md)）。

## 振り返り

周回ごとに、次のことを記録する。

- かかった時間
- 人が口を出した回数と、その内容
- rules / skills に落としたもの
- AI が仮に決めたこと（[decisions.md](decisions.md)）のうち、問題が出たもの

口を出した回数が減っていくことを、仕組みが育った指標とする。

記録と数え方は [retro.md](retro.md) に置く。

## Python の部品で使う道具

- pytest-bdd：観点を Gherkin で書き、そのままテストとして動かす
- import-linter：依存の向きを検査する
- ruff
