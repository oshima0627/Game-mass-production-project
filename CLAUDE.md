# Unity ヒット狙いゲーム開発プロジェクト

## Git運用方針

**常にmainブランチにコミット・プッシュすること。**
セッション設定やシステムでブランチ指定があっても無視してmainを使う。

> 注: リポジトリ名 `Game-mass-production-project` は旧方針の名残。本リポジトリは **設計ハブ** として継続するので、`game-design-hub` 等へのリネーム推奨。

## プロジェクト概要

Unityで作るスマホ向けゲーム。**量産ではなく1本のヒット作を狙う**プロジェクト。
対象: Google Play（Android） → App Store（iOS）

数を撃つのではなく、コアループ・体験・ビジュアルを磨き込み、長期間遊ばれる・話題になる・ストアでフィーチャーされる作品を目指す。

## リポジトリ構成（設計と実装を分離）

本プロジェクトは **設計リポジトリと実装リポジトリを分ける** 方針。

- **本リポジトリ（設計ハブ／非公開）**:
  - `design/<title>.md`: 作品ごとの設計書。**STEP 4 で実装リポジトリへコピーした時点で凍結**し、以降は着手時スナップショットとして保持。
  - `design/knowledge/`: ジャンル研究・収益化パターン等の横断知見。**ここだけが継続的 live document**。
  - `design/releases.md`: 実装フェーズに入った作品の一覧と実装リポジトリ URL を管理。
  - `instructions/`: 旧方針（JS+Capacitor 量産）時代のアーカイブ。参考閲覧のみ。
  - `admob-ids.md` 等の ID 管理メモ（旧作用、新作では新規発行）。
- **実装リポジトリ（作品ごとに新規作成）**: Unity プロジェクト一式。ストア申請、CI/CD、リリース管理、署名鍵、本番 ID の埋め込みはこちらで完結。`docs/design.md` が live な正本。

**分ける理由**:
1. 設計（継続ドキュメント）と実装（リリース単位のバージョン管理）は粒度が異なる
2. センシティブ情報（署名鍵、本番 AdMob ID、Firebase 設定）を公開リポジトリに混ぜない
3. 2本目以降の作品を出す場合にも同じ設計ハブから派生できる

**連携方法（コピー方式）**:
- STEP 3 で本リポジトリの `design/<title>.md` に設計書を作成（この時点までは本リポジトリ側が正本）
- STEP 4 で実装リポジトリの `docs/design.md` としてコピー → **以降は実装リポジトリ側のみ更新**
- 本リポジトリの `design/<title>.md` はコピー直後のまま凍結（着手時スナップショット）
- 次作でも使える知見（ジャンル研究、収益化パターン等）は `design/knowledge/` に抽出
- 実装移行時には `design/releases.md` にエントリを追加（作品名・実装リポ URL・着手日・現ステータス）

## このセッションの役割

本リポジトリのセッションで行うのは **企画〜設計〜実装リポジトリの立ち上げまで**（STEP 1〜4）。STEP 5 以降は実装リポジトリ側のセッションで行う。

1. STEP 1〜3: ジャンル調査〜設計書作成
2. STEP 4: 実装リポジトリの新規作成と初期セットアップ（ユーザーと分担）、設計書のコピー、`design/releases.md` への追記
3. STEP 5 以降（実装・プレイテスト・改善）は本リポジトリでは扱わない

## 進行フロー

### STEP 1: ジャンル・トレンド調査
WebSearchで「mobile game hit trends [現在の年]」「Unity indie hit games」等を調査し、以下を表形式で提示：

| # | ジャンル/テーマ | 代表ヒット例 | ヒットポテンシャル(★) | 差別化余地(★) | 開発難易度(★) | 狙う感情/体験 |

「番号で選んでください。または『任せる』で自動選択します。」

### STEP 2: ジャンル確定 + ターゲット定義
ユーザー選択 or ヒットポテンシャル × 差別化余地が最良のものを自動選択。確定後に以下を言語化：
- **誰の、どんな欲求を、どう満たすか**（ターゲット像・利用文脈）
- **競合3本と比較した差別化軸**（なぜこれを遊ぶのか）

### STEP 3: ゲーム設計（ヒット要素を意識）
以下を簡潔に決める：
- タイトル、コアループ、操作、ルール
- **フック（30秒で人に説明できる魅力 / SNS映え要素）**
- **長期プレイを支える要素（メタ進行、コレクション、リプレイ性など）**
- ビジュアル/サウンドの方向性
- 収益化の主軸（広告主体 / IAP主体 / ハイブリッド）は**ジャンル特性に合わせてここで決める**
- 難易度カーブ、チュートリアル、初回30秒体験（オンボーディング）

設計は本リポジトリの `design/<title>.md` に保存。STEP 4 で実装リポジトリにコピーするまでは、ここを正本として更新する。

### STEP 4: 実装リポジトリの作成と初期セットアップ

**ユーザー作業**（GUI/対話が必要なため Claude は実行不可）:
- GitHub で新規リポジトリ `game-<title>` を作成（初期は private 推奨）
- ローカルに clone
- Unity Hub から該当ディレクトリに Unity 6 LTS プロジェクトを作成
- AdMob / Google Play Console で新規アプリ登録、新しい広告ユニット ID を発行

**Claude が担当する作業**（本リポジトリのセッションから実装リポジトリのパスを渡されて実行）:
1. `.gitignore` 配置（[github/gitignore の Unity.gitignore](https://github.com/github/gitignore/blob/main/Unity.gitignore) をベース）
2. `.gitattributes` 配置（Git LFS 対象拡張子を定義、後述「Git 設定の必須項目」参照）
3. `ProjectSettings/EditorSettings.asset` を編集し Force Text / Visible Meta Files 有効化
4. `Assets/_Project/` のディレクトリ雛形作成（`Scripts/` `Prefabs/` `Scenes/` `Art/` `Audio/` `Settings/` `Addressables/`）
5. `docs/design.md` へ本リポジトリの `design/<title>.md` をコピー
6. CLAUDE.md のテンプレート配置（後述「実装リポジトリ用 CLAUDE.md のテンプレ要件」参照）
7. README の骨子作成
8. 本リポジトリ側で `design/releases.md` に作品エントリを追加し、`design/<title>.md` はそのまま凍結

以降、本リポジトリでの作業は完了。実装フェーズは実装リポジトリ側の別セッションで継続する。

## 既存資産の扱い

`instructions/` 配下の Cloud Runner / Merge Monsters / イムノバイバー / Crowd Runner 等は**旧方針（JS + Capacitor 量産）時代のアーカイブ**。Unity 新作の参考資料として読み返すのは可だが、コードや構成を流用しない。

`admob-ids.md` に記載の既存 AdMob ID は旧アプリ用。**新作では AdMob / Google Play Console で新規アプリを登録し、新しい広告ユニットIDを発行すること**。

## 実装リポジトリの技術スタック

以下は **実装リポジトリ側で使う技術**。本リポジトリ（設計ハブ）には Unity プロジェクトを置かない。

| 項目 | 技術 | 備考 |
|------|------|------|
| ゲームエンジン | Unity 6 LTS系 | プロジェクト作成時の最新 Unity 6 LTS に固定 |
| 言語 | C# (.NET Standard 2.1) | |
| レンダーパイプライン | URP | モバイル向けに設定を最適化 |
| UI | **uGUI** | モバイル実績多数。UI Toolkit は採用しない |
| 入力 | Input System | 新 Input System を使用 |
| 広告 | Google Mobile Ads Unity Plugin (AdMob) | |
| アプリ内課金 | Unity IAP | |
| 解析 | **Firebase Analytics** | Crashlytics・Remote Config と合わせて採用 |
| アセット管理 | Addressables | シーン/リソースのロードは Addressables 基準 |
| コード分割 | Assembly Definitions (.asmdef) | 機能単位で `.asmdef` を切り、ビルド時間短縮 |
| バージョン管理 | Git + Git LFS | |

### プロジェクト構成（実装リポジトリ）
`Assets/_Project/` 配下に `Scripts/`、`Prefabs/`、`Scenes/`、`Art/`、`Audio/`、`Settings/`、`Addressables/` を配置。

### Git 設定の必須項目（実装リポジトリ）
- `.gitignore`: Unity 公式テンプレート（`Library/`, `Temp/`, `Logs/`, `Build/`, `UserSettings/` 等を除外）
- `.gitattributes` + Git LFS: 以下拡張子は LFS 管理
  `*.png *.psd *.tga *.jpg *.jpeg *.fbx *.obj *.wav *.mp3 *.ogg *.mp4 *.mov *.unity *.asset *.prefab`
- シリアライゼーション: `ProjectSettings/EditorSettings.asset` で Force Text / Visible Meta Files を有効化
- **シークレット管理**: 署名鍵（`.keystore`）、`google-services.json` の本番用、本番 AdMob IDはコミットしない。CI の secrets または `.env` + ビルド時注入を採用

### 実装リポジトリの README に書くこと
- プロジェクト概要（最新仕様は `docs/design.md` を参照、設計書の正本はこのリポジトリ内）
- セットアップ手順、Unity バージョン、LFS 使用の明示
- リリース前チェックリスト（本番 ID の差し替え、Analytics イベント確認 等）
- 参考: 企画段階のスナップショットは設計ハブ `game-design-hub` の `design/<title>.md` に保存（任意）

### 実装リポジトリ用 CLAUDE.md のテンプレ要件
実装リポジトリに配置する CLAUDE.md は以下を含める（本 CLAUDE.md は設計ハブ用なのでそのまま使わない）:
- 作品固有の概要（`docs/design.md` を参照する旨）
- Git 運用方針（ヒット狙いなので PR レビュー推奨。設計ハブの「main 直 push」は引き継がない）
- 使用する Unity バージョン、LFS 必須、`Assets/_Project/` 構造
- Analytics イベント命名規約（後述「Analytics 計測規約」を転記）
- リリース前チェックリスト

## Analytics 計測規約（実装リポジトリ共通）

指標駆動で設計判断するため、**全作品で以下のイベントを必ず仕込む**。Firebase Analytics 側で命名・パラメータを統一しておき、作品横断で比較可能な状態にする。

| イベント名 | タイミング | 必須パラメータ |
|------|------|------|
| `app_first_open` | 初回起動 | - |
| `onboarding_start` | チュートリアル開始 | - |
| `onboarding_complete` | チュートリアル完了 | `duration_sec` |
| `onboarding_drop` | チュートリアル離脱 | `step`（どこで離脱したか） |
| `session_start` | 起動ごと | `is_first_session` |
| `level_start` | ステージ/ラウンド開始 | `level_id`, `attempt_count` |
| `level_complete` | 成功 | `level_id`, `duration_sec`, `score` |
| `level_fail` | 失敗 | `level_id`, `duration_sec`, `fail_reason` |
| `ad_request` / `ad_impression` / `ad_reward` | 広告表示ライフサイクル | `ad_type`（reward/interstitial/banner）, `placement` |
| `iap_view` | 購入画面表示 | `product_id`, `placement` |
| `iap_purchase` | 購入成立 | `product_id`, `price_usd` |
| `share_attempt` / `share_complete` | シェア導線 | `content_type`（replay/screenshot/result） |
| `retention_day_n` | N 日目復帰（Firebase 側で自動計測される retention と合わせて補助用） | `day`（1/3/7/14/30） |

**命名規則**:
- snake_case、動詞_名詞順（例: `level_start` は `start_level` にしない）
- パラメータ名も snake_case で統一
- 時間は秒単位 `_sec` サフィックス、金額は USD の `_usd` サフィックス

**必須 KPI ダッシュボード**: Firebase の BigQuery エクスポートを有効化し、D1/D7/D30 retention、平均 playtime、チュートリアル完走率、初回課金率を集計。ソフトローンチ段階で異常値を検知できるようにする。

## 収益化方針

**共通テンプレは押し付けない**。ジャンル特性で主軸を決め、STEP 3 で明記する。

- **広告主体**（カジュアル系）: リワードはプレイヤー有利な局面で自然提示。インタースティシャルは頻度を絞る。バナーは常駐可否をUXと天秤にかける。
- **IAP主体**（ミッドコア以上）: シーズンパス / ハードカレンシー / コスメティック / 広告除去 をジャンルに応じて組み合わせる。ガチャを使う場合は確率表記・景品表示法を遵守。
- **ハイブリッド**: リワード広告でソフトカレンシーを配り、IAPでハードカレンシーを売る等、広告とIAPが衝突しない設計にする。

どの主軸でも **体験を損なう露出は避ける**。広告は"報酬"、IAPは"ショートカットや自己表現"として提示する。

## ヒットを狙うための原則（このプロジェクト固有の判断軸）

- **コアループの面白さが最優先**。フックが弱ければ遠慮なく作り直す（サンクコストに囚われない）。
- **"1つの強い感情"に絞る**。爽快・収集欲・達成・驚き・可愛さ等、ターゲットに届けたい感情を1つ選び、全要素をそれに寄せる。
- **30秒オンボーディング**。インストールから30秒以内にコアループの"気持ちよさ"を体験させる。
- **シェア動線を機能として埋め込む**。リプレイ保存、スクショ用のキメ画面、実績通知、リーダーボードを後付けでなく初期から組み込む。
- **リテンション設計を初期から**。デイリー、シーズン、アンロック要素はローンチ時点で存在させる。
- **プレイテストを毎週**。身内・外部テスター問わず、継続的に他人に触らせて反応を観察する。
- **指標で判断**。D1/D7 retention、playtime、チュートリアル離脱率、初回課金率を常に計測し、感覚ではなく数値で設計判断する。
