# Issue #2 前提台帳

## 1. メタ情報

| 項目 | 内容 |
|---|---|
| target_issue | GitHub Issue #2 `[Child 01] 前提情報・決定事項を抽出する` |
| target_bp_chapter | BP-14 |
| secondary_bp_chapter | BP-13, 全章 |
| level | L2 |
| created_at | 2026-07-01 |
| created_by | Orchestrator / Creation-Implementation |
| status | レビュー済み。Integration Ready（構造化素材として）/ PR #19作成済み |
| user_approval | 2026-07-01にユーザーが「#2をL2で開始してOK」と承認 |

この文書は、駄菓子事業計画書初版へ統合する前の前提台帳である。事業計画書本文ではない。

## 2. スコープ

### 含めること

- 既存docs、GitHub Issue #1〜#18、添付PDF、会話上の承認から前提候補を抽出する。
- 決定事項、内部前提、事実、仮説、推論、要検証、要調査、Human Checkを分離する。
- BP-01〜BP-14およびG-01〜G-15への接続を示す。
- 決定事項候補を、AIだけで決定事項へ自動昇格させない。

### 含めないこと

- 事業計画書本文の作成。
- 法規制、衛生、安全、保険、責任範囲、施設合意、保護者同意の確定。
- 商品、価格、実証実験条件、外部説明文の確定。
- GitHub Issueのclose、label、assignee、Status変更。

## 3. source_id一覧

| source_id | 種別 | 内容 |
|---|---|---|
| CONV-GOAL-2026-07-01 | 会話 | ユーザーが駄菓子事業計画書初版完成までdaily_triageを継続するゴールを設定 |
| CONV-2026-07-01-001 | 会話 | ユーザーが#2をL2で開始してよいと承認 |
| DOC-PROJECTS | docs | `docs/projects.md` |
| DOC-INTEND | docs | `docs/intend.md` |
| DOC-LOOP | docs | `docs/loop.md` |
| DOC-LEVELS | docs | `docs/levels.md` |
| DOC-DENYLIST | docs | `docs/denylist.json` |
| DOC-LOOP-PLAN | docs | `docs/loop_engineering_plan.md` |
| DOC-AUTOMATION | docs | `docs/automation_process_plan.md` |
| PDF-INSTRUCTION | pdf | `駄菓子事業計画 Loop Engineering 実行指示書.pdf` |
| GH-ISSUE-001 | GitHub | #1 `[Parent] 駄菓子事業計画 初版作成` |
| GH-ISSUE-002 | GitHub | #2 `[Child 01] 前提情報・決定事項を抽出する` |
| GH-ISSUE-003-018 | GitHub | #3〜#18の各Issue本文 |
| GH-STATE-2026-07-01 | GitHub | 2026-07-01確認時点で#1〜#18 open、コメント付きopen Issueなし、open PRなし |

## 4. 前提台帳

| ID | 主張 | 分類 | source_id | 接続BP | 接続G | 確信度 | 次に確認すべきこと |
|---|---|---|---|---|---|---|---|
| P-001 | 最終成果物は駄菓子事業計画書初版である | 決定事項 | CONV-GOAL-2026-07-01, DOC-PROJECTS, DOC-INTEND, GH-ISSUE-001 | BP-01〜BP-14 | G-01〜G-15 | 高 | 初版の外部利用範囲はHuman Checkで確認する |
| P-002 | Loop Engineeringは目的ではなく、事業計画書初版を作るための手段である | 決定事項 | DOC-PROJECTS, DOC-LOOP, GH-ISSUE-001 | BP-14 | G-14 | 高 | なし |
| P-003 | この事業は単なる駄菓子販売ではなく、駄菓子を媒介にして子どもと地域・社会をつなぐ体験事業として扱う | 内部前提 / 決定事項候補 | DOC-PROJECTS, DOC-LOOP-PLAN, PDF-INSTRUCTION, GH-ISSUE-001 | BP-02, BP-03, BP-05 | G-01, G-02, G-04 | 高 | 決定事項として断定してよいかHuman承認を取る |
| P-004 | 収益最大化を主目的にしない | 内部前提 / 決定事項候補 | DOC-PROJECTS, DOC-LOOP-PLAN, PDF-INSTRUCTION | BP-03, BP-11 | G-02, G-10 | 高 | 外部説明での表現をHuman Checkへ送る |
| P-005 | 実費回収を基本とする | 内部前提 / 決定事項候補 | DOC-PROJECTS, DOC-LOOP-PLAN, PDF-INSTRUCTION | BP-07, BP-11 | G-06, G-10 | 高 | 実費に含める範囲、赤字許容、無料提供の扱いをHuman Checkへ送る |
| P-006 | 最初は移動式駄菓子屋として小さく始める | 内部前提 / 決定事項候補 | DOC-PROJECTS, DOC-LOOP-PLAN, PDF-INSTRUCTION | BP-06, BP-08, BP-12 | G-05, G-07 | 高 | 実施場所、規模、頻度は未確定として扱う |
| P-007 | 最初から本格展開しない | 内部前提 / 決定事項候補 | DOC-PROJECTS, DOC-LOOP-PLAN, PDF-INSTRUCTION | BP-06, BP-12 | G-05, G-07 | 高 | ロードマップで段階的検証として扱う |
| P-008 | 実証実験でニーズ、子どもの反応、運営課題、施設側の受け入れやすさを確認する | 内部前提 / 決定事項候補 | DOC-PROJECTS, DOC-LOOP-PLAN, PDF-INSTRUCTION | BP-08, BP-12 | G-07, G-11 | 高 | 測定項目と成功基準は#14で仮説化し、Human Checkへ送る |
| P-009 | 重視する体験価値は、選ぶ体験、金銭感覚、交流、地域接点、思い出づくりである | 内部前提 | DOC-PROJECTS, PDF-INSTRUCTION | BP-02, BP-05 | G-01, G-04 | 中 | 価値の優先順位と外部説明表現をHuman Checkへ送る |
| P-010 | 子ども、保護者、施設、地域団体がこの体験価値を必要としているかは未検証である | 要検証 | DOC-PROJECTS, DOC-INTEND | BP-02, BP-04, BP-05, BP-08 | G-01, G-03, G-04, G-07 | 高 | #3, #4, #5, #8, #14で検証方法へ落とす |
| P-011 | 移動式駄菓子屋が施設や地域に受け入れられるかは未検証である | 要検証 | DOC-PROJECTS, GH-ISSUE-006, GH-ISSUE-012 | BP-06, BP-08, BP-12 | G-05, G-07 | 高 | 施設相談、施設ルール確認、実証条件をHuman Checkへ送る |
| P-012 | 実費回収で継続可能な商品構成、価格、販売量になるかは未検証である | 要検証 | DOC-PROJECTS, GH-ISSUE-009, GH-ISSUE-011 | BP-07, BP-11 | G-06, G-10 | 高 | 商品・価格・費用は#9と#11で仮説化する |
| P-013 | 安全、衛生、アレルギー、現金管理、責任分界は運用可能性を確認する必要がある | 要調査 / Human Check | DOC-PROJECTS, DOC-DENYLIST, GH-ISSUE-010, GH-ISSUE-013 | BP-09, BP-10, BP-13 | G-08, G-09, G-11, G-12 | 高 | 公式確認先、施設ルール、責任分界を人間または専門先で確認する |
| P-014 | BP-01〜BP-14が事業計画書初版の必須章である | 内部前提 | DOC-PROJECTS, DOC-INTEND, GH-ISSUE-001 | BP-01〜BP-14 | G-14 | 高 | 統合時は#15で不足章を確認する |
| P-015 | G-01〜G-15が事業計画書初版のゴール判定表である | 内部前提 | DOC-INTEND, GH-ISSUE-001 | BP-01〜BP-14 | G-01〜G-15 | 高 | #17でPass / Conditional Pass / Failを判定する |
| P-016 | すべての主張は分類、根拠、確信度、次確認事項を持つべきである | 内部前提 | DOC-PROJECTS, DOC-LOOP-PLAN, PDF-INSTRUCTION, GH-ISSUE-002 | BP-14 | G-13, G-14 | 高 | 後続Issue成果物でも同じ形式を維持する |
| P-017 | 既存資料に書かれているだけの情報を決定事項として扱ってはいけない | 制約 / Human Check | DOC-PROJECTS, GH-ISSUE-002 | BP-13, BP-14 | G-12, G-13 | 高 | 決定事項候補はHuman承認があるまで内部前提として扱う |
| P-018 | レビュー済みまたは未レビューであることが明確な情報だけを統合対象にする | 内部前提 | DOC-PROJECTS, DOC-LOOP, DOC-AUTOMATION, GH-ISSUE-001 | BP-14 | G-14, G-15 | 高 | #15の統合前チェックで判定する |
| P-019 | #2の成果物は前提台帳、BP章別マッピング、主張とエビデンス表、矛盾・不足・未確認事項、Human Check一覧である | 事実 | GH-ISSUE-002 | BP-14, BP-13 | G-12, G-13, G-14 | 高 | 本文作成ではなく構造化素材として扱う |
| P-020 | #2はlevel_default L2で、integration_target yes、major_issue yesである | 事実 | GH-ISSUE-002 | BP-14, BP-13 | G-14, G-15 | 高 | 主要IssueとしてR/B/P/Judgeを実施済み。PR作成前の最終denylist再確認を維持する |
| P-021 | #3〜#14は各章の素材を作る子Issueであり、#15〜#17は統合・レビューゲートである | 事実 | GH-ISSUE-001, GH-ISSUE-003-018 | BP-01〜BP-14 | G-01〜G-15 | 高 | #2の前提は後続Issueの入力として扱う |
| P-022 | #18は事業計画書本文対象外のcontrol issueである | 事実 | GH-ISSUE-001, GH-ISSUE-018 | BP本文対象外 | G-14 | 高 | BP本文には統合しない |
| P-023 | 2026-07-01時点でGitHub Issue #1〜#18はopen、コメント付きopen Issueなし、open PRなしである | 事実 | GH-STATE-2026-07-01 | BP-14 | G-14 | 高 | 次回daily_triageで状態を再確認する |
| P-024 | #2実行は事業目的、前提、Human Check分類を扱うためL3不可である | 推論 | DOC-LEVELS, DOC-DENYLIST, GH-ISSUE-002 | BP-13, BP-14 | G-12, G-13 | 高 | L2の範囲内でPR作成前に実差分denylistを再確認する |
| P-025 | 事業計画書初版は完成版や外部提出版ではない | 内部前提 | DOC-PROJECTS, DOC-INTEND | BP-01, BP-13 | G-09, G-12 | 高 | 外部提出、営業、交渉に使う確定版は別途Human承認が必要 |

## 5. BP章別マッピング

| BP章 | 関連前提ID | 引き継ぎ方 |
|---|---|---|
| BP-01 Executive Summary | P-001, P-002, P-025 | 初版の位置づけと未確認事項を短く示す |
| BP-02 Why / 解決したい課題 | P-003, P-009, P-010 | 課題仮説として扱い、根拠不足の断定を避ける |
| BP-03 事業コンセプト | P-003, P-004 | 単なる販売ではなく体験事業という前提をHuman承認待ちで扱う |
| BP-04 対象者とステークホルダー | P-010, P-011 | 子ども、保護者、施設、地域団体を仮説として分ける |
| BP-05 提供価値 | P-003, P-009, P-010 | ステークホルダー別価値仮説へ渡す |
| BP-06 実施形態 | P-006, P-007, P-011 | 移動式・小規模開始は前提候補、場所や頻度は未確定にする |
| BP-07 商品・価格・実費回収 | P-005, P-012, P-013 | 価格確定を避け、仮説と要調査を分ける |
| BP-08 初回実証実験計画 | P-008, P-010, P-011, P-012, P-013 | 実施条件を確定せず、検証項目とHuman Checkへ分ける |
| BP-09 運営体制 | P-013 | 安全、現金管理、責任分界を未確認事項として扱う |
| BP-10 リスク・規制・衛生・安全 | P-013, P-024 | 確定判断をせず、確認先候補と要調査へ渡す |
| BP-11 収支・継続性 | P-004, P-005, P-012 | 実費回収の範囲と継続条件を仮説化する |
| BP-12 ロードマップ | P-006, P-007, P-008 | 小さく検証し改善する流れへ接続する |
| BP-13 未確認事項・Human Check | P-005, P-011, P-013, P-017, P-025 | Human承認、専門確認、施設確認を一覧化する |
| BP-14 エビデンス・Issue対応表 | P-001〜P-025 | source_idとIssue対応を維持する |

## 6. 矛盾・不足・未確認事項

| ID | 内容 | 種別 | 影響 | 次アクション |
|---|---|---|---|---|
| GAP-001 | `docs/projects.md`には「現時点の決定事項」とあるが、#2は既存資料にあるだけの情報を決定事項扱いしないよう求めている | 分類上の注意 | P-003〜P-008を決定事項へ自動昇格できない | Human Checkで決定事項として扱ってよい前提を承認する |
| GAP-002 | PDFの章立て候補は27項目、現行docs/intend.mdはBP-01〜BP-14で整理されている | 仕様差分 | 後続Issueの統合単位が混乱する可能性 | 現行のBP-01〜BP-14を正とし、PDF章立ては由来情報として扱う |
| GAP-003 | GitHub Issueの本文にはStatus設計があるが、実際のIssue metadataにはlabelやStatusが付いていない | 運用未実装 | 自動Status遷移はまだできない | 必要なら#18でStatus/label運用を実装検討する |
| GAP-004 | #2はmajor_issueのためRed / Blue / Purple / Judge結果が必要 | レビュー工程 | 本レビューで実施済み。PR化時はレビュー結果を含める | LOG-005および本書9章を参照する |
| GAP-005 | 法規制、衛生、安全、責任範囲は公式確認や専門確認が未実施 | 要調査 | BP-10、BP-13で断定できない | 確認先候補と人間担当を決める |
| GAP-006 | 対象地域、施設候補、対象年齢層、参加条件が未確定 | 要検証 / Human Check | BP-04、BP-06、BP-08が仮説止まりになる | #4、#6、#8で仮説化し、人間判断へ送る |
| GAP-007 | 価格、商品カテゴリ、仕入れ先、無料提供の有無が未確定 | 要検証 / Human Check | BP-07、BP-11が仮説止まりになる | #9、#11で試算し、最終判断はHuman Checkへ送る |

## 7. Human Check一覧

| ID | 判断待ち内容 | 関連前提 | 推奨扱い | 再開条件 |
|---|---|---|---|---|
| HC-001 | P-003〜P-008を決定事項として扱ってよいか | P-003〜P-008, GAP-001 | 承認までは内部前提 / 決定事項候補 | Humanが承認または修正する |
| HC-002 | 事業目的、収益方針、実施方針の外部説明表現 | P-003〜P-006, P-025 | 外部説明文としては未確定 | Humanが使用可能な表現を指定する |
| HC-003 | 実費回収に含める費用範囲、赤字許容、無料提供、寄付、施設負担 | P-004, P-005, P-012 | 仮説扱い | Humanが費用方針を判断する |
| HC-004 | 初回実証の場所、日時、対象者、参加条件 | P-006, P-008, P-011 | 未確定 | Humanが候補条件を承認する |
| HC-005 | 施設承認、保護者説明、責任分界、保険 | P-011, P-013 | 要調査 / Human Check | 施設または専門先への確認方針を決める |
| HC-006 | 食品衛生、営業許可・届出、食品表示、アレルギー対応 | P-013, P-024 | 要調査 / Human Check | 公式確認先、地域の保健所等への確認方針を決める |
| HC-007 | GitHub IssueのStatus/label運用を実際に適用するか | GAP-003 | #18候補 | Human / Orchestratorが運用実装の要否を判断する |

## 8. 統合用サマリ

#2時点では、駄菓子事業計画書初版の上位目的、章構成、証拠分類、Issue接続は整理済みである。一方、事業コンセプト、収益方針、移動式で小さく始める方針は、既存資料上は強い前提として繰り返し現れるが、#2の禁止事項に従い、Human承認までは「内部前提 / 決定事項候補」として扱う。法規制、食品衛生、安全、アレルギー、責任範囲、施設合意、保護者説明、価格や費用負担はAIだけで確定せず、BP-13のHuman CheckおよびBP-10の要調査事項へ引き継ぐ。

## 9. レビュー / Verifier結果

| 項目 | 内容 |
|---|---|
| reviewed_at | 2026-07-01 11:17 JST |
| result | Integration Ready（構造化素材として） |
| pr_status | PR #19作成済み。人間レビュー待ち |
| done_status | 未完了。mergeまたは完了判断は人間が行う |

### Red Team指摘

| ID | 指摘 | リスク | 対応 |
|---|---|---|---|
| R-001 | P-001の根拠に#2開始承認の会話IDが含まれており、上位ゴールの根拠として曖昧だった | #2開始承認を事業計画書初版の決定根拠と誤読する | `CONV-GOAL-2026-07-01` を追加し、P-001のsource_idを差し替えた |
| R-002 | Human Checkが残っているため、Integration Readyを「事業判断の確定」と誤読する可能性がある | P-003〜P-008や法規制・衛生・安全が確定扱いになる | Integration ReadyはBP-13/BP-14へ渡せる構造化素材としての判定に限定する |
| R-003 | GitHub IssueのStatus/label未適用を成果物の未完了と混同しやすい | GitHub上の運用未整備により進行状態が見えにくい | 本成果物ではGitHub Issue変更を行わず、HC-007および#18候補として維持する |
| R-004 | #2の前提台帳は後続Issueの入力になるため、出典スナップショットの鮮度を誤解しやすい | 次回triage時点のIssue状態とずれる | GH-STATE-2026-07-01は確認時点の事実として維持し、次回daily_triageで再確認する |

### Blue Team対応

| ID | 対応 | 反映先 |
|---|---|---|
| B-001 | 上位ゴールの会話根拠を独立したsource_idに分けた | source_id一覧、P-001 |
| B-002 | R/B/P/Judge未実施だったGAP-004をレビュー実施済みへ更新した | GAP-004 |
| B-003 | Integration Readyの範囲を「構造化素材として」に限定した | メタ情報、レビュー結果、Judge判定 |
| B-004 | PR作成状況、GitHub Issue label/comment、Human Check未解消を明示した | レビュー結果、PR Handoff |

### Purple Team判断

- R-001は採用し、根拠IDを修正済み。
- R-002は採用し、Integration Readyを事業判断確定ではなく、BP-13/BP-14へ渡せる構造化素材の判定として限定する。
- R-003は保留ではなく運用課題として維持する。GitHub IssueのStatus/label変更は外部副作用に当たるため本L2レビューでは行わない。
- R-004は採用し、GitHub状態は2026-07-01確認時点のスナップショットとして扱う。

### Judge判定

| 判定項目 | 結果 | 理由 |
|---|---|---|
| 出力契約 | Pass | 前提台帳、BP章別マッピング、主張とエビデンス表、矛盾・不足・未確認事項、Human Check一覧を含む |
| 証拠分類 | Pass | 決定事項、内部前提、事実、推論、要検証、要調査、Human Checkを分離している |
| Human Check分離 | Pass | P-003〜P-008、P-013、HC-001〜HC-007を確定せず分離している |
| denylist | Pass for L2 / Fail for L3 | `business_decision`, `legal_hygiene_safety`, `external_side_effects`に触れるためL3不可。L2成果物としては確定判断を避けている |
| Integration Ready | Pass | BP-13/BP-14へ渡せる構造化素材として統合可能 |
| PR Ready | Pass | PR #19を作成済み。mergeまたは完了判断は人間が行う |

### PR Handoff

- branch: `codex/issue-2-prerequisite-ledger`
- pr_url: https://github.com/harukishimo/dagashi_jigyo/pull/19
- pr_title候補: `#2 前提台帳を追加`
- PR本文に含めるべき内容: 前提台帳作成、R/B/P/Judgeレビュー結果、Integration Readyは構造化素材としての判定であること、Human Check未解消事項、L3不可理由。
- PR作成前確認: `docs/denylist.json` の実差分再確認、既存の未コミット変更を巻き込まないことを確認済み。
- GitHub Issue更新: #2に `status: pr-ready` と `integration-ready` を付与し、PR #19へのコメントを追加済み。
