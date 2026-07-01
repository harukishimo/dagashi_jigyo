# Loop State

## 1. この文書の役割

この文書は、セッションをまたいでLoop Engineeringの現在状態を共有するための状態ファイルである。

この文書は、事業計画書本文、完成条件、詳細ログではない。作業再開時にOrchestrator、Triage、各エージェントが「今どこまで進んでいて、次に何をすべきか」を短時間で把握するために使う。

## 2. 読むタイミング

- セッション開始時。
- `daily_triage` 開始前。
- `pr_babysitter` 開始前。
- Status変更前。
- L2 / L3実行前。
- Human Queueから再開する時。
- コンテキスト利用率が高くなり、要約して継続する時。

## 3. 更新するタイミング

- セッション終了前。
- OrchestratorがLoop pattern、L分類、次アクションを採用した後。
- Human Queue、Blocked、PR Ready、Auto Committed、Doneへ移る時。
- `docs/loop-run.log.md` で速度低下または一時停止が必要になった時。
- Red / Blue / Purple Teamの運用確認結果が出た時。

更新主体はOrchestratorまたはKnowledge / Log Agentとする。Triage Agentは更新案を出せるが、原則として直接更新しない。

## 4. 現在状態

| 項目 | 内容 |
|---|---|
| project_goal | 駄菓子事業計画書初版を作成する |
| current_phase | #2 前提台帳Done / #3 L2開始判断前 |
| active_loop_patterns | `daily_triage`, `pr_babysitter` |
| current_branch | main |
| github_repo | `git@github.com:harukishimo/dagashi_jigyo.git` |
| current_control_stage | none |
| current_run_id | LOG-007 |
| last_decision_by | Human / GitHub |
| next_target | #3 Why / 解決したい課題をL2で開始するかHuman / Orchestratorが判断する |
| resume_condition | PR #19 merge済み。Issue #2はcompletedとしてclose済み、`integration-ready` と `status: done` 付与済み。次に対応可能なIssue候補は#3 |
| resume_owner | Orchestrator / Human |
| last_updated | 2026-07-01 |
| updated_by | Orchestrator |

## 5. 重要な決定事項

| ID | 決定事項 | 根拠 | 更新日 |
|---|---|---|---|
| D-001 | 上位ゴールはLoop基盤整備ではなく、駄菓子事業計画書初版の作成である | `docs/projects.md`, `docs/intend.md` | 未記録 |
| D-002 | サブエージェント起動方式はB方式である | `agents/README.md`, `docs/loop.md` | 未記録 |
| D-003 | 現時点のLoop patternは `daily_triage` と `pr_babysitter` のみである | `docs/loop.md` | 未記録 |
| D-004 | L1はレポートのみ、L2はPR作成まで、L3はdenylist外のAuto-commitまでである | `docs/levels.md` | 未記録 |
| D-005 | denylist本体は `docs/denylist.json` を正とする | `docs/levels.md`, `docs/denylist.json` | 未記録 |
| D-006 | GitHub Issue #1〜#18は、事業計画書初版作成に必要な問い、入力、出力契約、Human Check、Integration Ready条件を持つ構造へ更新済み | GitHub Issue #1〜#18 | 2026-06-30 |
| D-007 | 2026-07-01のdaily_triageでは、#1〜#18はすべてopen、Issueコメントはなし、open PRはなし。次に対応可能なIssueは#2 前提台帳 | GitHub Issue #1〜#18, GitHub PR検索, `docs/dailytriage.md` | 2026-07-01 |
| D-008 | Humanが#2 前提台帳のL2開始を承認した | ユーザー発言「#2をL2で開始してOK」 | 2026-07-01 |
| D-009 | #2 前提台帳はR/B/P/Judgeレビューを通し、構造化素材としてIntegration Readyと判定した。ただしHuman Check未解消事項は残す | `docs/issue-02-prerequisite-ledger.md`, `docs/loop-run.log.md` | 2026-07-01 |
| D-010 | #2 前提台帳のPR #19を作成し、Issue #2に `status: pr-ready` と `integration-ready` を付与した | PR #19, GitHub Issue #2, `docs/loop-run.log.md` | 2026-07-01 |
| D-011 | PR #19がmergeされ、Issue #2はcompletedとしてcloseされた。Issue #2の状態labelは `status: done` に更新済み | PR #19, GitHub Issue #2 | 2026-07-01 |

## 6. 現在の未完了事項

| ID | 対象 | 状態 | 次アクション | Owner |
|---|---|---|---|---|
| S-001 | Loop運用資料 | 整備済み | 必要に応じて運用しながら更新する | Orchestrator |
| S-002 | GitHub Issue品質改善 | 完了 | 更新済みIssueを起点に実行開始可否を判断する | Human / Orchestrator |
| S-003 | 事業計画Issue実行 | #2 Done / #3未着手 | #3 Why / 解決したい課題をL2で開始するか判断する | Orchestrator / Human |
| S-004 | PR handoff | open PRなし | open PRが作成されたら `pr_babysitter` へ渡す | Orchestrator |

## 7. Human Queue

| ID | 判断待ち内容 | 関連資料 | 再開条件 |
|---|---|---|---|
| HQ-002 | #2前提台帳のP-003〜P-008を決定事項として扱ってよいか | `docs/issue-02-prerequisite-ledger.md`, GitHub Issue #2 | Humanが承認または修正する |
| HQ-003 | 事業目的、収益方針、実施方針の外部説明表現をどう扱うか | `docs/issue-02-prerequisite-ledger.md`, `docs/projects.md` | Humanが使用可能な表現を指定する |
| HQ-004 | 法規制、衛生、安全、責任範囲の確認先と担当をどうするか | `docs/issue-02-prerequisite-ledger.md`, `docs/denylist.json` | Humanが確認先または判断担当を決める |
| HQ-005 | #3をL2で開始してよいか | GitHub Issue #3, `docs/issue-02-prerequisite-ledger.md`, `docs/levels.md` | Human / Orchestratorが#3開始可否を判断する |

## 8. 参照すべき中核資料

| 資料 | 役割 |
|---|---|
| `docs/projects.md` | 上位目的 |
| `docs/intend.md` | 事業計画書初版のゴール判定表 |
| `docs/loop.md` | Loop全体運用、停止条件 |
| `docs/levels.md` | L1 / L2 / L3分岐基準 |
| `docs/denylist.json` | L3除外条件 |
| `docs/dailytriage.md` | Issue棚卸しLoop pattern |
| `docs/pr-babysitter.md` | PR監視Loop pattern |
| `docs/loop-run.log.md` | Loop実行ログ、メトリクス監視 |
| `agents/agent_list.md` | エージェント早見表 |
| `agents/README.md` | エージェント仕様一覧と基本処理フロー |

## 9. 再開時チェックリスト

1. `docs/state.md` を読む。
2. `docs/loop-run.log.md` で停止条件に該当していないか確認する。
3. `docs/loop.md` でLoop patternと停止条件を確認する。
4. `agents/agent_list.md` で起動候補エージェントを確認する。
5. 対象がIssueなら `daily_triage`、PRなら `pr_babysitter` を使う。
6. Triage結果をOrchestratorが採用または差し戻す。
7. 必要な場合だけL2 / L3の実行へ進む。

## 10. 状態更新テンプレート

```md
## State Update

- updated_at:
- updated_by:
- trigger:
- loop_pattern:
- target:
- current_phase:
- status_before:
- status_after:
- level:
- denylist_check:
- changed_files:
- human_queue:
- control_stage: none / 速度を落とす / 一時停止
- run_id:
- last_decision_by:
- resume_condition:
- resume_owner:
- next_action:
- notes:
```

## 11. Human Queue確認テンプレート

```md
## Human Queue Request

- request_id:
- target:
- reason:
- 判断してほしい事項:
- 選択肢:
- 推奨案:
- リスク:
- 再開条件:
- resume_owner:
```
