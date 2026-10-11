# ビジュアル設計 — 原理（蒸留済み）

最終更新: 2026-10-11（time-sense 追記）

## ink 統一アウトライン

- アクセント色を何色使っても、**輪郭・影・本文ベースを 1 色**（dopa-drill: `#1b1d4d`）に固定するとブランドが崩れにくい
- 子供向けでも「キラキラ多色」より **太枠 + 紙影** の方が一貫する
- 出典: [dopa-drill デザイン分解](../../references/2026-09-27-dopa-drill-design.md)

## 物理ボタン（押下フィードバック）

- `border + box-shadow(下) + active 時 translateY` で、タップに **物理的な沈み** を返す
- 主/副/数字選択で **高さ・色・書体** を変え、階層を 3 段以内に
- **弾性 ButtonStyle** や **長押し確認** で、通常 CTA と破壊的操作の触覚差を付ける（誤タップ防止）
- 出典: [dopa-drill デザイン分解](../../references/2026-09-27-dopa-drill-design.md), [SwiftPieces](../../references/2026-09-27-swiftpieces.md)

## 2 書体ルール

- 本文・説明: 丸ゴシック（読みやすさ）
- 数字・スコア・ロゴ・主 CTA: ディスプレイ書体（効いた感）
- 出典: [dopa-drill デザイン分解](../../references/2026-09-27-dopa-drill-design.md)

## body グローバル lv でエスカレート

- セッション進行度を `body.lvN` 等 **1 クラス** で付け、各コンポーネントが CSS で反応する
- 個別ウィジェットより **全体が盛り上がる** 見え方になる（core-loop のエスカレーションと連動）
- 出典: [dopa-drill デザイン分解](../../references/2026-09-27-dopa-drill-design.md)

## マスコットは overlay

- 本体形状は不変、**色・衣装・表情・ポーズ** だけ差し替え
- ガイド文は UI カードが持ち、マスコットは指差し・表情のみ（台詞にしない）
- 出典: [dopa-drill デザイン分解](../../references/2026-09-27-dopa-drill-design.md)

## 派手さは止められる

- `prefers-reduced-motion` + アプリ内「動きの強さ」スライダーの **二段**
- 演出 OFF でもゲームロジック・得点は同じ
- UI 部品側でも Reduce Motion / Dynamic Type / セマンティックカラーを **部品内蔵** し、OS 設定に追従する
- 出典: [dopa-drill デザイン分解](../../references/2026-09-27-dopa-drill-design.md), [SwiftPieces](../../references/2026-09-27-swiftpieces.md)

## UI 部品は 1 ファイル 1 責務で所有

- 外部 UI ライブラリをブラックボックス依存にせず、**1 部品 = 1 ファイル** をプロジェクトに取り込み、コードを完全に所有する
- `#Preview` と `Style` 構造体を正本にし、部品間で **アクセシビリティ契約**（Reduce Motion、Dynamic Type、セマンティックカラー）を揃える
- 出典: [SwiftPieces](../../references/2026-09-27-swiftpieces.md)

## Liquid Glass は Material フォールバック必須

- 新 OS の Glass 効果は `#available` で分岐し、未対応端末では **Material 等の既存表現** に落とす
- Glass を主役にする画面ほど、フォールバック時の **情報可読性** を先に確認する
- 出典: [SwiftPieces](../../references/2026-09-27-swiftpieces.md)

## カードは深度・反転・スワイプで役割を分ける

- **深度/パララックス/チルト** — 注目・所有感（ウォレット、プロフィール、ギャラリー）
- **表裏反転** — 同一オブジェクトの 2 面（フラッシュカード、詳細切替）
- **スワイプスタック** — 意思決定・オンボーディングの「選ぶ/捨てる」
- 出典: [SwiftPieces](../../references/2026-09-27-swiftpieces.md)

## リッチ背景は degradable

- Metal シェーダーや Canvas 背景は **視覚的ハイライト** として使い、非対応・Reduce Motion 時は **静的グラデーション等** に落とす
- 背景演出と操作 UI は z 層で分離し、入力領域を背景に埋もれさせない
- 出典: [SwiftPieces](../../references/2026-09-27-swiftpieces.md), [dopa-drill デザイン分解](../../references/2026-09-27-dopa-drill-design.md)

## 固定幅カラム + 全画面演出

- 操作 UI は **狭い中央カラム**（例: 460px）に固定し、入力位置をセッション間で不変にする
- 粒子・WebGL・Canvas 演出はカラム外の **全画面レイヤ** に置く
- 背景は **紙質 + 方眼グリッド** 等、文脈を示すテクスチャで世界観を固定する
- 出典: [dopa-drill デザイン分解](../../references/2026-09-27-dopa-drill-design.md)

## ドラッグ dismiss で自然に閉じる

- シート・カード・全画面写真は **2 軸ドラッグ** で閉じられると、モーダル感が減り没入が保たれる
-  dismiss ジェスチャは Reduce Motion 時に **即時クローズ** へ切り替える
- 出典: [SwiftPieces](../../references/2026-09-27-swiftpieces.md)

## テキスト演出は情報量に応じて使い分け

- **リビール** — オンボーディング見出しなど、初見の 1 フレーズ
- **Glass 輪郭** — ヒーロータイトル（Liquid Glass 対応端末のみ）
- **省略 + 展開** — 長文キャプションは初期省略し「もっと見る」で段階開示
- 出典: [SwiftPieces](../../references/2026-09-27-swiftpieces.md)

## 進行状態は色・バッジ・アニメ

- locked / new / in-progress / mastered 等は **色 + 小バッジ + 短いアニメ** の 3 点セットで判別する
- 長文ラベルに頼らず、一覧スキャンで状態が読めるようにする
- 出典: [dopa-drill デザイン分解](../../references/2026-09-27-dopa-drill-design.md)

## ホーム画面ウィジェットは 1 アクセント + マスコット 2 役

- **CTA・選択・完了** を 1 色（例: オレンジ）に寄せ、ダークまたは高彩度背景 1 色と組み合わせる — 小面積でも状態が一目
- マスコットは **focal（ノッチ/右カラム）** と **ambient（中央 watermark）** の 2 スケール — リストが空でもブランドが残る
- 出典: [yatta! ウィジェット](../../references/2026-10-11-widget-yatta.md)

## ウィジェットの達成は fill + 短バッジ

- プログレスは **幾何 fill**（全面 or 部分）で十分。祝辞は **2–3 語 + 小バッジ**（例: OK!）に圧縮
- ピクセル/レトロ UI では **背景 + ink の 2 色** だけで階層を作り、アクセント色を増やさない
- 出典: [Steppy ウィジェット](../../references/2026-10-11-widget-steppy.md)

## ウィジェット内ナビは pill 3 段以内

- 底固定の **pill-in-pill セグメント** で 3 分類（今日/あとで/いつか 等）に上限 — タブを増やすよりアプリ本体へ誘導
- 出典: [yatta! ウィジェット](../../references/2026-10-11-widget-yatta.md)

## 残り時間は扇形 sector で体感化

- カウントダウンは **水平バー** だけでなく、円内 **コーラル（等 accent 1 色）の扇形** で「減っていく量」を見せる — 特に子ども・短時間タスク向き
- **中央デジタル（MM:SS）** と **周辺扇形** を同面に置き、精度と intuition を両立する
- 出典: [TimeSense UI 部類](../../references/2026-10-11-time-sense-ui.md)

## 時間スケールは同心リングで重ねる（必要分だけ）

- 12h / 24h / 分目盛を **三重リング** で 1 画面に — 使わないリングは削ってよい
- 強調中の分は **accent 丸**、秒針も同色にすると「今」が一瞬で読める
- 出典: [TimeSense UI 部類](../../references/2026-10-11-time-sense-ui.md)
