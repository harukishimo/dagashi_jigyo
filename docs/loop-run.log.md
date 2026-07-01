# Loop Run Log

## 1. この文書の役割

この文書は、Loop Engineeringの実行ログと運用メトリクスを記録するためのログである。

記録目的は、タスク単位のコスト増加、同一対象への過剰リトライ、false positive増加、コンテキスト圧迫を検知し、Loopを「速度を落とす」または「一時停止」へ切り替えることである。

この文書は、事業計画書本文や通常の作業メモではない。

## 2. 監視メトリクス

| 観点 | metric | 記録単位 | しきい値 | しきい値到達時の基本対応 |
|---|---|---|---|---|
| トークンコスト | token-per-task | タスク単位コスト | ベースラインの2倍超 | 速度を落とす |
| 実行回数 | loop iterations | 同一アイテムへのリトライ数 | 同一対象に3回超 | 一時停止 |
| 成功率 | completion rate / false positive rate | 対象種別ごとの完了率、誤検知率 | false positive 30%超 | 速度を落とす。改善不能なら一時停止 |
| コンテキスト利用率 | context window occupancy | セッション単位 | 85%超 | 速度を落とす。要約不能なら一時停止 |

## 2.1 メトリクス算出方法

| metric | 算出方法 | 不明時の扱い |
|---|---|---|
| token-per-task | 対象タスクで使用した概算トークン数。取得できない場合はセッション内の相対的な重さを記録する | 不明の場合は `unknown` とし、他の停止条件と併せて判断する |
| token_baseline | 同種タスク直近3件のtoken-per-task中央値。3件未満の場合は初回値を仮ベースラインにする | 未設定の場合は速度低下候補として記録する |
| token_ratio | `token_per_task / token_baseline` | baseline不明なら `unknown` |
| loop iterations | 同一 `target_id` に対する同一目的の再試行回数 | target_id不明なら一時停止候補 |
| completion rate | 直近10件または当日分のうち `completed` の割合 | 母数3件未満なら参考値 |
| false positive rate | 直近10件または当日分のうち、完了扱い後に誤検知・誤分類・不要PR・不適切handoffと判明した割合 | 分母不明なら速度低下候補 |
| context window occupancy | ツールまたは実行環境で確認できる文脈使用率。数値取得不能なら、長大履歴、要約必要、読み戻し困難の有無で判断する | 不明で、かつ作業継続に支障がある場合は速度低下 |

## 3. 運用段階

本Loopの停止段階は以下の2つだけとする。

| 段階 | 意味 | 実行できること | 実行しないこと |
|---|---|---|---|
| 速度を落とす | Loopは継続するが、対象数、同時実行、L3実行、再試行を制限する | `daily_triage`、状態整理、ログ記録、Human Queue整理、必要最小限のL2 | 新規L3、複数対象の並列処理、判断が曖昧なPR作成 |
| 一時停止 | Loopを止め、人間判断または状態整理を待つ | `docs/state.md` 更新、`docs/loop-run.log.md` 更新、停止理由の記録 | 新規実行、Auto-commit、PR作成、merge、外部副作用 |

## 4. Red / Blue / Purple Team運用確認

しきい値に到達した場合、L3候補の場合、またはdenylist該当が `unknown` の場合、OrchestratorはRed / Blue / Purple Teamを起動し、運用が正しく制御されているか確認する。

| Team | 確認観点 | 出力 |
|---|---|---|
| Red Team | 誤検知、過剰実行、停止漏れ、denylist見落とし、同一対象の過剰リトライを指摘する | リスク、失敗パターン、一時停止候補 |
| Blue Team | Red Team指摘に対し、速度低下、対象分割、ログ補強、Human Queue化などの改善策を出す | 改善策、再開条件、追加ログ項目 |
| Purple Team | Red / Blueの結果から、速度を落とすか一時停止するかを判断材料として整理する | 採用判断、保留判断、Orchestratorへの推奨 |

Red / Blue / Purple Teamは、運用判断の材料を作る。最終的に速度を落とすか一時停止するかはOrchestratorが採用し、必要に応じてHumanへ確認する。

## 5. ログ記録タイミング

- `daily_triage` 実行後。
- `pr_babysitter` 実行後。
- L2 / L3実行後。
- 同一対象への再試行後。
- false positiveを検出した時。
- context window occupancyが85%を超えた時。
- 速度を落とす、または一時停止を採用した時。
- Red / Blue / Purple Team運用確認を行った時。

## 6. ログテンプレート

```md
## Loop Run Entry

- run_id:
- timestamp:
- loop_pattern:
- target_type: issue / pr / doc / session
- target_id:
- target_chapter:
- level:
- agents_used:
- status_before:
- status_after:
- token_per_task:
- token_baseline:
- token_ratio:
- loop_iterations:
- completion_result: completed / partial / blocked / false_positive
- false_positive_rate:
- context_window_occupancy:
- threshold_hit:
- action: 速度を落とす / 一時停止 / none
- red_team_check:
- blue_team_check:
- purple_team_check:
- human_queue_reason:
- state_update_required:
- resume_condition:
- resume_owner:
- next_action:
```

## 7. 現在のログ

| run_id | timestamp | target | loop_pattern | level | token_ratio | iterations | false_positive_rate | context_occupancy | threshold_hit | action | next_action |
|---|---|---|---|---|---:|---:|---:|---:|---|---|---|
| LOG-000 | 未記録 | 初期設定 | none | none | 0 | 0 | 0% | 0% | none | none | 初回Loop実行時に記録する |
| LOG-001 | 2026-06-30 | Loop運用資料整備 | none | none | unknown | 0 | 0% | unknown | none | none | GitHub Issue確認再開前にdaily_triageを実行する |
| LOG-002 | 2026-06-30 | GitHub Issue #1〜#18品質改善 | daily_triage | L2 | unknown | 1 | 0% | unknown | none | none | ユーザー指示後、#2から事業計画Issue実行へ進む |
| LOG-003 | 2026-07-01 10:17 JST | GitHub Issue #1〜#18 daily triage | daily_triage | L1 | unknown | 1 | 0% | unknown | none | none | #2 前提台帳のL2実行開始可否をHuman / Orchestratorが判断する |
| LOG-004 | 2026-07-01 10:59 JST | GitHub Issue #2 前提台帳L2ドラフト | issue_execution | L2 | unknown | 1 | 0% | unknown | none | none | #2 前提台帳ドラフトをR/B/P/JudgeまたはPR化前レビューへ進める |
| LOG-005 | 2026-07-01 11:17 JST | GitHub Issue #2 前提台帳R/B/P/Judgeレビュー | issue_review | L2 | unknown | 1 | 0% | unknown | none | none | #2をPR化する。PR作成前に実差分denylist再確認と未コミット変更の分離を確認する |

## 8. Red / Blue / Purple Team運用レビュー

### LOG-001 レビュー結果

対象:

- `docs/state.md`
- `docs/loop-run.log.md`
- `docs/loop.md`
- `agents/14_triage_agent.md`
- `agents/agent_list.md`

Red Team指摘:

- TriageがLoop patternやStatusを採用できるように読める箇所があり、権限境界が揺れていた。
- L3のdenylist確認が実行前に寄っており、実差分ベースの再確認が不足していた。
- Verifierの実体が曖昧だった。
- L2標準フローと短縮フローが混在し、短絡実行の危険があった。
- 停止メトリクスの算出方法と再開条件が不足していた。

Blue Team提案:

- `state.md` に `run_id`、停止段階、再開条件、再開Ownerを追加する。
- `loop-run.log.md` にメトリクス算出方法を追加する。
- `loop.md` に再開条件を追加する。
- Triage出力に `evidence_source`、`confidence`、`state_update_required`、`loop_log_update_required` を追加する。
- 停止段階ごとの許可エージェントを `agents/agent_list.md` に追加する。

Purple Team判断:

- `daily_triage` / `pr_babysitter` をLoop patternとして分離し、Triage Agentを診断担当に限定する設計は採用可能。
- Red / Blue / Purple Teamは判断材料を作る役割に留め、最終採用はOrchestratorに残す。
- メトリクス定義、Verifierの扱い、TriageとOrchestratorの権限境界は実運用前に明記する。

採用した修正:

- Triageは候補提示のみ、OrchestratorがLoop pattern、L分類、Status、起動順を採用するよう統一した。
- Status変更はOrchestratorに限定した。
- Review AgentがVerifierを兼務することを明記した。
- PR作成前またはAuto-commit前に、実差分ベースのdenylist再確認を必須化した。
- `docs/loop-run.log.md` にメトリクス算出方法を追加した。
- `docs/state.md` に `resume_condition`、`resume_owner`、`current_run_id` を追加した。
- `agents/agent_list.md` に停止段階ごとの許可エージェントを追加した。

### LOG-002 レビュー結果

対象:

- GitHub Issue #1〜#18
- `docs/projects.md`
- `docs/intend.md`
- `docs/loop.md`
- `docs/levels.md`
- `docs/denylist.json`
- `agents/agent_list.md`

Red Team指摘:

- 既存Issueは手順の骨格が多く、事業計画書のどの章を作るIssueなのか追跡しづらかった。
- `Why`、対象者、価値、実証実験、リスク、収支などのIssueで、どの情報を根拠に判断するかが弱かった。
- Human Check、法規制、衛生、安全、責任範囲が分離されないままIssueが走ると、AIが確定してはいけない事項まで確定する危険があった。
- 統合前チェックと最終レビューのゲートが弱く、質の低い素材でも事業計画書へ統合される可能性があった。

Blue Team提案:

- 各Issueに `Target`、対応BP章、対応G、入力情報、出力契約、Human Check、Integration Ready条件を持たせる。
- 各Issueを「作業指示」ではなく「事業計画書の章を成立させる問い」に組み替える。
- 主張とエビデンス、仮説、要検証、要調査、人間確認事項を分離する。
- #15〜#17をゲートIssueとして扱い、未レビュー情報や根拠なし断定を統合しない。

Purple Team判断:

- GitHub Issueは、Loop運用対象ではなく、事業計画書初版の情報収集・判断・統合単位として再設計する必要がある。
- #1は親Issue、#2〜#14は事業計画素材、#15〜#17は統合・レビューゲート、#18はBP本文対象外のcontrol issueとして分ける方針を採用する。
- 実装や各Issueの実行は行わず、Issue本文の品質改善までで止める。

採用した修正:

- #1〜#18のIssue本文を更新した。
- 各子Issueに、問い、入力情報、出力契約、表テンプレート、Human Check、Integration Ready条件、禁止事項を追加した。
- #13で法規制、食品衛生、営業許可・届出、アレルギー、安全、責任範囲をHuman Checkへ分離した。
- #15〜#17を、統合前チェック、初版統合、最終レビューのゲートとして再定義した。
- #18を事業計画書本文対象外の自動化・Loop運用Issueとして整理した。

### LOG-003 Daily Triage Result

- loop_pattern: daily_triage
- pattern_source: docs/dailytriage.md
- target_issue: GitHub Issue #1〜#18
- target_chapter: #2はBP-14 / BP-13 / 全章、#3〜#17は各IssueのTarget定義に従う、#18はBP本文対象外
- current_status: #1〜#18はopen。Issueコメントは全件なし。open PRなし
- proposed_status: 事業計画Issue実行開始判断待ち
- level: triage自体はL1。次に対応可能な#2はlevel_default L2
- denylist_check: #2実行は事業目的、前提、Human Checkの分類を扱うため、L3不可。`business_decision` と `legal_hygiene_safety` に触れる可能性があり、Human Checkを分離してL2で扱う
- denylist_source: docs/denylist.json
- trigger_reason: 1日1回のdaily_triage。事業計画書初版完成までの進行確認
- current_state: Issue品質改善は完了済み。事業計画Issueの実行、本文作成、統合用素材作成は未実施
- missing_information: #2開始可否に関するHuman / Orchestratorの明示判断
- human_decision_required: #2 前提台帳をL2で開始してよいか。決定事項として扱ってよい前提の承認は#2実行内でHuman Checkへ分離する
- recommended_next_action: Orchestratorが#2を対象にRequirements -> Research -> Analysis -> Creation / Implementer -> Review / VerifierのL2実行可否を判断する
- handoff_to: human / orchestrator
- related_pr: なし
- blocker: 技術的blockerなし。進行上のblockerは#2開始判断待ち
- pr_handoff: open PRなしのため `pr_babysitter` への引き渡しなし
- changed_files: `docs/state.md`, `docs/loop-run.log.md`

### LOG-004 Issue #2 L2 Result

- loop_pattern: issue_execution
- target_issue: GitHub Issue #2 `[Child 01] 前提情報・決定事項を抽出する`
- target_chapter: BP-14 / BP-13 / 全章
- trigger_reason: Humanが「#2をL2で開始してOK」と承認したため、daily_triage後の次工程としてL2実行へ進めた
- status_before: daily_triage完了 / #2 L2開始判断待ち
- status_after: #2 前提台帳L2ドラフト作成済み / Integration Ready判定前
- level: L2
- agents_used: Orchestrator, Requirements, Research, Analysis, Creation / Implementer, Review / Verifier, Knowledge / Log
- source_inputs: GitHub Issue #1〜#18, `docs/projects.md`, `docs/intend.md`, `docs/loop.md`, `docs/levels.md`, `docs/denylist.json`, `docs/loop_engineering_plan.md`, `docs/automation_process_plan.md`, `agents/*.md`, `駄菓子事業計画 Loop Engineering 実行指示書.pdf`
- created_artifact: `docs/issue-02-prerequisite-ledger.md`
- output_summary: 前提台帳、BP章別マッピング、主張とエビデンス表、矛盾・不足・未確認事項、Human Check一覧を作成した
- evidence_classification: 決定事項、内部前提、事実、推論、要検証、要調査、Human Checkを分離した
- denylist_check: L3不可。`business_decision` と `legal_hygiene_safety` に該当し得るため、確定判断を避けてHuman Checkへ分離した
- diff_denylist_check: 差分は前提台帳ドラフトと状態/ログ更新。事業判断、法規制、衛生、安全、責任範囲を確定していない
- review_result: 条件付きOK。#2ドラフトとしての出力契約は満たすが、R/B/P/Judge未実施のためIntegration Readyとは判定しない
- human_queue: P-003〜P-008の決定事項化可否、外部説明表現、法規制・衛生・安全・責任範囲の確認先、#2次工程判断
- pr_handoff: なし。open PR未作成
- threshold_hit: none
- action: none
- changed_files: `docs/issue-02-prerequisite-ledger.md`, `docs/state.md`, `docs/loop-run.log.md`
- next_action: #2 前提台帳ドラフトをRed / Blue / Purple / Judgeへ回すか、条件付きOKとしてPR化前レビューへ進めるかをOrchestrator / Humanが判断する

### LOG-005 Issue #2 R/B/P/Judge Review Result

- loop_pattern: issue_review
- target_issue: GitHub Issue #2 `[Child 01] 前提情報・決定事項を抽出する`
- target_chapter: BP-14 / BP-13 / 全章
- trigger_reason: Humanが#2ドラフトのレビュー実施と修正を依頼したため
- status_before: #2 前提台帳L2ドラフト作成済み / Integration Ready判定前
- status_after: #2 前提台帳L2レビュー済み / Integration Ready / PR未作成
- level: L2
- agents_used: Red Team, Blue Team, Purple Team, Judge, Review / Verifier, Knowledge / Log
- red_team_result: P-001の根拠IDが曖昧、Integration Readyの誤読リスク、GitHub Status/label未適用の混同リスク、GitHub状態スナップショットの鮮度リスクを指摘
- blue_team_result: 上位ゴール用source_id追加、P-001修正、GAP-004更新、Integration Readyの範囲限定、PR Handoff追加を実施
- purple_team_result: R-001〜R-004を採用。GitHub Issue Status/label変更は外部副作用のため本レビューでは行わず、Human Queueへ残す
- judge_result: Integration Ready。BP-13/BP-14へ渡せる構造化素材として統合可能。ただしPR未作成でL2完了ではない
- output_summary: #2前提台帳にR/B/P/Judgeレビュー結果、修正内容、Judge判定、PR Handoffを追加した
- evidence_classification: 決定事項、内部前提、事実、推論、要検証、要調査、Human Checkの分離を維持した
- denylist_check: L3不可。`business_decision`, `legal_hygiene_safety`, `external_side_effects`に触れるため、PR作成・GitHub Issue変更・mergeは人間承認またはL2の明示判断が必要
- diff_denylist_check: 差分はレビュー結果、状態、ログの更新。事業判断、法規制、衛生、安全、責任範囲、GitHub Issue Status/labelを確定・変更していない
- integration_ready: yes。構造化素材としての判定であり、P-003〜P-008やP-013の人間確認事項を確定したものではない
- pr_handoff: PR未作成。PRタイトル候補は `#2 前提台帳を追加`
- human_queue: P-003〜P-008の決定事項化可否、外部説明表現、法規制・衛生・安全・責任範囲の確認先、#2 PR作成とGitHub Issue Status/label変更の扱い
- threshold_hit: none
- action: none
- changed_files: `docs/issue-02-prerequisite-ledger.md`, `docs/state.md`, `docs/loop-run.log.md`
- next_action: #2をPR化する。PR作成前に実差分denylist再確認と未コミット変更の分離を確認する
