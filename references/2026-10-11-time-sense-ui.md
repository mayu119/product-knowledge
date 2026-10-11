# TimeSense — キッズビジュアルタイマー UI 分解（部類・パクリ先）

- 出典: App Store [Kids Visual Timer: TimeSense](https://apps.apple.com/us/app/kids-visual-timer-timesense/id6747689461) / 開発者サイト [SynCraft TimeSense](https://syncraft.dev/timesense/en/)
- スクリーンショット: [time-sense-ui.png](/cursor/stores/bc-01a0e4fe-67b4-782e-aa4f-6290f907d0b3/media/time-sense-ui.png)
- 解析日: 2026-10-11
- 注: 画面上ロゴは **「Time Sense」**（2 語）。App Store 表記は **Kids Visual Timer｜TimeSense**。同名の勤怠アプリ [Time Sense（Ubrixy）](https://apps.apple.com/ae/app/time-sense/id6478931778) とは別物 — 本 UI は SynCraft 版と一致（キリン masc・扇形タイマー）。

## アプリ同定

| 項目 | 内容 |
|---|---|
| 名称 | **Kids Visual Timer: TimeSense**（ロゴ: Time Sense） |
| カテゴリ | 教育 / キッズ — ビジュアルタイマー・ルーティン |
| プラットフォーム | iPhone / iPad（iOS） |
| 開発 | **Shinpei Ohno**（SynCraft） |
| App Store ID | `6747689461` |
| キャラ | Gentle Giraffe ほか（Wolf / Elephant / Chicken） |

**デザイン言語（一言）**: **パステル空 × コーラル 1 アクcent × 二重円（時計 + 扇形タイマー）× キリン masc** — 「残り時間」を扇形で体感させる、オシャレな時間可視化 UI のレベル感（部類参照。ピクセル一致のコピー対象ではない）。

---

## パクリ用 UI 仕様（部類トークン）

### 1. カラーシステム（hex 推定）

| トークン | 推定値 | 用途 |
|---|---|---|
| `--sky-bg` | `#D4ECFF` 〜 `#E8F4FC` | 全面空色グラデ |
| `--cloud` | `#FFFFFF` | 雲シルエット |
| `--hill` | `#7BC67E` 〜 `#8FD493` | 前景の丘 |
| `--ink-primary` | `#1E2A44` 〜 `#243B55` | 12h 数字・時針・ロゴ |
| `--ink-secondary` | `#8FA3B8` | 24h 内側数字 |
| `--accent-coral` | `#F28B82` 〜 `#FF9B8A` | 扇形タイマー・秒針・強調分 dot |
| `--hand-minute` | `#7EB8DA` 〜 `#9CCAE8` | 分針 |
| `--ring-minute-dot` | `#B8D4E8` | 5 分刻み外周 |
| `--surface-clock` | `#FFFFFF` + 薄 shadow | 下段タイマー円 |
| `--header-icon-bg` | `#FFFFFF` / `#EAF6FF` | 丸アイコン底 |
| `--header-cta` | `#1E2A44` | 右端 chevron 丸（primary fill） |

**原理**: 背景は **低彩度パステル 1 系**、インタラクションと「時間が減っている」は **コーラル 1 色** に集約。ネイビーは読み物、水色は針・補助。

### 2. タイポグラフィ

| 役割 | 推定 | サイズ感 |
|---|---|---|
| ロゴ「Time Sense」 | 丸み Sans（Varela Round / Nunito 系） | ~18–22 pt |
| 12h リング数字 | Bold、ネイビー | ~28–36 pt（外周） |
| 24h 内側 | Regular、グレー | ~14–18 pt |
| タイマー外周 60/15/30/45 | Bold、ネイビー | ~20–24 pt |
| 中央デジタル `30:02` | Bold tabular、ネイビー | ~36–44 pt |

- **原理**: 数字は **太さ + サイズ** だけで階層。装飾書体はロゴのみ。

### 3. レイアウトゾーン（縦スタック）

| ゾーン | 高さ感 | 内容 |
|---|---|---|
| **Header** | ~8–10% | 左ロゴ / 右 **5 連丸アイコン**（履歴・カレンダー・音量・設定 + primary CTA） |
| **Analog clock** | ~35–40% | **三重リング** アナログ時計（現在時刻） |
| **Visual timer** | ~40–45% | 大円 + **コーラル扇形** + 中央デジタル |
| **Scene footer** | ~10–15% | 丘 + **キリン masc**（右下、アンカー） |

水平方向は **2 大円を中央揃え** — 上が「今」、下が「この区間の残り/経過」。

### 4. コンポーネント inventory

#### 丸アイコンヘッダー

| 要素 | 仕様 |
|---|---|
| タッチ面 | 直径 ~40–44 px 円 |
| 通常 | 白 or 極淡青底 + ネイビー線アイコン |
| CTA | ネイビー fill + 白 chevron |

#### 三重リング時計

| リング | 内容 |
|---|---|
| 外周 | 分 dot + **5 分ごと数字**（0,5,10…）— アクティブ分は **コーラル丸** |
| 中 | **1–12** 太字（12h） |
| 内 | **13–24** 薄字（24h 対応） |
| 針 | 太時針（ネイビー）/ 中分針（水色）/ 細秒針（コーラル） |

#### ビジュアルタイマー（下段）

| 要素 | 仕様 |
|---|---|
| ベース | 白円、軽い drop shadow |
| 扇形 | **コーラル sector** — 経過 or 残り（スクショは ~180° = 半円） |
| 目盛 | 12/3/6/9 に **60 / 15 / 30 / 45** + 短 tick |
| 中心 | 白小円 + **MM:SS** + 下に **リセット/同期** 丸アイコン |

#### マスコット（キリン）

| 用法 | 実装 |
|---|---|
| 配置 | 前景丘の **右下** — UI 円と重ならない |
| 役割 | ブランド・「優しい案内」 — タイマー操作本体ではない |
| スタイル | フラット illustration、暖色イエロー + 茶 spots |

---

## 部類として借りるもの vs そのまま真似しないもの

### 部類として借りるもの（レベル感・構造）

| 要素 | 借り方 |
|---|---|
| **二段円レイアウト** | 上 = 文脈時刻 / 下 = タスク時間 — 同じ「円」語彙で役割分担 |
| **扇形 progress** | 残り時間を **角度 fill** で見せる — 数字だけより子供・非エキスpert向き |
| **アクcent 1 色（コーラル系）** | 秒針・sector・強調 dot を同色 — 「時間の緊張」が一瞬で読める |
| **三重リングの情報密度** | 12h + 24h + 分目盛を **同心円** で — 画面を増やさない |
| **丸アイコンヘッダー** | 設定系を **等径円** に並べ、視線を主 UI に通す |
| **空 + 雲 + masc 丘** | 機能 UI は白円、**情緒は背景レイヤ** — yatta の watermark より「シーン」寄り |
| **中央デジタル + 周辺アナログ** | 正確さ（数字）と intuition（扇形）の **二重符号化** |

### そのまま真似しないもの（適応必須）

| 要素 | 理由 |
|---|---|
| **Time Sense ロゴ・キリン IP** | SynCraft ブランド資産 — masc は自社キャラへ差替 |
| **60/15/30/45 の 4 点ラベル** | キッズ 1 時間タイマー前提 — 自社は 5/10 分 or ポモドーロ等に再設計 |
| **5 アイコン機能セット** | 履歴/カレンダー/音量/設定は製品機能に依存 — 数と意味は自社定義 |
| **パステル空の exact hex** | 部類は「明るい空」で足りる — もふ天気等は自社 palette |
| **24h 内リング** | 対象ユーザーが 12h のみなら削る — 情報過多になりうる |
| **App Store 上の voice / routine コピー** | UI 部類とは別 — 機能要件は自社 PRD |

---

## どれだけパクれるか（チェックリスト）

| 項目 | パクれる度 | メモ |
|---|---|---|
| **Design tokens**（空/ネイビー/コーラル/水色針） | ◎ 構造のみ | hex は推定で OK、自社 theme にマップ |
| **Layout pattern**（header row + 2 stacked circles + scene footer） | ◎ | 比率は端末で可変 |
| **Progress visualization**（sector fill） | ◎ | 角度 ↔ 残秒の式は自社実装 |
| **Triple-ring clock** | ○ | 2 リング化 or 1 リング + デジタルでも可 |
| **Circular icon header** | ◎ | 4+1 CTA パターン汎用 |
| **Mascot placement**（丘・右下） | ○ | キャラ形状は不可、**配置役割**のみ |
| **Typography scale**（大数字 + 中央 tabular） | ○ | フォントファイルは別 |
| **Cloud/sky illustration** | △ | 雰囲気参考 — 自社イラスト or 単色グラデでも可 |
| **Exact tick labels 60/15/30/45** | △ | ラベル体系だけ借り、数値は自社 |

凡例: ◎ そのままパターン化 / ○ 要簡略 / △  mood のみ

---

## PK パターン mapping

| PK レイヤ | この UI が支えること |
|---|---|
| **visual** | パステル空、コーラル sector、三重リング、丸ヘッダー、scene masc |
| **information** | **二層時間** — グローバル「今」とローカル「このタイマー」；12/24/分の同心階層 |
| **feedback** | 扇形 **連続減衰**、秒針・強調 dot、中央 MM:SS の **即時更新** |
| **habit** | （アプリ側）ルーティン・音声 45/30/15/1/10 秒 — UI 部類外 |
| **progression** | （アプリ側）ステッカー・メダル — 本スクショ範囲外 |

---

## 横断原理（時間 UI 一般化）

1. **扇形 = 残りの体積** — バーより「食べていく/減っていく」イメージが強い
2. **アナログ + デジタル同面** — 親は数字、子どもは色面積、両方満たす
3. **情緒は背景、masc は前景** — 操作 UI（白円）のコントラストを守る

---

## 蒸留マップ

| 要素 | 移行先 |
|---|---|
| 色・扇形・三重リング・scene | `patterns/visual/candidates.md` |
| 二層時間・12/24/分 hierarchy | `patterns/information/candidates.md` |
| sector + 秒針 + デジタル更新 | `patterns/feedback/candidates.md` |

→ [patterns/visual/candidates.md](../patterns/visual/candidates.md)

---

## 自社への転用メモ（未検証）

| アプリ | 借りやすい要素 |
|---|---|
| もふ天気 | 空背景 + 1 masc 丘 — 天気は扇形ではなく sun/cloud でも可 |
| Forbear | セッション残りを **sector**、上段に「今の気分」小時計 |
| Words For Me | 1 問制限時間を下段扇形 + 上段は時刻不要なら **1 円だけ** に簡略 |
| dopa-drill | 問題タイマーを sector — ink 統一色に扇形だけアクcent |
