# {{Game Title}}

{{game_tagline}}

## 概要

- **ジャンル**: {{genre}}
- **対象プラットフォーム**: Android（Google Play） → iOS（App Store）
- **Unity バージョン**: {{unity_version}}（正本は `ProjectSettings/ProjectVersion.txt`）

最新の仕様は [`docs/design.md`](docs/design.md) を参照（設計書の正本はこのリポジトリ内）。

## セットアップ

1. Unity Hub で対応バージョンの Unity をインストール
2. このリポジトリを `git clone` し、Git LFS を初期化（`git lfs install && git lfs pull`）
3. Unity Hub から「Open」で本ディレクトリを開く
4. Firebase / AdMob の設定ファイル（開発用）を配置
   - `google-services.json`（開発用、本番用は CI secrets で差し替え）
   - AdMob テスト ID はコード中に `#if UNITY_EDITOR || DEBUG` で切り分け

## プロジェクト構成

`Assets/_Project/` 配下に機能単位で配置。詳細は [`CLAUDE.md`](CLAUDE.md) 参照。

## リリース前チェックリスト

[`CLAUDE.md`](CLAUDE.md) の「リリース前チェックリスト」を参照。

## 参考

- 企画段階のスナップショット: 設計ハブ `game-design-hub` の `design/{{game-slug}}.md`
- Analytics 計測規約: [`docs/analytics-spec.md`](docs/analytics-spec.md)
