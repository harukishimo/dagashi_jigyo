# 駄菓子事業計画 Loop Engineering 自動化計画 処理書

## 1. 自動化の目的

- 駄菓子事業計画書初版の作成プロセスを、GitHub Issue駆動、証拠駆動、レビュー駆動で運用できるようにする。
- LooperまたはオーケストレーターAIが、Issueごとに必要なサブエージェントを起動し、Statusを進めるための処理ルールを定義する。
- 事業計画書本文を先に生成せず、レビュー済みの統合素材だけを最終統合に使う仕組みを作る。
- サブエージェント起動方式はB方式を採用する。Looper / Orchestratorは各エージェントの役割、起動条件、出力責務を前提知識として保持し、Issueごとに必要なエージェントを判断する。

## 2. 自動化対象範囲

- Issueテンプレートに基づくIssue作成支援。
- Issue Statusの遷移チェック。
- エージェント起動条件の判定。
- 子Issueの標準処理フロー実行。
- エビデンス表、仮説、要検証、要調査の存在確認。
- レビュー、レッドチーム、ブルーチーム、パープルチーム、ジャッジの実行順序管理。
- Integration Ready、Integrated、Doneの判定支援。
- ナレッジ / ログの記録支援。
- 追加Issue候補の抽出。

## 3. 自動化しない範囲

- 人間確認が必要な判断の確定。
- 法規制、衛生、保険、責任範囲の最終判断。
- 施設、保護者、地域団体との実際の合意形成。
- 実証実験の現場実施。
- 未確認情報の事実化。
- レビュー未完了Issueの最終統合。
- この段階での駄菓子事業計画書本文作成。

## 4. GitHub Issue運用設計

- 親Issueを全体管理Issueとし、子Issue、統合前チェックIssue、最終統合Issue、最終レビューIssue、自動化計画Issueをリンクする。
- 子Issueは論点単位で作成し、最終事業計画書へ統合するための素材を作る。
- 各IssueにはStatus、Type、Priority、Agent、Evidence、Reviewに関するLabelを付ける。
- Issue本文には、統合用サマリ、エビデンス表、仮説、要検証、要調査、レビュー結果、チーム検証結果、ジャッジ結果を残す。

## 5. Issue種別

| 種別 | 用途 | テンプレート |
|---|---|---|
| Parent | 全体方針、子Issue一覧、完了条件管理 | `parent_issue.md` |
| Child | 論点別の調査、分析、統合素材作成 | `child_issue.md` |
| Integration | Integration Ready Issueの統合 | `integration_issue.md` |
| Review | 最終レビューと人間確認 | `review_issue.md` |
| Automation Plan | 自動化処理設計 | `automation_plan_issue.md` |

## 6. Issueテンプレート

- `.github/ISSUE_TEMPLATE/parent_issue.md`
- `.github/ISSUE_TEMPLATE/child_issue.md`
- `.github/ISSUE_TEMPLATE/integration_issue.md`
- `.github/ISSUE_TEMPLATE/review_issue.md`
- `.github/ISSUE_TEMPLATE/automation_plan_issue.md`

各テンプレートは、根拠、仮説、要検証、要調査、レビュー結果、Status判定を記録できる構造にする。

## 7. Status設計

| Status | 自動化上の意味 |
|---|---|
| Backlog | 作成済み、未着手 |
| Ready | 入力がそろい着手可能 |
| In Progress | エージェント処理中 |
| Blocked | 人間判断、外部情報、追加調査待ち |
| Evidence Check | 主張分類と根拠確認中 |
| Review | レビュー待ち |
| Changes Requested | 修正待ち |
| Integration Ready | 統合可能 |
| Integrated | 最終統合へ反映済み |
| Done | 反映確認とログ記録まで完了 |

## 8. Label設計

| Label | 用途 |
|---|---|
| `type:parent` | 親Issue |
| `type:child` | 子Issue |
| `type:integration` | 統合Issue |
| `type:review` | レビューIssue |
| `type:automation` | 自動化計画Issue |
| `status:backlog` | Backlog |
| `status:ready` | Ready |
| `status:in-progress` | In Progress |
| `status:blocked` | Blocked |
| `status:evidence-check` | Evidence Check |
| `status:review` | Review |
| `status:changes-requested` | Changes Requested |
| `status:integration-ready` | Integration Ready |
| `status:integrated` | Integrated |
| `status:done` | Done |
| `agent:review-required` | レビュー必須 |
| `agent:red-team-required` | レッドチーム必須 |
| `agent:blue-team-required` | ブルーチーム必須 |
| `agent:purple-team-required` | パープルチーム必須 |
| `evidence:required` | エビデンス表必須 |
| `human-check-required` | 人間確認必須 |
| `blocked:external-info` | 外部情報待ち |
| `blocked:human-decision` | 人間判断待ち |

## 9. ステータス遷移ルール

### 基本遷移

```text
Backlog -> Ready -> In Progress -> Evidence Check -> Review -> Integration Ready -> Integrated -> Done
```

### 修正遷移

```text
Review -> Changes Requested -> In Progress -> Evidence Check -> Review
```

### Blocked遷移

```text
In Progress -> Blocked -> Ready
Evidence Check -> Blocked -> Evidence Check
Review -> Blocked -> Review
```

### 遷移ガード

- `Evidence Check` へ進むには、主張、分類、根拠、確信度、次に確認すべきことが記録されていること。
- `Review` へ進むには、統合用サマリ、仮説、要検証、要調査が記録されていること。
- `Integration Ready` へ進むには、レビュー、必要なチーム検証、ジャッジが完了していること。
- `Integrated` へ進むには、最終統合Issueで反映先が記録されていること。
- `Done` へ進むには、反映漏れ確認、ログ記録、ジャッジのDone判定が完了していること。

## 10. Looper処理フロー

1. 対象IssueのStatusを読み取る。
2. `Ready` のIssueを対象に、Statusを `In Progress` に変更する。
3. Issue種別を判定する。
4. Orchestratorが保持するエージェント役割マップに基づき、起動するサブエージェントと順序を判断する。
5. 子Issueなら、原則として要件定義、リサーチ、分析、作成、レビューの順でサブエージェントを起動する。
6. 主要Issueなら、レッドチーム、ブルーチーム、パープルチームを追加で起動する。
7. サブエージェントから次工程の提案が出た場合も、起動可否はOrchestratorが判断する。
8. 統合用サマリ、エビデンス表、仮説、要検証、要調査の有無を確認する。
9. `Evidence Check` で主張分類と根拠を確認する。
10. `Review` でレビューエージェントの結果を確認する。
11. ジャッジエージェントで `Integration Ready` 可否を判定する。
12. 条件未達なら `Changes Requested` または `Blocked` にする。
13. 条件達成なら `Integration Ready` にする。
14. ナレッジ / ログ管理エージェントが履歴を記録する。

## 11. サブエージェント起動条件

サブエージェントは自律的に別のサブエージェントを起動しない。各サブエージェントは、自分の出力内で次に必要な工程やエージェントを提案できる。実際の起動、順序変更、Status変更はOrchestratorが決定する。

| エージェント | 起動条件 |
|---|---|
| オーケストレーター | 全Issue |
| 要件定義エージェント | 子Issue開始時、Issueが抽象的な時 |
| リサーチエージェント | 必要情報、外部情報、内部前提確認がある時 |
| 分析エージェント | 情報整理後、示唆や優先度を出す時 |
| 作成 / 実装エージェント | 統合用素材や処理書を作る時 |
| レビューエージェント | 各Issueで必須 |
| レッドチームエージェント | 主要Issue、リスクが高いIssue |
| ブルーチームエージェント | レッドチーム指摘後 |
| パープルチームエージェント | レッド / ブルー結果の統合が必要な時 |
| 統合エージェント | Integration Ready判定前、最終統合時 |
| ジャッジエージェント | Integration Ready、Integrated、Done判定時 |
| ナレッジ / ログ管理エージェント | 各Issue終盤、Status変更時 |
| 工数見積もりエージェント | WBSや実行工数の整理が必要な時 |

## 12. エージェント仕様書生成処理

- `agents/README.md` を作成する。
- 各エージェントにつき1つのMarkdown仕様書を作成する。
- 共通テンプレートの12項目を必ず含める。
- 全エージェント共通の禁止事項を必ず含める。
- 各エージェントの起動タイミング、入力、処理内容、出力、Statusとの関係、人間確認ケースを明記する。
- 生成した `agents/*.md` は、Orchestratorの役割マップの原本、監査基準、将来の実装参照として扱う。
- 実行時は、Orchestratorがこの役割マップを前提知識として保持しているものとして進める。毎回すべてのサブエージェント仕様書を読み込ませる運用を必須とはしない。

## 13. レビュー処理

- レビューエージェントは各Issueで必須起動する。
- レビュー結果は `OK`、`条件付きOK`、`要修正`、`要追加調査` のいずれかにする。
- 根拠が弱い主張、仮説として扱うべき主張、要検証、要調査、追加Issue候補を明示する。
- `OK` または条件が満たされた `条件付きOK` のみ、Integration Ready判定に進める。

## 14. レッド / ブルー / パープルチーム処理

### レッドチーム

- 主要Issueで起動する。
- 施設側、保護者側、運営側、子ども側、費用面、安全面から失敗要因と前提破綻を探す。
- 主要Issueでは3から7体相当の観点を想定して検証する。

### ブルーチーム

- レッドチームの指摘に対し、改善策、代替案、表現修正、追加Issue化、要検証化を提案する。

### パープルチーム

- 採用する指摘、反映する改善案、追加Issue化する内容、保留する内容を整理する。
- 現実的な修正方針に落とす。

## 15. Integration Ready判定

以下をすべて満たす場合のみ `Integration Ready` とする。

- Issue目的に対応した成果物がある。
- 統合用サマリがある。
- 主張とエビデンス一覧がある。
- 決定事項、内部前提、事実、仮説、推論、要検証、要調査が分離されている。
- レビューエージェントの確認が完了している。
- 主要Issueではレッド、ブルー、パープルの処理が完了している。
- ジャッジエージェントが可と判定している。
- 人間確認が必要な事項が分離されている。

## 16. Integrated判定

- 最終統合Issueで対象Issueの統合用サマリが反映されている。
- 反映箇所、反映した内容、反映しなかった内容が記録されている。
- 統合元Issueを追跡できる。
- 新しい主張が追加されていない。
- 要検証、要調査、人間確認事項が保持されている。

## 17. Done判定

- Statusが `Integrated` である。
- 反映漏れ確認が完了している。
- ナレッジ / ログ管理エージェントが履歴を記録している。
- ジャッジエージェントがDone可と判定している。
- 親Issueの場合、主要子Issue、統合Issue、最終レビューIssueがDoneになっている。

## 18. Blocked処理

Blockedにする場合は、Issueに以下を必ず記録する。

```md
## Blocked理由

### 何が不足しているか
-

### 誰の判断が必要か
-

### どの情報があれば再開できるか
-

### 追加Issueが必要か
-
```

Blocked解除後は、元の処理段階に戻す。人間判断待ちの場合は `human-check-required` を付ける。

## 19. Changes Requested処理

- レビュー、レッドチーム、ジャッジで条件未達となった場合に設定する。
- 修正内容、修正理由、再レビュー観点をIssueに記録する。
- 修正後は `In Progress` に戻し、`Evidence Check` と `Review` を再実行する。

## 20. 追加Issue作成ルール

追加Issue候補は以下の場合に作成する。

- 要調査事項が現在Issueのスコープを超える。
- 法規制、衛生、保険、責任範囲など人間確認または専門確認が必要。
- 主要な仮説を実証実験で検証する必要がある。
- レッドチーム指摘が大きく、別論点として扱う必要がある。
- 統合時に不足章や未整理論点が見つかった。

追加Issueには、親Issue、発生元Issue、発生理由、必要エージェント、期待成果物を記録する。

## 21. 最終統合処理

1. `Integration Ready` のIssue一覧を取得する。
2. 統合対象Issueのレビュー結果とジャッジ結果を確認する。
3. 統合対象サマリを章立て候補にマッピングする。
4. 反映する内容、反映しない内容、追加仮説、要検証、要調査を分ける。
5. 統合元Issueを追跡できる形で記録する。
6. 新しい主張が混入していないか確認する。
7. 最終レビューIssueへ渡す。
8. 反映済みIssueを `Integrated` にする。
9. 反映漏れ確認後に `Done` 判定へ進める。

## 22. 人間確認ポイント

- 事業の目的表現と外部説明表現。
- 施設側への説明、依頼、許可の要否。
- 法規制、食品衛生、アレルギー、保険、責任範囲。
- 初回実証実験の実施場所、日時、参加対象、運営体制。
- 商品仕入れ、価格設定、実費回収の許容範囲。
- 子ども、保護者、施設、地域団体への説明内容。
- 最終事業計画書として公開または提出してよい内容。

## 23. 自動化ログ・監査証跡

各Issueに以下を記録する。

- Status変更履歴。
- 起動したエージェント。
- 各エージェントの出力要約。
- レビュー結果。
- レッド / ブルー / パープルチーム結果。
- ジャッジ結果。
- 統合先と反映内容。
- 追加Issue候補。
- 人間確認が必要な事項。
- 未解決論点。

## 24. エラー時処理

| エラー | 処理 |
|---|---|
| 必須項目が空 | `Changes Requested` にして不足項目を列挙する |
| 根拠がない断定 | `Evidence Check` に戻し、分類を修正する |
| 要調査を事実扱い | `Changes Requested` にして要調査へ移す |
| 人間判断が必要 | `Blocked` にして確認事項を記録する |
| エージェント出力が矛盾 | パープルチームまたはジャッジエージェントで整理する |
| 統合対象外Issueが混入 | 最終統合Issueで除外し、理由を記録する |

## 25. 実装前チェックリスト

- [ ] GitHub Issueテンプレートが作成されている。
- [ ] StatusとLabelの対応が定義されている。
- [ ] 各Issueでレビューエージェントが必須になっている。
- [ ] 主要Issueでレッド / ブルー / パープルチームが起動する。
- [ ] 分析エージェントが3C分析固定ではない。
- [ ] Integration ReadyとDoneが分離されている。
- [ ] エビデンス表の形式が定義されている。
- [ ] Blocked、Changes Requestedの戻し先が定義されている。
- [ ] 人間確認ポイントが定義されている。
- [ ] ログ・監査証跡が残る。
- [ ] この段階で事業計画書本文を作成していない。

## 26. 今後の改善余地

- GitHub ProjectsのStatusフィールドとIssue Labelを同期する自動化。
- Issueテンプレートの必須項目未入力チェック。
- エビデンス表の分類漏れ検出。
- レビューエージェント出力の自動判定支援。
- Integration Ready条件の機械的チェック。
- 最終統合時のIssue反映トレーサビリティ自動生成。
- 人間確認待ちIssueのリマインド。
- 実証実験後の結果取り込みテンプレート追加。
