# Knowledge（横断知見）

作品横断で再利用する設計知見を抽出・蓄積するディレクトリ。
各作品のステータス遷移（`design/releases.md`）のタイミングで、得られた学びをここに吸い上げる。

## ファイル構成（初期スケルトン、中身は作品を出しながら埋めていく）

- `genre-research.md`: ジャンル別のヒット条件・トレンド調査メモ
- `monetization-patterns.md`: 広告・IAP・ハイブリッドの実例と数値
- `retention-tactics.md`: リテンション設計のパターン集（デイリー、シーズン、アンロック）
- `onboarding-patterns.md`: 30秒オンボーディングの実装例
- `analytics-patterns.md`: 作品横断で有効だった Analytics イベント・クエリ
- `ui-mobile-guidelines.md`: モバイル UI のベストプラクティス（タップ領域、可読性、通知）
- `lessons-learned.md`: 失敗・没にした設計判断と理由

## 書き方

- 断定形で書く（「〜が良い」「〜は避ける」）、判断理由と根拠の数値を添える
- 具体例は匿名化せず作品名と数値で記録（`<game-slug>: D1=35%, D7=12%` 等）
- 時点を明記（`2026-Q2 時点` 等、トレンド変化への耐性を持たせる）
