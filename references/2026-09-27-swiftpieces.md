# SwiftPieces — SwiftUI UI コンポーネント OSS の分解

- 出典: https://github.com/Saivion/SwiftPieces
- サイト: https://swiftpieces.com
- コンポーネント一覧: https://swiftpieces.com/components
- CLI: https://swiftpieces.com/docs/cli
- 解析日: 2026-09-27
- きっかけ: iOS 向け UI ピースの横断参照。Product Knowledge では **コンポーネント・演出の候補** として蓄積（アプリ固有実装は `apps/` へ降ろさない）。
- 詳細タクソノミー: [swiftpieces-taxonomy.md](/cursor/stores/self/docs/swiftpieces-taxonomy.md)（55 ピース全件・CLI・レジストリ構造）

## 概要

**SwiftPieces** は、SwiftUI 向けアニメーション付き UI コンポーネント（「ピース」）を **1 ファイル = 1 コンポーネント** で提供する OSS ライブラリ。SPM 依存ではなく、CLI またはサイトから `.swift`（必要時 `.metal`）をコピーし、**コードを完全に所有** する設計。

中核は **55 個の SwiftUI ピース（14 カテゴリ）**。iOS 17 以上・Apple フレームワークのみ・Reduce Motion / Dynamic Type 対応が共通前提。付随資産として npm CLI、Next.js ドキュメントサイト、Xcode プレビューハーネス、レジストリ生成パイプラインがある。

---

## 一言で言うと

**「画面テンプレート」ではなく「触覚的 UI 部品のカタログ」**。コアループや進行設計は含まない。ボタン・カード・リスト・フィードバック・Liquid Glass 等の **実装済み SwiftUI ピース** を借りて、自社アプリの体験層を速く作るための OSS。

---

## 採用方法

```bash
# 初回（省略可 — add が自動 init も可）
npx swiftpieces init

# ピース取得（PascalCase 名）
npx swiftpieces add SwipeDeck CommitButton GlassActionMenu

# Metal 付きピース（.swift + .metal が同梱）
npx swiftpieces add AssistantOrb Silk
```

| 項目 | 内容 |
|---|---|
| **CLI** | `npx swiftpieces add <Name>` — Node 18.17+、Xcode 16+ |
| **手動** | [swiftpieces.com/components](https://swiftpieces.com/components) からコピー |
| **配置** | デフォルト `SwiftPieces/` — Xcode フォルダ同期グループで自動認識 |
| **カスタム** | 各ピースの `Style` 構造体で色・角丸等を調整。`#Preview` が用法の正本 |
| **更新** | アップストリーム更新は手動または CLI 再取得（所有権モデル） |

---

## 制約・前提

| 制約 | 詳細 |
|---|---|
| **最低 iOS** | 17.0（全 55 ピース） |
| **Swift Tools** | 6.1 |
| **Metal 3** | 3 ピース — `AssistantOrb`, `ThoughtOrb`, `Silk`。`.metal` を同一ターゲットに追加必須 |
| **Liquid Glass** | 5 ピース — iOS 26+ で Liquid Glass、`#available` + Material フォールバック |
| **iOS 18 機能** | `StreamingReply` — `TextRenderer` は iOS 18+、17 はフォールバック |
| **第三者依存** | SPM / CocoaPods 不要。`registryDependencies` / `spmDependencies` は現状すべて空 |
| **ライセンス** | **MIT + Commons Clause** — アプリへの組み込み・改変は可。**ピース単体の再販・再配布・ポート版の販売は不可** |
| **Pro** | 各ピースの `pro: "<screen-id>"` は Pro 画面への導線。**Pro 本体（画面テンプレート・Build Kit）は有料・非公開** |

---

## 55 ピース概览（14 カテゴリ）

| カテゴリ | 数 | ピース名 |
|---|---:|---|
| **AI** | 6 | AssistantOrb, CodeBlock, PromptChips, StreamingReply, ThinkingState, ThoughtOrb |
| **Backgrounds** | 2 | Silk, TouchGrid |
| **Cards** | 4 | FlipCard, MotionCard, ParallaxCard, SwipeDeck |
| **Controls** | 5 | CommitButton, ElasticButton, FanStack, HoldToConfirm, TimerDial |
| **Data** | 4 | LiveStat, Odometer, RingBreakdown, ScrubChart |
| **Feedback** | 5 | OutcomeScreen, RatingScrub, ReactionToggle, SkeletonLoader, StatusMorph |
| **Glass** | 3 | GlassActionMenu, GlassSegments, GlassSurface |
| **Inputs** | 9 | AmountField, DateRangePicker, ExpandingTrack, FilterRail, FormField, RangeSlider, ScrubStepper, SecureEntry, TokenField |
| **Lists** | 5 | DepthCarousel, PagedList, StatusTimeline, SwipeActionRow, TaskRow |
| **Media** | 2 | PhotoViewer, StoryStrip |
| **Motion** | 1 | DragToDismiss |
| **Navigation** | 3 | FloatingDock, StretchHeader, TrackingTabs |
| **Sheets** | 3 | ConfirmSheet, PermissionSheet, Toast |
| **Text** | 3 | ExpandableText, GlassText, TextReveal |
| **合計** | **55** | 🆕 直近 30 日追加 7 件（AmountField, DateRangePicker, FormField, RangeSlider, TokenField, PagedList, ExpandableText） |

### カテゴリ別の要点

| カテゴリ | 借りやすい用途 |
|---|---|
| AI | チャット/アシスタント UI（思考状態、ストリーミング返答、プロンプトチップ） |
| Backgrounds | オンボーディング・ヒーロー背景（Metal シルク、タッチ反応グリッド） |
| Cards | フラッシュカード、ウォレットカード、スワイプ意思決定 |
| Controls | 非同期 CTA、破壊的操作確認、タイマー設定 |
| Data | KPI タイル、カウンター、スクラブ可能チャート |
| Feedback | 成功/失敗/空、スケルトン、リアクション |
| Glass | iOS 26 Liquid Glass 系（ドック、セグメント、アクションメニュー） |
| Inputs | フォーム、通貨、日付範囲、タグ、スライダー |
| Lists | 無限スクロール、スワイプ行、タイムライン |
| Media | 写真ビューア、Stories UI |
| Motion | シート/カードのドラッグ dismiss |
| Navigation | フローティングタブ、ストレッチヘッダ、追従タブ |
| Sheets | 確認・権限・トースト |
| Text | リビール見出し、省略展開、Glass テキスト |

---

## 借りるもの vs 期待しないもの

### 借りる（Product Knowledge 向け）

| 資産 | 内容 |
|---|---|
| **55 SwiftUI ピース** | 触覚・モーション付き UI 部品。Reduce Motion 対応済み |
| **CLI** | `npx swiftpieces add` — プロジェクトへの即投入 |
| **設計ガイド 6 本** | Animations / Buttons / Cards / Haptics / Loading States / Liquid Glass — コードではなく設計知識 |
| **#Preview** | 各ピースの用法・パラメータの正本 |
| **Style 構造体** | 多くのピースで色・形状をカスタム可能 |

### 借りない / 期待しない

| 項目 | 理由 |
|---|---|
| **アプリアーキテクチャ** | MVVM、状態管理、ナビゲーション設計は含まない |
| **Swift Pieces Pro** | 画面テンプレート・Build Kit・エージェントスキルはクローズドソース |
| **コアループ・進行・習慣化** | ゲームループ、ストリーク、解放設計は対象外 |
| **Web サイト基盤** | `app/`, `components/previews/` は swiftpieces.com 専用。iOS アプリには不要 |
| **@swiftpieces/brand** | Tailwind v4 + React — Web フォーク向け。iOS には不要 |
| **計測・CVR** | Growth Knowledge の領域 |

---

## 横断原理（蒸留候補の方向性）

Product Knowledge の `patterns/` への候補は **UI 部品・演出** 中心。コアループ / progression / habit への該当は最小（ゲーム設計ではないため）。

| 原理の方向 | 代表ピース | 蒸留先 |
|---|---|---|
| Liquid Glass + Material FB | GlassSurface, FloatingDock, GlassText | `patterns/visual/` |
| 非同期 CTA 状態遷移 | CommitButton, StatusMorph | `patterns/feedback/` |
| 触覚的ボタン | ElasticButton, HoldToConfirm | `patterns/visual/` |
| スケルトン・完了画面 | SkeletonLoader, OutcomeScreen | `patterns/feedback/` |
| フォーム入力パターン | FormField, SecureEntry, TokenField | `patterns/information/` |
| リスト・ナビ | PagedList, SwipeActionRow, TrackingTabs | `patterns/information/` |
| Reduce Motion 二段 | 全ピース共通 | `patterns/visual/`（dopa-drill 候補と同型） |

---

## 自社への転用メモ（未検証）

| アプリ | 借りやすいピース |
|---|---|
| もふ天気 | LiveStat（気温 KPI）、SkeletonLoader（予報ロード）、StretchHeader |
| Words For Me | TextReveal（問い見出し）、OutcomeScreen（着地）、ExpandableText |
| ホンネカード | FlipCard（表裏）、HoldToConfirm（公開確認）、ReactionToggle |
| Forbear | StatusTimeline（週次進捗）、Odometer（カウント）、Toast |

---

## 蒸留マップ

| 要素 | 移行先 |
|---|---|
| ボタン・カード・背景・モーション・タイポ | [patterns/visual/candidates.md](../patterns/visual/candidates.md) |
| 成功/失敗・ローディング・進捗・ハプティック FB | [patterns/feedback/candidates.md](../patterns/feedback/candidates.md) |
| リスト・フォーム・ナビ・設定 UI | [patterns/information/candidates.md](../patterns/information/candidates.md) |
| コアループ・進行・習慣 | 該当最小 — 本参照では候補追加なし |
