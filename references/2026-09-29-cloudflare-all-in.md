# Cloudflare All-in — 足場を 1 プラットフォームに寄せる

- 出典: ユーザー提供サマリー（Indie dev / Cloudflare all-in 論）
- 性質: スタック選定・インフラ配線の **判断フレーム**（コード解析ではない）
- 解析日: 2026-09-29
- プラットフォーム: **Web / バックエンド中心**。iOS / Android ネイティブアプリは Cloudflare を **API バックエンド** として利用可能（クライアント足場は別途）

## 概要

Indie dev の痛みは **コードを書くこと** より、DB・Storage・Hosting・Queue・auth・networking・billing・サービス同士の **配線を選び続けること** にある。

Cloudflare all-in の論点は「ベストオブブリードを散らす」より **good-enough な部品を 1 プラットフォームに集約** し、非プロダクト思考時間を削って ship すること。Compute・Storage・DB・Queue・Cron・stateful realtime・Tunnel・Zero Trust・AI Gateway・Sandbox まで同一ベンダー上に揃う。

**Bindings** — Platform が Worker に Resource を渡す（`.env` に credential を置かない）— は CSIRT フレンドリーで、配線事故を減らす足場原理として PK scaffold に取り込む。

---

## 一言で言うと

「各カテゴリの最強 SaaS を 5 つつなぐ」より **1 Platform に賭けて配線を減らす**。lock-in はあるが、**インフラにうんざりして作るのをやめる** 方が Indie には致命的 — 早期は lock-in を許容する。

---

## Indie dev の pain（なぜ足場が問題か）

| 領域 | 選定・配線で消える時間 |
|---|---|
| DB | Postgres vs SQLite vs serverless SQL |
| Storage | S3 互換、egress 料金、CDN 連携 |
| Hosting | Server / PaaS / Serverless、デプロイパイプライン |
| Queue / Cron | 別サービス、認証、監視 |
| Auth | OAuth、JWT、セッション、各 SaaS 連携 |
| Networking | TLS、WAF、Tunnel、社内アクセス |
| Billing | 複数請求書、予算の見通し |

**Product Knowledge の含意**: 足場レイヤ（`scaffold/`）は「どの部品が最強か」より **配線本数と思考コスト** を最小化する判断基準を持つ。

---

## プラットフォームマップ（Cloudflare）

| カテゴリ | サービス | 役割 |
|---|---|---|
| Compute | **Workers** | エッジ/serverless 実行。API・BFF・軽量バックエンド |
| Object Storage | **R2** | S3 互換。**egress 無料** — 小規模でも転送コストを気にしにくい |
| DB | **D1** | SQLite 互換 serverless SQL |
| KV | **KV** | 低レイテンシ key-value、設定・キャッシュ |
| Queue | **Queues** | 非同期ジョブ、Worker 連携 |
| Cron | **Cron Triggers** | 定期実行（別 cron サービス不要） |
| Stateful / Realtime | **Durable Objects** | WebSocket、チャット、協調編集、マルチプレイ、stateful agent |
| Networking | **Tunnel** | ローカル / プライベートを公開せず接続 |
| Access | **Zero Trust Access** | 認可・社内/限定公開 |
| AI | **AI Gateway** | LLM 呼び出しのプロキシ・レート・観測 |
| Sandbox | **Sandbox** | 隔離実行（agent / コード実行系） |

**進化の文脈**: CDN → Serverless → Full-stack → **Agent Runtime** へ。足場の「賭け先」が compute だけでなく agent 実行基盤まで広がっている。

---

## Bindings 原理

| 従来 | Bindings |
|---|---|
| `.env` / Secret Manager に API key・接続文字列 | Platform が Worker 実行時に **Resource 参照** を注入 |
| 漏洩リスク（git commit、ログ） | credential をアプリコードから分離 |
| ローカルと本番で env 差分 | wrangler.toml / dashboard で宣言的に近い |

**横断原理（プラットフォーム非依存）**: **Binding > .env** — 実行環境が Resource を渡すモデルを優先し、開発者が credential を運ばない。

Cloudflare 以外の例: AWS Lambda + IAM role、GCP service account binding、Fly.io secrets as mounts — 思想は同じ。

---

## Durable Objects — stateful / realtime の足場

別途 Redis + WebSocket サーバー + sticky session を組まずに、**1 Worker エコシステム内** で stateful を載せられる。

| ユースケース | なぜ DO |
|---|---|
| チャット | ルーム単位の状態 + WebSocket |
| 協調編集 | ドキュメントごとの単一 writer / CRDT ホスト |
| マルチプレイ | セッション state、tick 同期 |
| Stateful agent | 会話・ツール状態をオブジェクトに閉じる |

**含意**: realtime / collaborative は「別バックエンドスタック」を増やす前に、**既に賭けている Platform の stateful primitive** があるかを見る。

---

## Agent Runtime 方向（ emerging stack ）

同一 Platform 上に agent 向け部品が揃い始めている:

| 部品 | 役割 |
|---|---|
| Browser | 外部サイト操作・スクレイピング |
| Sandbox | 隔離コード実行 |
| AI Gateway | LLM ルーティング・観測 |
| Agent Memory | 長期コンテキスト（プラットフォーム側） |
| Email | 通知・ワークフロー |
| Wallet | 決済・クレジット（将来/限定） |

**含意**: agent プロダクトも **compute + storage + queue + sandbox + AI** を別ベンダーに散らさず、足場判断を core-loop 以前に固定できる。

---

## Lock-in vs ship のトレードオフ

| 懸念 | 反論（all-in 論） |
|---|---|
| Vendor lock-in | 早期 Indie は **作るのをやめる** 方が先に来る |
| 部品が best-in-class ではない | **good-enough on ONE platform** > scattered best-in-class |
| 将来の移行コスト | 移行は **プロダクトが生き残ってから** 検討すればよい |

**横断原理**: lock-in は悪ではない。**配線と非プロダクト思考がプロダクトを殺す** 場合、Phase 0–1 では 1 Platform 賭けを許容する。Phase 2+ で revenue / scale に応じて部分脱出を検討。

---

## コスト・Tier

- Free tier が厚く、**小さなサービスは低コスト** で試せる
- egress-free storage（R2）は **転送料の予測可能性** を足場判断に効く
- 請求が 1 ベンダーに集約 → Indie の mental model が単純

（具体的な料金・CVR は Growth Knowledge 管轄 — 本参照では **ship しやすさ** のみ）

---

## プラットフォーム注記

| クライアント | 足場の位置づけ |
|---|---|
| Web / SPA / SSR | Cloudflare を **フルスタック足場** として all-in 可能 |
| iOS / Android | ネイティブ UI は各 OS。**API・auth・storage・realtime backend** を Cloudflare Workers + D1/R2/DO に置く |
| ハイブリッド | BFF を Workers、モバイルは API 消費 — **配線はバックエンド側で 1 Platform に寄せる** |

PK の `guide.md` は原理を **プラットフォーム非依存** で書き、Cloudflare は **primary example + 出典** とする（[NORTH-STAR](../docs/NORTH-STAR.md) 方針）。

---

## Growth Knowledge へ（PK 候補にしない）

| テーマ | 理由 |
|---|---|
| 具体的な Cloudflare 料金比較 | コスト最適化・予算 → GK |
| 獲得・課金・ARPU | 足場ではなく成長 |

---

## 蒸留マップ

| 原理 / 要素 | 移行先 |
|---|---|
| 1 Platform に寄せて配線を減らす | `patterns/scaffold/candidates.md` → `guide.md` |
| Binding > .env | 同上 |
| Lock-in vs 作るのをやめる | 同上 |
| R2 egress-free storage（転送コスト予測） | 同上 |
| Durable Objects for realtime/stateful | 同上 |
| Agent runtime を同一 Platform に | 同上 |
| 非プロダクト思考時間を削る | 同上 |
| Free tier / 低コストで小さく始める | 同上 |

---

## 自社への転用メモ（未検証）

| シナリオ | 借りやすい判断 | 注意 |
|---|---|---|
| 新規 Web SaaS | Workers + D1 + R2 + Queues で MVP | D1 の制約・スケール上限を事前確認 |
| Realtime 機能 | Durable Objects 先行 | 複雑な CRDT は設計コストは残る |
| iOS アプリ | API を Workers に集約 | クライアント auth（Sign in with Apple 等）は別配線 |
| AI agent 試作 | AI Gateway + Sandbox + DO | 観測・コスト cap は GK と連携 |
