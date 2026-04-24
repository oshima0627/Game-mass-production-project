# Analytics 計測規約（実装リポジトリ共通）

指標駆動で設計判断するため、**全作品で以下のイベントを必ず仕込む**。Firebase Analytics 側で命名・パラメータを統一し、作品横断で比較可能な状態にする。

## 必須イベント

| イベント名 | タイミング | 必須パラメータ |
|------|------|------|
| `app_first_open` | 初回起動 | - |
| `onboarding_start` | チュートリアル開始 | - |
| `onboarding_complete` | チュートリアル完了 | `duration_sec: int` |
| `onboarding_drop` | チュートリアル離脱 | `step: string`（ステップ識別子） |
| `session_start` | 起動ごと | `is_first_session: bool` |
| `level_start` | ステージ/ラウンド開始 | `level_id: string`, `attempt_count: int` |
| `level_complete` | 成功 | `level_id: string`, `duration_sec: int`, `score: int` |
| `level_fail` | 失敗 | `level_id: string`, `duration_sec: int`, `fail_reason: string` |
| `ad_request` / `ad_impression` / `ad_reward` | 広告表示ライフサイクル | `ad_type: "reward"|"interstitial"|"banner"`, `placement: string` |
| `iap_view` | 購入画面表示 | `product_id: string`, `placement: string` |
| `iap_purchase` | 購入成立 | `product_id: string`, `price_usd: float` |
| `share_attempt` / `share_complete` | シェア導線 | `content_type: "replay"|"screenshot"|"result"` |

## 命名規則

- snake_case、動詞_名詞順（例: `level_start` は `start_level` にしない）
- パラメータ名も snake_case で統一
- 時間は秒単位 `_sec` サフィックス、金額は USD の `_usd` サフィックス
- `level_id` などの識別子は意味のある文字列（`stage_1_3` 等）、数値連番のみは避ける

## KPI ダッシュボード

Firebase の BigQuery エクスポートを有効化し、以下を定常監視:
- D1 / D7 / D30 retention
- 平均 playtime / セッションあたり playtime
- チュートリアル完走率（`onboarding_complete` / `onboarding_start`）
- 初回課金率（`iap_purchase` ユニーク / `app_first_open` ユニーク）
- 広告あたり収益（ad_impression × eCPM）
- レベル離脱ヒートマップ（`level_fail` の `fail_reason` 集計）

ソフトローンチ段階で異常値を検知できるようにダッシュボードを先に組む（指標が出てから作らない）。

## 追加イベントの申請

作品固有イベントを追加する場合は、本スペックのフォーマットに準拠した上で `docs/design.md` に追記。設計ハブにバックポートする場合は `design/knowledge/analytics-patterns.md` に抽出。
