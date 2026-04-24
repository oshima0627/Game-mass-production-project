# {{Game Title}}（実装リポジトリ）

Unity で開発中のモバイルゲーム。最新仕様は必ず `docs/design.md` を参照する。
設計ハブ（`game-design-hub`）の `design/{{game-slug}}.md` は着手時点のスナップショットであり、現在の仕様とは食い違う可能性がある。

## Git 運用方針

**機能ブランチ + Pull Request レビューで運用する。**
- `main` は保護ブランチ、直接 push 禁止
- `feature/<task>`, `fix/<task>` で作業し PR を立てる
- セルフレビューでも PR は作る（差分の記録・ロールバック容易性のため）
- ソフトローンチ以降は `release/*` タグ運用を追加

## 技術スタック

| 項目 | 技術 |
|------|------|
| ゲームエンジン | Unity {{unity_version}}（例: `6000.0.23f1`、正本は `ProjectSettings/ProjectVersion.txt`） |
| 言語 | C# (.NET Standard 2.1) |
| レンダーパイプライン | URP |
| UI | uGUI |
| 入力 | Input System |
| 広告 | Google Mobile Ads Unity Plugin (AdMob) |
| アプリ内課金 | Unity IAP |
| 解析 | Firebase Analytics + Crashlytics + Remote Config |
| アセット管理 | Addressables |
| コード分割 | Assembly Definitions (.asmdef) |
| バージョン管理 | Git + Git LFS |

## プロジェクト構成

`Assets/_Project/` 配下:
- `Scripts/Runtime/`: ランタイム C# スクリプト（機能単位で `.asmdef`）
- `Scripts/Editor/`: Editor 拡張・ビルドスクリプト（`*.Editor.asmdef`）
- `Scripts/Tests/`: EditMode / PlayMode テスト
- `Prefabs/`
- `Scenes/`
- `Art/`
- `Audio/`
- `Shaders/`
- `Settings/`
- `Addressables/`

## Analytics イベント命名規約

`docs/analytics-spec.md` を参照（設計ハブから初期コピー済み、以降はこのリポジトリ側が正本）。
追加イベントを実装する場合は snake_case、動詞_名詞順、時間 `_sec` サフィックス、金額 `_usd` サフィックスを厳守。

## シークレット管理

以下は**絶対にコミットしない**:
- `.keystore`（署名鍵）
- `google-services.json`（本番用）
- 本番 AdMob ID

CI の secrets または `.env` + ビルド時注入を採用。ローカルの sample は `*.example` として拡張子を分けてコミット可。

## CI/CD

- **ビルド**: GitHub Actions + `game-ci/unity-builder`（Android APK/AAB、iOS xcarchive）
- **配布**: Android は Firebase App Distribution / Play 内部テスト、iOS は TestFlight（Fastlane でアップロード自動化）
- **バージョン採番**: `semver` + CI で `PlayerSettings.bundleVersion` と `bundleVersionCode` を自動インクリメント
- **Unity ライセンス**: Personal ライセンスは Unity Activate の手順を CI secrets に登録

初期セットアップ時はスタブのワークフロー（`.github/workflows/build.yml`）だけ置いて、ソフトローンチ前に実装する。

## リリース前チェックリスト

- [ ] 本番 AdMob ID / Firebase 設定に差し替え済み
- [ ] Analytics 必須イベントがすべて飛んでいる（BigQuery で確認）
- [ ] `docs/design.md` と実装の整合性確認
- [ ] プライバシーポリシー / 利用規約リンクを設定画面に配置
- [ ] アプリ内で IAP 復元導線を実装（iOS 審査必須）
- [ ] 内部テスト → クローズドテスト → ソフトローンチ（フィリピン/カナダ等） → 指標検証 → グローバル展開

## プレイテスト

- 配布手段: Android は Google Play 内部テスト / Firebase App Distribution、iOS は TestFlight
- 週1回、最低3人の外部テスターで継続計測
- フィードバックは Analytics 数値と突合して定性/定量の両面で判断
