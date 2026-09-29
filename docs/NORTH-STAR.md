# Product Knowledge — North Star

作成: 2026-09-28  
更新: 2026-09-29  
ステータス: 方向正本（「向かう先」）

> 立ち上げ設計・ディレクトリ詳細 → [INITIAL-DESIGN.md](INITIAL-DESIGN.md)

## ミッション

**プロダクト開発の baseline OS** — 新規プロダクトの出発点にも、既存プロダクトの品質底上げにも使える、横断的な開発知識ベース。

足場（スタック・ホスティング・認証の配線）から、ドーパミン・習慣・依存設計、UI・体験まで — **プロダクトをどう作るか** を蓄積する。

[Growth Knowledge](https://github.com/mayu119/growth-knowledge) が **プロダクトをどう売る・伸ばすか**（マーケ・マネタイズ・集客・計測）なら、Product Knowledge（PK）は **その前とその土台** — ユーザーが価値を感じ、続けたくなる体験と、その裏のルール、そしてそれを載せる足場を蓄積する。

## 2 repo モデル

| | Product Knowledge | Growth Knowledge |
|---|---|---|
| **問い** | プロダクトをどう作るか | プロダクトをどう売る・伸ばすか |
| **中身** | 足場 + 体験 + ロジック | マーケ + マネタイズ + 集客 + 計測 |
| **典型** | スタック選定、コアループ、習慣化、UI・演出、ドメインルール | ペイウォール、価格、広告、UGC 獲得、experiments |
| **数字** | 書かない（GK へ） | CVR、ARPU、adopt 判定 |
| **アプリ層** | 採用判断 × **実装・ルール** | 施策 × 日付 × **結果** |

```
PK: プロダクトをどう作るか（足場 + 体験 + ロジック）
GK: プロダクトをどう売る・伸ばすか（マーケ + マネタイズ + 集客 + 計測）
```

### 2 つのユースケース

| ユースケース | 目的 | PK の使い方 |
|---|---|---|
| **新規プロダクト baseline** | 足場から体験・ロジックの土台を揃える | `patterns/*/guide.md` で原理を決め、`references/` の parts を参照しながら実装 |
| **既存プロダクト quality lift（C）** | 弱点を特定し、採用パターンで底上げ | `apps/<app>.md` に Before/After・採用記録・PR リンクを残す |

`apps/` は **最終段**（採用記録）。設計の出発点ではない。

## PK スコープ — 7 レイヤ

| レイヤ | テーマ | 内容 |
|---|---|---|
| **足場** | `scaffold/` | スタック選定、ホスティング、DB、バインディング、認証配線、「1 プラットフォームに賭ける」判断（例: Cloudflare）、lock-in vs ship のトレードオフ |
| **体験 1** | `core-loop/` | セッション構造、報酬粒度、エスカレーション、失敗設計 |
| **体験 2** | `feedback/` | 即時フィードバック、マイクロ報酬、音・動き |
| **体験 3** | `progression/` | 解放、マスター、コレクション、星・実績 |
| **体験 4** | `habit/` | ストリーク、デイリー、復帰、さびつき |
| **体験 5** | `information/` | 画面構成、ナビゲーション、状態パターン（空・読込・エラー等） |
| **体験 6** | `visual/` | 色・タイポ・コンポーネント・マスコット・モーション |

### Logic vs 非 Logic の境界

| 区分 | テーマ | 中身 |
|---|---|---|
| **Logic（PK 管轄）** | core-loop / progression / habit | セッション構造、報酬粒度、解放・進行、ストリーク・復帰 |
| **非 Logic（PK 管轄）** | feedback / information / visual | 演出、状態表示、UI・モーション |
| **足場（PK 管轄）** | scaffold | スタック・インフラ・認証配線の横断判断 |

Logic は「ユーザーが何を繰り返し、どう進むか」。非 Logic は「それがどう見え・感じるか」。足場は「それをどう載せるか」。

## プラットフォーム方針

**原理はプラットフォーム非依存** — iOS、Web、Android など、どのプラットフォームにも適用できる形で `guide.md` を書く。

- **guide（原理）**: プラットフォームを限定しない。判断基準・トレードオフを横断で書く
- **parts・参照 OSS**: プラットフォーム固有でよい（例: iOS → [SwiftPieces](../references/2026-09-27-swiftpieces.md)、Web → 別途追加）
- **除外なし**: 「Android / Web baseline 対象外」のような platform 除外は設けない

## 運用フロー

```
references/              外部事例・OSS 解析（未蒸留・parts 出典）
    ↓
patterns/*/candidates.md   横断候補（出典付き）
    ↓
patterns/*/guide.md        蒸留済み原理（正本）
    ↓
apps/<app>.md              採用記録（最終段）
```

| 段階 | 正本 | 役割 |
|---|---|---|
| `references/` | 一次解析・parts カタログ | 外部 OSS・競合の分解。コードは **リポジトリ内に置かない**（外部 URL のみ） |
| `candidates.md` | 未蒸留候補 | guide 昇格前の仮置き |
| `guide.md` | 横断原理 | 採用判断の根拠 |
| `apps/<app>.md` | プロダクト固有採用 | どの pattern/part を入れたか、弱点改善、実装 PR |

1. **拾う** — 事例・競合・OSS → `references/YYYY-MM-DD-slug.md`
2. **候補** — 横断で使えそうなら `patterns/<theme>/candidates.md`
3. **蒸留** — 精査後 `patterns/<theme>/guide.md`
4. **降ろす** — プロダクト固有 → `apps/<app>.md`
5. **検証** — 施策を出荷したら **数字だけ** GK の `apps/` + `measurement/experiments.md`

## テンプレートの定義

PK における「テンプレート」= **2 層**:

1. **原理（principles）** — `patterns/<theme>/guide.md`  
   なぜそうするか。横断で再利用する判断基準。

2. **Parts カタログ（copy-paste 参照）** — `references/` および `candidates.md` 内の外部 OSS 参照  
   例: SwiftPieces のコンポーネント URL。実コードは repo に含めない。

新規プロダクトは guide で方針を決め、parts 参照で実装を加速する。

## `apps/<app>.md` 仕様（採用記録）

既存プロダクト uplift（C）および新規プロダクト出荷後の記録用。

```markdown
# <AppName>

## 概要
<!-- 1–2 行 -->

## 採用パターン
| テーマ | guide 参照 | 採用した part / 判断 | 実装 PR |
|---|---|---|---|
| scaffold | patterns/scaffold/guide.md#… | … | https://github.com/…/pull/… |
| core-loop | patterns/core-loop/guide.md#… | … | … |
| visual | … | SwiftPieces: … | … |

## Before / After
| 弱点（Before） | 採用した PK 知見 | 改善メモ（After） |
|---|---|---|
| … | … | … |

## ドメインルール（Logic）
<!-- reveal 条件、状態遷移など apps 固有のルール -->

## メモ
<!-- 計測結果・CVR は Growth Knowledge へ。ここには書かない -->
```

**必須**: 採用パターン表の **実装 PR** 列。  
**禁止**: CVR・adopt 判定・実験数字（→ GK `apps/` + `measurement/`）。

## 蒸留基準（distillation）

**現行: 緩め**

- `guide.md` 昇格条件: **信頼できる出典 1 件 + 妥当な rationale** で OK
- 複数ソース・自社 adopt 実績が溜まったら基準を **段階的に厳格化**
- 未検証の海外ポストだけで既存 guide を書き換えない

## PK がやらないこと

- **マーケ・マネタイズ・集客・計測** → Growth Knowledge
- **`apps/` を設計の出発点にすること**（採用記録は最終段）
- **ソースコードのホスティング**（外部 OSS 参照のみ）

## 関連

- [INITIAL-DESIGN.md](INITIAL-DESIGN.md) — repo 立ち上げ・ディレクトリ・運用フロー（足場・プラットフォームの狭い記述は本書が優先）
- [README.md](../README.md) — 入口・テーマ一覧
- [Growth Knowledge](https://github.com/mayu119/growth-knowledge) — マーケ・マネタイズ・計測
