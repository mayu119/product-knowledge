# ドパドリル（dopa-drill）— ビジュアル・UI・モーション設計の分解

- 出典: https://github.com/grmchn/dopa-drill
- 関連: [快感・コアループ解析](./2026-09-27-dopa-drill.md)
- 主要ファイル: `app/style.css`, `app/index.html`, `app/js/dopakichi.js`, `app/js/guide.js`, `docs/dopakichi.svg`
- 解析日: 2026-09-27

## デザイン言語（一言）

**ネオブルータリスト × 工作紙 × 算数ノート**。太い `#1b1d4d` アウトライン、紙の影（`box-shadow: 0 Npx 0 ink`）、丸ゴシック＋ディスプレイ書体。子供向けだが「安っぽいキラキラ」ではなく **手作り感のある紙工作**。

---

## 1. カラーシステム

### CSS トークン（`:root`）

| トークン | 値 | 用途 |
|---|---|---|
| `--ink` | `#1b1d4d` | 輪郭・本文・影 |
| `--paper` | `#fff8ec` | 背景紙 |
| `--blue` | `#3b6bff` | 主 CTA・進行 |
| `--pink` | `#ff7ab6` | コンボ・強調・アクセント |
| `--yellow` | `#ffd23f` | ドパ表示・選択・フォーカス |
| `--mint` | `#3fdcb0` | 装飾・粒子 |
| `--violet` | `#a77bff` | おしいカウント等 |
| `--red` | `#ff4f6d` | 急ぎ・危険・削除 |

### ドパキチ正典色（SPEC / `dopakichi.js`）

| 部位 | 色 |
|---|---|
| 体 | `#FF97BF` |
| 顔・腹 | `#FFF3E4` |
| 耳内 | `#FFE6F0` |
| 足 | `#2F79F7` |
| 輪郭 | `#000000` |

**原理**: アクセントは多色（6 色パレット）だが、**ink 1 色で統一した輪郭**がブランドを固定。背景は `--paper` + 方眼グリッドで「算数プリント」文脈。

---

## 2. タイポグラフィ

| 役割 | フォント | 使い所 |
|---|---|---|
| `--round` | Zen Maru Gothic Bold/Black | UI 本文、ラベル、説明 |
| `--chunky` | Dela Gothic One | ロゴ、得点、ドパ、大ボタン、見出し |

- ローカル WOFF2 サブセット（`app/fonts/`）。SIL OFL
- 数字は `--chunky` + `font-variant-numeric: tabular-nums`（時計）
- ひらがな UI（「じぶんレベル」「おしい」）— 学年向けに漢字を抑えたラベル設計

**原理**: **2 書体だけ** — 読む用（丸ゴ）と「効いた数字・タイトル用（デコラ）」。混ぜすぎない。

---

## 3. ロゴ・タイトル画面

`index.html` + `.logo` CSS:

- **紙切り風レイヤー**: 「ド」「パ」が別角度で hop アニメ、`::after` で二色クリップ（青/ピンク）
- **リボン**: SVG パスで折れたリボン上に「ドリル」
- **周辺演算子**: ＋×÷− が `logo-float` で浮く
- **バースト**: 回転する黄色/ピンクの burst SVG が背面

**原理**: 静止画ロゴではなく **CSS アニメで常に生きているタイトル**。初回起動前から「盛り上がる」予告。

---

## 4. コンポーネントパターン

### ボタン階層

| クラス | 見た目 | 用途 |
|---|---|---|
| `.big-btn` | 青塗り、68px 高、chunky 28px、影 7px | 主アクション（じぶんレベル、つぎへ） |
| `.sub-btn` | 白、52px、影 5px | 副次（スキルツリー、もどる） |
| `.pick button` | 白/chunky 数字、選択で yellow + 傾き | 問題数 6/10/14 |
| `.icon-btn` | 48px 円、設定・ヘルプ | タイトル右上 |

共通: `border: 3px solid ink` + **押下で translateY(5px) + 影縮小** — 物理ボタン感。

### モーダル

- 暗幕 `rgba(27,29,77,.45)`
- カード: `border-radius: 26px`, `box-shadow: 0 8px 0 ink`
- 破壊的操作: `.sub-btn.danger` 赤、確認 2 段階

### HUD（プレイ画面）

| 要素 | デザイン |
|---|---|
| `.pip` | 14px 丸 — 未/現在(yellow pulse)/完了(blue) |
| `.dopa` | yellow チップ、lv6 で虹シーン gradient |
| `.combo` | 白チップ + ピンク数字 + 下部プログレスバー |
| `.clock` |  pill 型。extra モードは ink 背景 + yellow 数字 |

---

## 5. レイアウト・アーキテクチャ

```
z-index 層（下→上）:
  #paper (方眼背景)
  #bg WebGL / #rays-fallback
  #fx-back, #fx (Canvas 粒子)
  #app (max 460px 中央、100dvh)
  #actors (ドパキチ)
  #cutins, #flash
  .modal / #guide
```

- **モバイル縦 460px カラム** — PC でも中央寄せ、数字キー対応
- **safe-area-inset** — テンキー最下段、タイトル padding
- **700px 以下** — 問題紙・テンキー再計算、キー高 48px 維持

**原理**: 演出 Canvas は `#app` の外（全画面）。UI は狭いカラムに固定し **入力領域を常に同じ位置** に。

---

## 6. マスコット（ドパキチ）

`dopakichi.js` + `docs/dopakichi.svg`:

| 設計 | 内容 |
|---|---|
| 造形 | rubber-hose。横耳、広い頭、二重円の目、細腕・青足 |
| 表現 | **ノンバーバル**（台詞なし）。表情セット（open/happy/x/swirl/star/heart 等） |
| 動き | Spring 物理、左右手独立運搬、誤答 10 種演技 |
| カスタム | 8 色 + 9 衣装（帽子/マント/王冠等）— **本体 SVG は不変、overlay のみ** |
| 数字 | `.dk-digit` — stroke 付き fill、手元で運ぶ |

**原理**: キャラは **表情語彙 + 物理** で話す。ガイドでも「台詞カード」ではなく **UI カード + ポーズ指差し**（`guide.js` コメント: captions belong to interface, never mascot）。

---

## 7. ガイド / オンボ視覚

- スポットライト: SVG mask で穴開け + yellow `#ffd23f` リング
- 説明カード: 独立 `.modal-card` 風、ドパキチは `#guide-actor` SVG で配置
- 障害物回避: `rankGuideSpots` でマスコットとラベル被りを計算
- 7 ページ（初回）/ 設定から再表示

**原理**: Growth Knowledge `onboarding/` の「到達」とは別層。**製品内ツアーの visual pattern** — PK `patterns/visual/` + `patterns/information/`。

---

## 8. 演出強度と CSS クラス連動

`body.lv0` … `body.lv9`（JS が演出強度 E から付与）:

| レベル | 視覚変化の例 |
|---|---|
| lv3 | `.bunting` 旗表示 |
| lv4 | テンキー各キーにパステル色 |
| lv5 | `.spot` スポットライト |
| lv6 | `.dopa` 虹グラデーション sheen |
| lv8 | テンキー glow、`.marquee` 背景 |
| lv9 | `.card` 問題紙に pink リング + glow |

**原理**: 快感エスカレーション（core-loop）を **CSS クラス 1 本で UI 全体に伝播**。コンポーネント個別ではなく `body.lvN` グローバル設計。

---

## 9. スキルツリー UI

| 状態 | 見た目 |
|---|---|
| locked | グレー + 鍵アイコン CSS |
| new | yellow 背景 + NEW バッジ pulse |
| learning | 青系 `#e3ebff` |
| mastered | 金グラデ + ★ |
| さび | （ノード上の表示 — SPEC 参照） |
| 長押し削除 | ピンク fill 600ms で rising |

**原理**: **状態 = 色 + 小バッジ + アニメ** の 3 点セット。テキスト説明なしでも判別可能。

---

## 10. モーション・アクセシビリティ

| 機能 | 実装 |
|---|---|
| `prefers-reduced-motion` | `body.reduced` — logo/node/demo アニメ停止 |
| 設定「動きの強さ」 | 0–100%。粒子・揺れ・閃光・ドパキチ大動作をスケール |
| `:focus-visible` | yellow 4px outline |
| `aria-*` | ボタンラベル、ガイド dialog、grid role |

**原理**: 派手さと **止められる権利** をセット。判定・得点は動き OFF でも同じ（SPEC 明記）。

---

## 11. 粒子・背景（fx / bg）

- 粒子色: `#ff7ab6, #3b6bff, #ffd23f, #3fdcb0, #a77bff, #ff5a4f`
- テーマ差し替え: コレクション（音符/花びら/数字/泡/お菓子）
- WebGL 不可時: CSS `#rays-fallback` conic-gradient

---

## 横断原理（visual 蒸留候補）

1. **ink 統一アウトライン** — 多色アクセントでも輪郭 1 色でブランド固定
2. **物理ボタン影** — translateY + bottom shadow で押した感
3. **2 書体ルール** — 本文丸ゴ + 数字/見出しデコラ
4. **body グローバル lv** — セッション進行を UI 全体のクラス 1 本でエスカレート
5. **マスコットは overlay** — 本体形状不変、色/衣装/表情だけ差し替え
6. **ガイドは UI が語る** — マスコット台詞にしない、スポットライト + カード
7. **reduced motion 二段** — OS 設定 + アプリ内スライダー

→ [patterns/visual/guide.md](../patterns/visual/guide.md) に蒸留。

---

## 自社への転用メモ（未検証）

| アプリ | 借りやすい要素 |
|---|---|
| もふ天気 | 丸ゴ + 1 デコラ書体、ink 輪郭の weather カード |
| Words For Me | 問いカードの paper shadow、着地時の lv エスカレーション |
| ホンネカード | リビール時 flash + 控えめ spot（課金 UI は別） |
| Forbear | 週次レポート完了時の body.lv 的グローバル演出 |

---

## 蒸留マップ

| 要素 | 移行先 |
|---|---|
| カラー・タイポ・ボタン | `patterns/visual/guide.md` |
| ガイド spot UI | `patterns/information/candidates.md` |
| lv エスカレーション | `patterns/visual/` + `patterns/core-loop/`（連動） |
