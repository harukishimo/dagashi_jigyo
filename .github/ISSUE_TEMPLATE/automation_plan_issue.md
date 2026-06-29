---
name: Automation Plan Issue
about: Loop Engineering自動化計画作成用Issue
title: "[Automation] 自動化計画の処理書を作成する"
labels: "type:automation,evidence:required,agent:review-required"
---

# 自動化計画作成Issue

## 自動化目的

- 駄菓子事業計画書初版の作成プロセスを、GitHub IssueとLooperで運用できるようにする。
- Issue作成、Status管理、エージェント起動、レビュー、統合、ログ記録の処理を設計する。

## 対象範囲

- Issue種別設計。
- Issueテンプレート設計。
- Status設計。
- Label設計。
- ステータス遷移ルール。
- Looper処理フロー。
- サブエージェント起動条件。
- レビュー条件。
- Integration Ready判定。
- Done判定。
- ログ・監査証跡。
- 人間確認ポイント。

## 非対象範囲

- 事業計画書本文の作成。
- 人間確認事項の最終確定。
- 実証実験の現場運営。
- 法規制、衛生、保険、責任範囲の専門判断。

## GitHub Issue運用

- 親Issue、子Issue、統合Issue、レビューIssue、自動化計画Issueに分ける。
- 子Issueは最終統合用素材を作る単位として扱う。
- 主要Issueではレッド / ブルー / パープルチームを起動する。
- 各Issueでレビューエージェントを必須起動する。

## Status設計

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

## Label設計

- `type:parent`
- `type:child`
- `type:integration`
- `type:review`
- `type:automation`
- `status:*`
- `agent:*`
- `evidence:required`
- `human-check-required`
- `blocked:*`

## エージェント起動条件

本LoopではB方式を採用する。Looper / Orchestratorは各エージェントの役割を前提知識として保持し、Issue内容とStatusから必要なサブエージェントを判断する。サブエージェントは次工程を提案できるが、自律的に別サブエージェントを起動しない。

| エージェント | 起動条件 |
|---|---|
| オーケストレーター | 全Issue |
| 要件定義エージェント | 子Issue開始時 |
| リサーチエージェント | 情報収集が必要な時 |
| 分析エージェント | 分析と示唆整理が必要な時 |
| 作成 / 実装エージェント | 統合素材または処理書を作る時 |
| レビューエージェント | 各Issueで必須 |
| レッドチームエージェント | 主要Issueで必須 |
| ブルーチームエージェント | レッドチーム後 |
| パープルチームエージェント | レッド / ブルー統合時 |
| 統合エージェント | 統合用サマリ作成時 |
| ジャッジエージェント | Status判定時 |
| ナレッジ / ログ管理エージェント | Status変更時、Issue終盤 |
| 工数見積もりエージェント | WBSや工数整理が必要な時 |

## レビュー条件

- 統合用サマリがある。
- 主張とエビデンス一覧がある。
- 仮説、要検証、要調査が分離されている。
- 根拠のない断定がない。
- Issue目的に対応した成果物がある。
- Integration Ready可否の判定材料がある。

## Integration Ready判定

- [ ] Issue目的に対応した成果物がある。
- [ ] 統合用サマリがある。
- [ ] 主張とエビデンス一覧がある。
- [ ] 仮説、要検証、要調査が分離されている。
- [ ] レビューが完了している。
- [ ] 主要Issueではレッド / ブルー / パープルチームが完了している。
- [ ] ジャッジエージェントが可と判定している。

## Done判定

- [ ] 最終統合に反映済みである。
- [ ] Statusが `Integrated` になっている。
- [ ] 反映漏れ確認が完了している。
- [ ] ログが記録されている。
- [ ] ジャッジエージェントがDone可と判定している。

## ログ・監査証跡

| 日時 | Issue | Status変更 | 起動エージェント | 判断理由 | 人間確認 |
|---|---|---|---|---|---|
|  |  |  |  |  |  |

## 人間確認ポイント

- 法規制、衛生、保険、責任範囲。
- 施設側との合意事項。
- 保護者や子どもへの説明内容。
- 実証実験の実施条件。
- 外部説明に使う表現。

## レビュー結果

Status: OK / 条件付きOK / 要修正 / 要追加調査

### 判定理由

-
