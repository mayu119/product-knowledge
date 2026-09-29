# scaffold — プロダクト足場・スタック

スタック選定、ホスティング、DB、バインディング、認証配線、プラットフォームへの賭け、lock-in vs ship のトレードオフ。

## 読む順

1. この README
2. `guide.md` — 蒸留済み原理
3. `candidates.md` — 未蒸留候補（昇格前の仮置き）
4. `references/` — 外部事例・Platform 解析（例: [Cloudflare all-in](../../references/2026-09-29-cloudflare-all-in.md)）

## 典型の問い

- どのスタック・ホスティングに賭けるか（例: Cloudflare に一本化するか）
- DB・認証・API の配線をどう最小化して ship するか
- lock-in と開発速度のトレードオフは許容できるか
- iOS / Web などプラットフォームごとに足場はどう変わるか

## 使い方

### 新規プロダクト（baseline）

1. `guide.md` を **チェックリスト** として通読 — 各 `##` が 1 判断（配線最小化、Bindings、lock-in 許容など）
2. クライアントが Web か iOS/Android かで **足場の置き場** を決める（ネイティブは API backend を 1 Platform に寄せる）
3. Platform 固有の parts・サービス一覧は `references/` を参照（Cloudflare 例: [2026-09-29-cloudflare-all-in.md](../../references/2026-09-29-cloudflare-all-in.md)）
4. 採用した判断は出荷後 `apps/<app>.md` に記録（実装 PR リンク付き）

### 既存プロダクト（quality lift）

1. 現在の配線本数（DB / storage / queue / auth / cron / realtime）を棚卸し
2. `guide.md` で **削れる配線** と **lock-in 許容範囲** を照合
3. 改善 PR と Before/After を `apps/<app>.md` に残す

### 候補の追加・蒸留

- 新しい Platform 論・OSS 足場 → `references/YYYY-MM-DD-slug.md`
- 横断で使えそうな行 → `candidates.md` に出典付きで追加
- 精査後 `guide.md` に昇格し、candidates 行に `guide へ蒸留済` を付ける

原理は **プラットフォーム非依存** で書く。Cloudflare 等は primary example + 出典（[NORTH-STAR](../../docs/NORTH-STAR.md)）。
