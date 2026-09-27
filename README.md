# Product Knowledge

全アプリ横断で再利用する、体験設計・コアループ・習慣化・フィードバック・ドメインロジックの知見ベース。

[Growth Knowledge](https://github.com/mayu119/growth-knowledge) が「転換・課金・獲得・計測」なら、こちらは **転換の前** — ユーザーが価値を感じ、続けたくなる体験と、その裏のルールを蓄積する。

## Growth Knowledge との境界

| 問い | 置き場 |
|---|---|
| コアループ・快感・習慣化・情報設計は正しいか | **この repo** |
| ペイウォール・価格・trial・広告・UGC 獲得 | [Growth Knowledge](https://github.com/mayu119/growth-knowledge) |
| 施策を試した結果・CVR・adopt 判定 | Growth Knowledge `apps/` + `measurement/` |
| 体験パターンを採用した・実装した | この repo `apps/`（原理）→ GK `apps/`（数字） |

**重複を避けるルール**

- 外部事例・競合・OSS の解析 → `references/`（原文・要約）
- 横断で使える原理 → `patterns/<theme>/guide.md`
- 未蒸留の候補 → `patterns/<theme>/candidates.md`
- アプリ固有の体験判断・ドメインルール → `apps/<app>.md`
- 実験結果・売上・インストール数 → Growth Knowledge へ。ここには書かない

## 2層構造

- **横断層**: `patterns/`。アプリに依存しない体験原理、設計パターン、参照実装の分解、出典 URL。
- **アプリ固有層**: `apps/`。横断知見をそのアプリに採用したか、どう実装したか（計測結果は GK へ）。

横断原理とアプリの実装メモを混ぜない。

## どっちの repo を見るか（30秒）

```
転換・課金・広告・実験の数字が知りたい     → Growth Knowledge
体験・ループ・演出・状態・ドメインルール    → Product Knowledge
両方                                      → Product で設計 → GK で検証
```

## テーマディレクトリ

| 場所 | 中身 |
|---|---|
| [`patterns/core-loop/`](patterns/core-loop/README.md) | セッション構造、報酬粒度、エスカレーション、失敗設計 |
| [`patterns/feedback/`](patterns/feedback/README.md) | 即時フィードバック、マイクロ報酬、音・動き |
| [`patterns/progression/`](patterns/progression/README.md) | 解放、マスター、コレクション、星・実績 |
| [`patterns/habit/`](patterns/habit/README.md) | ストリーク、デイリー、復帰、さびつき |
| [`patterns/information/`](patterns/information/README.md) | 画面構成、状態表示、エラー、オンボ以外の案内 |
| [`references/`](references/README.md) | 外部事例・競合・OSS の解析（未蒸留） |
| [`apps/`](apps/README.md) | アプリごとの体験採用・ドメインルール |

## 運用ルール（Growth Knowledge と同型）

1. **拾う**: 事例・競合・OSS を `references/` に日付付きで保存
2. **候補にする**: 横断知見は各テーマの `candidates.md` に出典付きで追記
3. **蒸留する**: 精査後に `patterns/<theme>/guide.md` へ。未検証の海外ポストだけで正本を書き換えない
4. **アプリに降ろす**: `apps/<app>.md` に採用判断・実装メモ
5. **数字は GK へ**: 施策を出荷して計測したら Growth Knowledge の `apps/` と `measurement/experiments.md` に結果を書く

## ドキュメント

| ファイル | 内容 |
|---|---|
| [docs/INITIAL-DESIGN.md](docs/INITIAL-DESIGN.md) | repo 立ち上げの設計正本（GK との分離理由・ディレクトリ・運用） |
| [references/2026-09-27-dopa-drill.md](references/2026-09-27-dopa-drill.md) | 第 1 号参照実装の快感設計分解 |

## 最初の参照実装

- [ドパドリル（dopa-drill）解析](references/2026-09-27-dopa-drill.md) — コアループ・快感設計の分解（[初期設計](docs/INITIAL-DESIGN.md) 策定のきっかけ）
