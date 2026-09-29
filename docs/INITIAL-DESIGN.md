# Product Knowledge — 初期設計

作成: 2026-09-27  
ステータス: 採用（repo 立ち上げ時の設計正本）

> **向かう先**（ミッション・スコープ・ユースケース）→ [NORTH-STAR.md](NORTH-STAR.md)

> **2026-09-29 更新**: [NORTH-STAR.md](NORTH-STAR.md) が PK スコープの正本。本書に残る「体験のみ」「iOS のみ」「足場・API・auth を PK 外とする」記述は **上書き済み**。足場（`patterns/scaffold/`）とプラットフォーム非依存方針は NORTH-STAR を参照。

## 背景

[Growth Knowledge](https://github.com/mayu119/growth-knowledge) は **転換・課金・獲得・計測** の横断 OS として機能している。テーマは paywall / onboarding / marketing / ugc / measurement が中心。

一方、次の知見は GK のどのテーマにも「入るが本丸ではない」位置になりやすかった:

- コアループ・快感・習慣化（例: [dopa-drill](../references/2026-09-27-dopa-drill.md)）
- 即時フィードバック・演出・数値エスカレーション
- スキルツリー・コレクション・非ガチャ進行
- ドメインルール・状態機械（例: ホンネの reveal、Words For Me の問い→答え→着地）
- 自社 UGC 議論の「視聴者のドーパミン」——**本人の行為に報酬が紐づくか**

GK に無理やり足すと:

- onboarding に retention を押し込む
- ugc に体験原理を混ぜる
- `apps/` に原理と計測結果が混線する

→ **体験・ロジック専用 repo を切る** 判断。

## 2 repo の役割

| | Growth Knowledge | Product Knowledge |
|---|---|---|
| **問い** | これで DL / trial / purchase / 継続課金は動くか | ユーザーはここで快感・理解・達成感を得るか。ロジックは破綻しないか |
| **典型コンテンツ** | PW 構造、価格、広告、UGC 獲得、experiments | コアループ、フィードバック、進行、習慣化、ドメイン仕様 |
| **数字** | CVR、ARPU、完走率、adopt 判定 | 書かない（GK へ） |
| **アプリ層** | 施策 × 日付 × **結果** | 採用判断 × **実装・ルール** |

### 30 秒決定木

```
転換・課金・広告・実験の数字が知りたい     → Growth Knowledge
体験・ループ・演出・状態・ドメインルール    → Product Knowledge
両方                                      → Product で設計 → GK で検証
```

## ディレクトリ設計

```
product-knowledge/
├── README.md                 # 入口・境界・運用要約
├── docs/
│   └── INITIAL-DESIGN.md     # 本書（repo 設計の正本）
├── patterns/                 # 横断層 — 蒸留済み原理
│   ├── core-loop/            # セッション構造、報酬粒度、失敗設計
│   ├── feedback/             # 即時 FB、音・動き、マイクロ報酬
│   ├── progression/          # 解放、マスター、コレクション
│   ├── habit/                # ストリーク、デイリー、復帰、さび
│   ├── information/          # 日常 UI、状態表示、エラー（オンボ到達は GK）
│   └── visual/               # 色、タイポ、コンポーネント、マスコット、モーション
├── references/               # 外部事例・OSS・競合の解析（未蒸留）
└── apps/                     # アプリ固有の体験採用・ドメインルール
```

各 `patterns/<theme>/` は Growth Knowledge と同型:

| ファイル | 役割 |
|---|---|
| `README.md` | テーマの読み方 |
| `guide.md` | 蒸留済み原理（正本） |
| `candidates.md` | 未蒸留候補 |

## 置き場所の境界（混線防止）

| 内容 | Product Knowledge | Growth Knowledge |
|---|---|---|
| コアループ・快感・習慣化 | ✅ `patterns/` | ❌ |
| ペイウォール UI・価格・trial | ❌ | ✅ `paywall/` |
| オンボ完走・aha **到達** | ❌（到達後の日常体験は PK） | ✅ `onboarding/` |
| UGC 台本・フック・獲得 | ❌（「視聴者快感」の原理のみ PK 可） | ✅ `ugc/` |
| 実験結果・CVR・adopt | ❌ | ✅ `measurement/` + `apps/` |
| ドメイン状態機械・reveal ルール | ✅ `apps/<app>.md` | ❌ |
| 外部 OSS / 競合解析 | ✅ `references/` | ❌（マーケ記事は GK inbox） |

## 運用フロー（GK と同型）

1. **拾う** — 事例・競合・OSS → `references/YYYY-MM-DD-slug.md`
2. **候補** — 横断で使えそうなら `patterns/<theme>/candidates.md`
3. **蒸留** — 精査後 `patterns/<theme>/guide.md`。未検証の海外ポストだけで正本を書き換えない
4. **降ろす** — アプリ固有 → `apps/<app>.md`
5. **検証** — 施策を出荷したら **数字だけ** GK の `apps/` + `measurement/experiments.md`

## 重複を避けるルール

| 段階 | 正本 |
|---|---|
| 外部事例の原文・分解 | Product `references/` |
| 横断原理 | Product `patterns/*/guide.md` |
| アプリへの採用・実装 | Product `apps/` |
| 施策の計測・adopt | Growth Knowledge `apps/` |

同じ事例を両 repo に書かない。**references に一次解析 → patterns に原理 → GK に結果** の一方向。

## GK 拡張 vs 別 repo（採用理由）

| 観点 | GK に `retention/` 等を足す | 別 repo（採用） |
|---|---|---|
| 関心の分離 | 弱い（growth 色が強い） | 強い |
| 蒸留の語彙 | 転換・CVR に引っ張られる | 体験・ループ用語で書ける |
| エージェント | 1 コンテキスト | 「どっち？」の判断が必要 → README 決定木で吸収 |
| 運用コスト | 低 | repo が 2 つ |

**採用条件（2026-09-27 時点）**: dopa-drill 級の解析、Words For Me の問い→着地、UGC の視聴者快感議論など、growth 以前の知見が既に複数ある。

## リスクと対策

| リスク | 対策 |
|---|---|
| 二重管理 | references → patterns → GK apps の一方向。同じ OSS を両方に全文コピーしない |
| エージェントが迷う | README + 本書の決定木。Cloud Agent environment に両 repo を登録 |
| repo 増殖 | 当面 **GK + PK の 2 repo 固定**。engineering 専用 repo は中身が溢れたら |
| PK に数字が混入 | `apps/` テンプレに「計測は GK」と明記。レビュー時に除外 |

## 最初のコンテンツ

立ち上げと同時に入れた参照実装:

- [ドパドリル（dopa-drill）解析](../references/2026-09-27-dopa-drill.md) — コアループ・快感設計の分解（Product Knowledge 第 1 号 references）
- [ドパドリル デザイン分解](../references/2026-09-27-dopa-drill-design.md) — 色・UI・マスコット・モーション
- [core-loop/guide.md](../patterns/core-loop/guide.md) — 快感から蒸留した 4 原理
- [visual/guide.md](../patterns/visual/guide.md) — デザインから蒸留した 6 原理

## 今後足す候補（未着手）

- `apps/wordsforme.md` — 問い→答え→着地、ドーパミン→安堵（GK `apps/wordsforme.md` 817 行付近の設計）
- `apps/honnecard.md` — reveal 峰值、感情と課金の分離
- ~~Growth Knowledge README への相互リンク 1 行~~ ✅ 2026-09-27
- Linear プロジェクト（Growth Knowledge 並び、任意）

## 関連リンク

- Product Knowledge: https://github.com/mayu119/product-knowledge
- Growth Knowledge: https://github.com/mayu119/growth-knowledge
- 第 1 号解析: [references/2026-09-27-dopa-drill.md](../references/2026-09-27-dopa-drill.md)
