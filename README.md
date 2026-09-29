# Product Knowledge

全プロダクト横断で再利用する、プロダクト開発の知見ベース — 足場（スタック・ホスティング・認証配線）から、ドーパミン・習慣・依存設計、UI・体験まで。

```
PK: プロダクトをどう作るか（足場 + 体験 + ロジック）
GK: プロダクトをどう売る・伸ばすか（マーケ + マネタイズ + 集客 + 計測）
```

## Growth Knowledge との境界

| 問い | 置き場 |
|---|---|
| 足場・スタック・体験・ループ・ドメインルール | **この repo** |
| マーケ・マネタイズ・集客・計測 | [Growth Knowledge](https://github.com/mayu119/growth-knowledge) |
| 施策を試した結果・CVR・adopt 判定 | Growth Knowledge `apps/` + `measurement/` |
| 体験パターンを採用した・実装した | この repo `apps/`（原理）→ GK `apps/`（数字） |

**重複を避けるルール**

- 外部事例・競合・OSS の解析 → `references/`（原文・要約）
- 横断で使える原理 → `patterns/<theme>/guide.md`
- 未蒸留の候補 → `patterns/<theme>/candidates.md`
- アプリ固有の体験判断・ドメインルール → `apps/<app>.md`
- 実験結果・売上・インストール数 → Growth Knowledge へ。ここには書かない

## 2層構造

- **横断層**: `patterns/`。プラットフォーム非依存の原理、設計パターン、参照実装の分解、出典 URL。
- **プロダクト固有層**: `apps/`。横断知見をそのプロダクトに採用したか、どう実装したか（計測結果は GK へ）。

横断原理とプロダクトの実装メモを混ぜない。

## どっちの repo を見るか（30秒）

```
転換・課金・広告・実験の数字が知りたい     → Growth Knowledge
体験・ループ・演出・状態・ドメインルール    → Product Knowledge
両方                                      → Product で設計 → GK で検証
```

## テーマディレクトリ

| 場所 | 中身 |
|---|---|
| [`patterns/scaffold/`](patterns/scaffold/README.md) | スタック選定、ホスティング、DB、認証配線、lock-in vs ship |
| [`patterns/core-loop/`](patterns/core-loop/README.md) | セッション構造、報酬粒度、エスカレーション、失敗設計 |
| [`patterns/feedback/`](patterns/feedback/README.md) | 即時フィードバック、マイクロ報酬、音・動き |
| [`patterns/progression/`](patterns/progression/README.md) | 解放、マスター、コレクション、星・実績 |
| [`patterns/habit/`](patterns/habit/README.md) | ストリーク、デイリー、復帰、さびつき |
| [`patterns/information/`](patterns/information/README.md) | 画面構成、状態表示、エラー、オンボ以外の案内 |
| [`patterns/visual/`](patterns/visual/README.md) | 色・タイポ・コンポーネント・マスコット・モーション |
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
| [docs/NORTH-STAR.md](docs/NORTH-STAR.md) | **向かう先** — ミッション、7 レイヤ、PK/GK 境界、プラットフォーム方針 |
| [docs/INITIAL-DESIGN.md](docs/INITIAL-DESIGN.md) | repo 立ち上げの設計正本（GK との分離理由・ディレクトリ・運用） |
| [references/2026-09-27-dopa-drill.md](references/2026-09-27-dopa-drill.md) | 第 1 号参照実装の快感・コアループ分解 |
| [references/2026-09-27-dopa-drill-design.md](references/2026-09-27-dopa-drill-design.md) | 同上のビジュアル・UI・モーション分解 |

## 最初の参照実装

- [ドパドリル（dopa-drill）解析](references/2026-09-27-dopa-drill.md) — コアループ・快感（[デザイン分解](references/2026-09-27-dopa-drill-design.md) あり）
