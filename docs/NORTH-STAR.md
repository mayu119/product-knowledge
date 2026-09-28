# Product Knowledge — North Star

作成: 2026-09-28  
ステータス: 方向正本（「向かう先」）

> 立ち上げ設計・ディレクトリ詳細 → [INITIAL-DESIGN.md](INITIAL-DESIGN.md)

## ミッション

**アプリ体験の baseline OS** — 新規アプリの出発点にも、既存アプリの品質底上げにも使える、横断的な体験・ロジック知識ベース。

[Growth Knowledge](https://github.com/mayu119/growth-knowledge) が転換・課金・獲得・計測なら、Product Knowledge（PK）は **転換の前** — ユーザーが価値を感じ、続けたくなる体験と、その裏のルールを蓄積する。

## 2 つのユースケース

| ユースケース | 目的 | PK の使い方 |
|---|---|---|
| **新規アプリ baseline** | ゼロから体験・ロジックの土台を揃える | `patterns/*/guide.md` で原理を決め、`references/` の parts を参照しながら実装 |
| **既存アプリ quality lift（C）** | 弱点を特定し、採用パターンで底上げ | `apps/<app>.md` に Before/After・採用記録・PR リンクを残す |

`apps/` は **最終段**（採用記録）。設計の出発点ではない。

## Baseline スコープ B

**含む**

| レイヤ | 内容 |
|---|---|
| 体験 6 層 | core-loop / feedback / progression / habit / information / visual |
| information 拡張 | 画面構成、ナビゲーション、状態パターン（空・読込・エラー等） |
| UI・演出 | コンポーネント方針、即時 FB、モーション |

**含まない**

- プロジェクト足場（フォルダ構成、MVVM セットアップ、アーキテクチャチュートリアル）
- データ層、API、認証

## Logic vs 非 Logic の境界

| 区分 | テーマ | 中身 |
|---|---|---|
| **Logic（PK 管轄）** | core-loop / progression / habit | セッション構造、報酬粒度、解放・進行、ストリーク・復帰 |
| **非 Logic（PK 管轄）** | feedback / information / visual | 演出、状態表示、UI・モーション |
| **PK 外** | — | データ層、API、auth、永続化、ネットワーク |

Logic は「ユーザーが何を繰り返し、どう進むか」。非 Logic は「それがどう見え・感じるか」。

## プラットフォーム

**iOS のみ**（SwiftUI 中心）。

- 原理はプラットフォーム非依存で書く
- 実装 parts・参照 OSS は SwiftUI / iOS 向けを優先（例: [SwiftPieces](../references/2026-09-27-swiftpieces.md)）
- Android / Web の baseline は対象外

## レイヤの役割

```
references/          外部事例・OSS 解析（未蒸留・parts 出典）
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
| `apps/<app>.md` | アプリ固有採用 | どの pattern/part を入れたか、弱点改善、実装 PR |

## テンプレートの定義

PK における「テンプレート」= **2 層**:

1. **原理（principles）** — `patterns/<theme>/guide.md`  
   なぜそうするか。横断で再利用する判断基準。

2. **Parts カタログ（copy-paste 参照）** — `references/` および `candidates.md` 内の外部 OSS 参照  
   例: SwiftPieces のコンポーネント URL。実コードは repo に含めない。

新規アプリは guide で方針を決め、parts 参照で実装を加速する。

## `apps/<app>.md` 仕様（採用記録）

既存アプリ uplift（C）および新規アプリ出荷後の記録用。

```markdown
# <AppName>

## 概要
<!-- 1–2 行 -->

## 採用パターン
| テーマ | guide 参照 | 採用した part / 判断 | 実装 PR |
|---|---|---|---|
| core-loop | patterns/core-loop/guide.md#… | … | https://github.com/…/pull/… |
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

## Growth Knowledge との境界（不変）

[INITIAL-DESIGN.md の境界表](INITIAL-DESIGN.md#置き場所の境界混線防止) を正とする。要約:

| 問い | 置き場 |
|---|---|
| 体験・ループ・演出・状態・ドメインルール | **Product Knowledge** |
| ペイウォール・価格・trial・広告・UGC 獲得 | Growth Knowledge |
| 施策結果・CVR・adopt | Growth Knowledge `apps/` + `measurement/` |
| 体験パターン採用・実装 | Product Knowledge `apps/` |

```
転換・課金・広告・実験の数字     → Growth Knowledge
体験・ループ・演出・状態・ルール  → Product Knowledge
両方                            → PK で設計 → GK で検証
```

## PK がやらないこと

- 転換・課金・獲得・計測（Growth Knowledge の領域）
- ソースコードのホスティング（外部 OSS 参照のみ）
- プロジェクト足場・アーキテクチャチュートリアル
- データ層・API・認証の設計
- Android / Web baseline
- `apps/` を設計の出発点にすること（採用記録は最終段）

## 関連

- [INITIAL-DESIGN.md](INITIAL-DESIGN.md) — repo 立ち上げ・ディレクトリ・運用フロー
- [README.md](../README.md) — 入口・テーマ一覧
