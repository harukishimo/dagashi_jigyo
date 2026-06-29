---
name: Parent Issue
about: 駄菓子事業計画 Loop Engineering 全体管理用Issue
title: "[Parent] 駄菓子事業計画 初版作成"
labels: "type:parent,evidence:required,agent:review-required"
---

# 親Issue: 駄菓子事業計画 初版作成

## プロジェクト目的

- 駄菓子事業計画書初版を、GitHub Issue駆動・証拠駆動・レビュー駆動で作成する。
- この親Issueでは、事業計画書本文ではなく、Issue運用、エージェント運用、統合ルール、完了条件を管理する。

## 事業の前提

- この事業は、単なる駄菓子販売事業ではない。
- 駄菓子を媒介にして、子どもと地域・社会をつなぐ体験事業として扱う。
- 収益最大化ではなく、実費回収を基本とした地域貢献・社会体験型の活動として扱う。
- 最初は移動式駄菓子屋として小さく始め、実証実験で検証する。

## 最終成果物

- レビュー済みIssueの統合素材をもとにした「駄菓子事業計画書 初版」。
- エビデンス、仮説、要検証、要調査、関連Issue、レビュー履歴を追跡できる状態。

## 子Issue一覧

| No | Issue | Status | Link |
|---|---|---|---|
| 1 | 前提情報・決定事項を抽出する | Backlog |  |
| 2 | Why / 解決したい課題を整理する | Backlog |  |
| 3 | Who: 対象者・関係者を整理する | Backlog |  |
| 4 | What: 提供価値・体験価値を整理する | Backlog |  |
| 5 | How: 実施形態・運営方針を整理する | Backlog |  |
| 6 | 事業環境・利用者・代替手段を分析する | Backlog |  |
| 7 | 初回実証実験の計画案を作成する | Backlog |  |
| 8 | 商品・価格の仮説を作成する | Backlog |  |
| 9 | 運営オペレーション案を作成する | Backlog |  |
| 10 | 収益・費用の考え方を整理する | Backlog |  |
| 11 | 施設側への説明ポイントを整理する | Backlog |  |
| 12 | 想定リスクと対応策を整理する | Backlog |  |
| 13 | 実証実験で検証する項目と成功基準を整理する | Backlog |  |
| 14 | 統合前チェックを行う | Backlog |  |
| 15 | 駄菓子事業計画書 初版へ統合する | Backlog |  |
| 16 | 最終レビューと人間確認用チェックリストを作成する | Backlog |  |
| 17 | 自動化計画の処理書を作成する | Backlog |  |

## ステータス管理ルール

使用するStatus:

- Backlog
- Ready
- In Progress
- Blocked
- Evidence Check
- Review
- Changes Requested
- Integration Ready
- Integrated
- Done

基本遷移:

```text
Backlog -> Ready -> In Progress -> Evidence Check -> Review -> Integration Ready -> Integrated -> Done
```

修正遷移:

```text
Review -> Changes Requested -> In Progress -> Evidence Check -> Review
```

`Integration Ready` は統合可能状態であり、`Done` ではない。`Done` は最終統合への反映、反映漏れ確認、ログ記録が完了した状態とする。

## 証拠駆動ルール

- 根拠のない断定をしない。
- 決定事項、内部前提、事実、仮説、推論、要検証、要調査を分離する。
- 各Issueでエビデンス表を作成する。
- 未確認情報は要検証または要調査として扱う。

| 主張 | 分類 | 根拠 | 確信度 | 次に確認すべきこと |
|---|---|---|---|---|
|  |  |  |  |  |

## エージェント構成

- オーケストレーター
- 要件定義エージェント
- リサーチエージェント
- 分析エージェント
- 作成 / 実装エージェント
- レビューエージェント
- レッドチームエージェント
- ブルーチームエージェント
- パープルチームエージェント
- 統合エージェント
- ジャッジエージェント
- ナレッジ / ログ管理エージェント
- 工数見積もりエージェント

## 完了条件

- [ ] すべての主要子Issueが `Done` になっている。
- [ ] 最終統合Issueが `Done` になっている。
- [ ] 最終レビューIssueが `Done` になっている。
- [ ] レビュー済みIssueだけが事業計画書へ統合されている。
- [ ] Integration ReadyとDoneが混同されていない。
- [ ] 証拠、仮説、要検証、要調査が追跡できる。
- [ ] 人間確認が必要な点が明示されている。
- [ ] この親Issueの段階で事業計画書本文を作成していない。
