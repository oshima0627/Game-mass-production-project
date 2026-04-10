# [ゲームタイトル] 開発指示

## 最重要方針
- HTML/JS/CSS + Capacitor のプロジェクト構造で作る
- コードは最後まで書き切る。省略・TODO・「以下同様」は禁止
- スマホ縦持ちで快適に遊べることを最優先する
- 見た目・操作感・エフェクトすべてにこだわり、ストアに出せる品質にする
- 広告は @capacitor-community/admob プラグインで実装する

## 概要
[ゲーム概要を3〜5文で。コアループ・世界観・何が楽しいかを明記]

## 技術スタック
- 言語: JavaScript（ES2020+）
- 描画: [Three.js 0.165.0 / Phaser.js 3.80.0 / 素のCanvas 2D]
- モバイル: Capacitor 6
- 広告: @capacitor-community/admob
- 対象: Android（将来iOS）

### プロジェクト構造
```
[game-title-kebab-case]/
├── package.json
├── capacitor.config.json
├── .gitignore
├── www/
│   ├── index.html
│   ├── css/style.css
│   └── js/
│       ├── main.js        # エントリーポイント・ゲームループ
│       ├── state.js       # 状態管理（STATEオブジェクト）
│       ├── renderer.js    # 描画処理
│       ├── input.js       # タッチ・キーボード入力
│       ├── sound.js       # Web Audio API サウンド
│       ├── particles.js   # パーティクルエフェクト
│       ├── ui.js          # UI画面
│       └── ads.js         # AdMob広告管理
├── android/               # npx cap add android で生成
└── ios/                   # 将来用
```

### package.json
```json
{
  "name": "[game-title-kebab-case]",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "cap:add": "npx cap add android",
    "cap:sync": "npx cap sync",
    "cap:open": "npx cap open android",
    "cap:run": "npx cap run android"
  },
  "dependencies": {
    "@capacitor/core": "^6.0.0",
    "@capacitor-community/admob": "^6.0.0"
  },
  "devDependencies": {
    "@capacitor/cli": "^6.0.0"
  }
}
```

### capacitor.config.json
```json
{
  "appId": "[com.ドメイン.appid]",
  "appName": "[ゲームタイトル]",
  "webDir": "www",
  "server": { "androidScheme": "https" }
}
```

### .gitignore
```
node_modules/
android/
ios/
.DS_Store
*.log
```

## コードアーキテクチャ

### 必須ルール
- index.html: viewport meta（user-scalable=no）、apple-mobile-web-app-capable設定必須
- CSS先頭: `* { margin:0; padding:0; box-sizing:border-box; } body { overflow:hidden; background:#000; touch-action:none; }`
- state.js: STATEオブジェクトで状態管理（'title'|'playing'|'gameover'|'result'）、setState()で画面切替
- main.js: requestAnimationFrame + deltaTime（最大50ms cap）でゲームループ
- タッチ: touchstart/touchendで座標記録、passive:false、e.preventDefault()必須
- Three.js使用時: main.jsのみ`<script type="module">`、他は通常`<script src>`

## 画面構成

### タイトル画面（state: 'title'）
[タイトル、ハイスコア、STARTボタン、操作説明、背景演出の詳細]

### ゲームプレイ画面（state: 'playing'）
[キャンバス全画面、HUD（スコア等）、バナー広告エリア下部60px、ゲーム詳細ビジュアル]

### ゲームオーバー画面（state: 'gameover'）
[暗転フェード、GAME OVER、スコア/ハイスコア表示、NEW RECORD演出、リワード広告ボタン「広告を見て続ける」、リスタートボタン（インタースティシャル→再開）、バナー広告]

## ゲームルール・仕様

### 操作
[操作方法の詳細、キーボード対応（PC確認用）]

### ゲーム目的・勝敗条件
[目的、ゲームオーバー条件、クリア条件]

### スコア設計
[基本スコア計算式、ボーナス、コンボ倍率、ハイスコアlocalStorage保存]

### 難易度カーブ（数値で明記）
| 経過時間 | スピード | 障害物間隔 | その他変化 |
|---------|---------|----------|----------|
| 0〜30秒  | [値]    | [値]      | [詳細]    |
| 30〜60秒 | [値]    | [値]      | [詳細]    |
| 60秒〜   | [値]    | [値]      | [詳細]    |

### 特殊要素
[アイテム・パワーアップ・ボーナスなど]

## ゲームフィール（必ず実装）

以下すべて必須：
- **スクリーンシェイク**: ダメージ・衝突時（canvas/cameraオフセット）
- **フラッシュ**: スコア加算・アイテム取得時に白フラッシュ
- **フェード**: 画面遷移時0.3〜0.5秒フェードイン/アウト
- **パーティクル**: [イベントごとの色・数・動き]。シンプルなArray管理
- **サウンド**: Web Audio APIで生成（外部ファイル不要）。モバイルは最初のタップでAudioContext初期化必須。[ゲームに合った効果音リスト]
- **アニメーション**: ボタンタップ時スケール変化、スコア跳ねアニメーション、[キャラ/オブジェクトアニメ]

## 広告配置（必ず実装）

ads.jsに集約。@capacitor-community/admob使用。ブラウザ用モック実装も必須。

### AdMob ID
```js
const ADMOB_IDS = {
  app:           '[アプリID]',
  banner:        '[バナーID]',
  interstitial:  '[インタースティシャルID]',
  reward:        '[リワードID]',
};
```

### 広告フロー
```
ゲームオーバー → リワード広告「続ける？」→ (NO) → インタースティシャル → リザルト → ホーム（バナー常時）
```

### 実装要件
- ネイティブ/ブラウザ自動切替（window.Capacitor.isNativePlatform()判定）
- バナー: ADAPTIVE_BANNER, BOTTOM_CENTER。キャンバス高さ=innerHeight-60px
- body: `padding-bottom: calc(60px + env(safe-area-inset-bottom))`
- USE_TEST_ADS フラグでテストID/本番ID切替
- 公開API: ADS.init() / ADS.showBanner() / ADS.hideBanner() / ADS.showInterstitial(cb) / ADS.showReward(cb)

## ビジュアル仕様
- カラーパレット: [背景/メイン/アクセント/テキスト/警告のHEXコード]
- フォント: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif
- ボタン: border-radius:50px、グラデーション、box-shadow
- パネル: backdrop-filter:blur(10px)、半透明背景
- 画面遷移: opacityフェード
- [ゲームオブジェクトの色・形・サイズ詳細]

## パフォーマンス・モバイル最適化

- 60fps目標。オブジェクトプーリング使用。画面外オブジェクト即回収
- テクスチャ不使用、プリミティブ描画のみ。deltaTime必須
- 縦画面固定: AndroidManifest.xmlに`screenOrientation="portrait"`
- devicePixelRatio対応でシャープ描画
- touch-action:none(body)、manipulation(ボタン)

## 実装順序

1. プロジェクト初期化 → npm install
2. HTML/CSS/JS骨格
3. state.js + main.js（ゲームループ）
4. renderer.js + ui.js
5. input.js
6. コアゲームメカニクス
7. ゲームオーバー・画面遷移
8. スコア・ハイスコア
9. 難易度上昇
10. パーティクル・サウンド
11. ads.js（広告統合）
12. UI仕上げ
13. Capacitorビルド（cap add android → cap sync）
14. バランス調整・デバッグ

## 完成チェックリスト

- [ ] package.json/capacitor.config.json正常、npm install/cap sync成功
- [ ] 全画面遷移が動作（タイトル→プレイ→ゲームオーバー→リスタート）
- [ ] スコア・ハイスコアがlocalStorageで保存/読み込み
- [ ] 難易度が時間経過で上昇
- [ ] ads.jsにAdMob実装+ブラウザモック
- [ ] バナー/リワード/インタースティシャル全広告動作
- [ ] バナーエリア60px確保、キャンバスと非重複
- [ ] スマホタッチ操作正常、ボタン44×44px以上
- [ ] 縦持ちで全UI正常表示
- [ ] 60fps動作、スクリーンシェイク/パーティクル/サウンド/フェード実装
- [ ] NEW RECORD演出、コンソールエラー0件
