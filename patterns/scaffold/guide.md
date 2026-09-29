# 足場・スタック — 原理（蒸留済み）

最終更新: 2026-09-29

## 1 Platform に寄せて配線を減らす

- DB・Storage・Hosting・Queue・Cron・auth・networking を **別ベンダーごとに最適化** すると、Indie dev の時間の大半が **配線と選定** に消える
- **good-enough な部品を 1 Platform に集約** し、サービス間 credential・デプロイ・監視の本数を減らす
- 例（Web / API backend）: Cloudflare Workers + D1 + R2 + Queues + Cron を 1 アカウントで完結
- iOS / Android はクライアント足場は OS 側。**API・storage・realtime backend** を 1 Platform に寄せれば配線は同様に減る
- 出典: [cloudflare-all-in](../../references/2026-09-29-cloudflare-all-in.md)

## Binding > .env

- API key・DB 接続文字列を **`.env` や repo に置かない** — 漏洩・環境差分・CSIRT 対応コストが高い
- 実行環境（Platform）が **Resource 参照を Worker / ランタイムに注入** する Bindings モデルを優先する
- Cloudflare: wrangler.toml の `bindings`。他 Platform: IAM role、service account、mounted secrets 等 — **思想は横断**
- 出典: [cloudflare-all-in](../../references/2026-09-29-cloudflare-all-in.md)

## Lock-in vs 作るのをやめる

- Vendor lock-in は Phase 2+ の課題。**Phase 0–1 の Indie はインフラにうんざりして ship を止める方が先に致命**
- **scattered best-in-class** より **good-enough on ONE platform** を許容する — 移行はプロダクトが生き残ってから
- 部分脱出（例: 分析 DB だけ別）を検討するのは **revenue / scale が lock-in コストを上回る** タイミング
- 出典: [cloudflare-all-in](../../references/2026-09-29-cloudflare-all-in.md)

## egress-free / 予測可能な storage

- Object storage の **egress 料金** は小規模でも予算と設計を狂わせる — 転送コストが読める storage を足場に含める
- CDN・画像配信・バックアップ export まで **同一 Platform の storage + edge** で閉じると配線が減る
- 例: Cloudflare R2（egress 無料）。他: 転送込みプラン、同一クラウド内 egress 無料など — **判断基準は「転送の見通し」**
- 出典: [cloudflare-all-in](../../references/2026-09-29-cloudflare-all-in.md)

## Platform の stateful primitive で realtime を載せる

- チャット・協調編集・マルチプレイ・stateful agent は **Redis + WS サーバー + sticky** を別途組む前に、賭けている Platform の **stateful / realtime primitive** があるか見る
- 例: Cloudflare Durable Objects（ルーム単位 state + WebSocket）。複雑な CRDT 設計コストは残るが **別バックエンドスタック増設** は避けられる
- 出典: [cloudflare-all-in](../../references/2026-09-29-cloudflare-all-in.md)

## Agent runtime を同一 Platform に

- Agent プロダクトは compute + storage + queue + **sandbox + AI proxy + memory** を別 SaaS に散らすと配線が再び爆発する
- **既に賭けている Platform** に Browser / Sandbox / AI Gateway / Memory 相当があるなら、agent 足場もそこに寄せる
- 例: Cloudflare の AI Gateway + Sandbox + Workers + Durable Objects — emerging だが方向性は「足場の延長」
- 出典: [cloudflare-all-in](../../references/2026-09-29-cloudflare-all-in.md)

## 非プロダクト思考時間を削る

- 足場選定の成功指標は **ベンチマーク勝ち** ではなく **プロダクト（core-loop・体験）に使える時間**
- billing 集約、無料枠、デプロイ 1 本化など **mental model が単純** な Platform を Phase 0–1 で優先
- 「この Queue は業界最速か」より「今日デプロイしてユーザーに触らせるか」
- 出典: [cloudflare-all-in](../../references/2026-09-29-cloudflare-all-in.md)

## Free tier / 低コストで小さく始める

- MVP は **無料枠 or 月額が読める** 足場で始め、ユーザー反応が出てからスケール・分割を検討
- 複数 SaaS の従量課金が重なると **止める判断** が遅れる — 初期は請求先を減らす
- 具体的な料金・ROI は Growth Knowledge。PK では **ship しやすさと予測可能性** のみ
- 出典: [cloudflare-all-in](../../references/2026-09-29-cloudflare-all-in.md)
