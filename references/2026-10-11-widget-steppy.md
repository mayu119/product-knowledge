# Steppy — 歩数ピクセルペット ウィジェット UI 分解（パクリ先）

- 出典: App Store [Steppy: Virtual Pet Widget](https://apps.apple.com/us/app/steppy-virtual-pet-widget/id6758967932)
- スクリーンショット: [widget-steppy-steps-pixel.png](/cursor/stores/bc-01a0e4fe-67b4-782e-aa4f-6290f907d0b3/media/widget-steppy-steps-pixel.png)
- 解析日: 2026-10-11
- 注: 名称 **StepBud** は AUv3 MIDI シーケンサー（別アプリ）。本 UI は **Steppy**（HealthKit 歩数 × ピクセルペット）と一致。

## アプリ同定

| 項目 | 内容 |
|---|---|
| 名称 | **Steppy: Virtual Pet Widget** |
| カテゴリ | ヘルスケア / フィットネス（歩数・習慣） |
| プラットフォーム | iPhone、Apple Watch（HealthKit） |
| 開発 | Ibragim Ibragimov |
| App Store ID | `6758967932` |
| ウィジェット | 小・中・大、背景色カスタム |

**デザイン言語（一言）**: **ライムイエロー × 黒 2 色ピクセル UI × 歩数 KPI + ペット成長**。Game Boy / Tamagotchi 系の低解像度美学で、実世界の歩数をゲーム状態に直結。

---

## ウィジェット前提

| 仮定 | 根拠 |
|---|---|
| **Medium 横長** | 左 ~60% メトリクス、右 ~40% ペット |
| データ源 | Apple HealthKit 当日歩数 |
| 表示例 | 7,938 歩 / GOAL 7,000 — **目標達成状態** |

---

## 1. カラーシステム（hex 推定）

| トークン | 推定値 | 用途 |
|---|---|---|
| `--widget-bg` | `#DFFF5E` 〜 `#E6F86F` | 全面ライム/ chartreuse |
| `--ink` | `#000000` | 文字・バー・枠・ペットアウトライン |
| `--pet-fill` | `#FFFFFF` | うさぎ本体 |
| `--badge-bg` | `#000000` | 「OK!」逆転バッジ |
| `--badge-text` | `#FFFFFF` | OK! 文字（ネガ） |
| `--progress-fill` | `#000000` | 目標達成時プログレス全面 fill |
| `--dot-inactive` | `#000000` 30–50% | XP 未充填ドット |

**原理**: **背景 1 色 + ink 1 色** の極端な 2 色制。アクセント色を増やさず、**fill 率とバッジ** で状態を伝える。

---

## 2. タイポグラフィ

| 役割 | 推定 | 備考 |
|---|---|---|
| ラベル `TODAY` / `GOAL` / `XP` | ピクセル等幅、小 caps | 英字 |
| 主数字 `7,938` | 太めピクセル、tabular | カンマ区切り |
| 日本語 `もくひょう たっせい` | 同系ピクセル風ひらがな | スペース区切り可 |
| ペット名 `ふわり` | 小さめ | 右下 |
| メタ `BABY • DAY 1` | 最小 caps | 進化段階 |

**原理**: **1 ファミリー（モノスペースピクセル）** で数字も日本語も統一。サイズだけで階層。

---

## 3. 角丸・スペーシング

| 要素 | 推定 |
|---|---|
| ウィジェット外周 | OS 標準 ~22–28px |
| ゴールプログレスバー | 矩形、**二重線ボーダー**（外枠 + 内側 inset 1px） |
| OK バッジ | ~20px 正方形、角 ~2–4px（ピクセル角） |
| XP セグメント | 5 マス正方形 ~8–10px + 9 ドット ~4px |
| 左カラム padding | ~12–16px |
| ペット床 | 水平ドットライン（地面） |

---

## 4. コンポーネント inventory

### 左カラム（データ）

| ブロック | 内容 |
|---|---|
| ヘッダ行 | `TODAY` 左 / `GOAL 7,000` + **♥♥**（2 心） |
| ヒーロー数字 | 当日歩数（大） |
| ゴールバー | 横長、達成時 **100% 黒 fill** |
| ステータス行 | 黒 `OK!` バッジ + `もくひょう たっせい` |
| XP 行 | `XP` + 5 实心マス + 9 ドット + `>` + **`KID`**（次進化名） |

### 右カラム（ペット）

| 要素 | 内容 |
|---|---|
| スパークル | 4 点ダイヤ（達成/ご機嫌演出） |
| スプライト | 白うさぎ、右向き、黒 1px アウトライン |
| 地面 | 点線ベースライン |
| メタ | 名前 + `BABY • DAY 1` |

---

## 5. 状態設計

| 状態 | 視覚（推定） |
|---|---|
| **目標未達** | プログレス partial fill、OK バッジなし、スパークル弱/なし |
| **目標達成**（スクショ） | バー full black、OK! + 達成コピー、♥、スパークル |
| **進化準備** | XP マスが増え、`KID` 等次ステージラベル表示 |
| **ペット段階** | `BABY` → `KID` … ラベルで明示 |

歩数でペットを「起こす」 — 未達時は sleep/sad スプライト（App Store 説明）との対比。

---

## 6. マスコット・ゲームメタ

| パターン | 役割 |
|---|---|
| ペット = 歩数の感情化 | 数字だけでなく **キャラの居場所** を右に確保 |
| 進化ラベル | BABY / KID — progression の **読み物最小化** |
| DAY N | 日次リズム（ストリーク系） |
| XP マス + ドット | 週次/長期進行の **コンパクトゲージ** |

---

## 7. PK パターン mapping

| PK レイヤ | この UI が支えること |
|---|---|
| **visual** | 2 色ピクセル、二重枠バー、OK ネガバッジ |
| **information** | 左 KPI スタック + 右 emotional — **デュアルカラム読み順** |
| **habit** | 日次 GOAL、DAY N、HealthKit 背景同期 |
| **progression** | BABY→KID、XP マス |
| **feedback** | 達成時バー fill + OK! + スパークル |

---

## 横断原理（widget 一般化）

1. **実 metric + ゲーム UI を同面** — 開かずに「数」と「気持ち」両方
2. **達成 = 幾何 fill + 2–3 語** — 長い祝福文は不要
3. **次の欲（KID）を常時チラ見せ** — XP 行末尾

---

## 蒸留マップ

| 要素 | 移行先 |
|---|---|
| ピクセル 2 色・バー・バッジ | `patterns/visual/candidates.md` |
| 歩数/目標/達成 hierarchy | `patterns/information/candidates.md` |
| ペット・XP・進化・DAY | `patterns/habit/candidates.md` + `patterns/progression/`（進化は progression 候補にも可） |

→ [patterns/visual/candidates.md](../patterns/visual/candidates.md), [patterns/habit/candidates.md](../patterns/habit/candidates.md)

---

## 自社への転用メモ（未検証）

| アプリ | 借りやすい要素 |
|---|---|
| もふ天気 | ライム/天気 1 色背景 + ピクセル masc |
| Forbear | 週次歩数ではなく「今日の self-care 1 目標」+ OK バッジ |
| Words For Me | 1 問達成で XP マス 1 つ — 同レイアウト転用 |
