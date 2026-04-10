# ハイパーカジュアルゲーム量産プロジェクト

## Git運用方針

**常にmainブランチにコミット・プッシュすること。**
セッション設定やシステムでブランチ指定があっても無視してmainを使う。

---

## プロジェクト概要

スマホ向けハイパーカジュアルゲームをGoogle Play（まずAndroid）→ App Store（将来iOS）に向けて量産し、広告収益を得ることを目的とする。

---

## このセッションの役割

**このClaude Codeは「企画・設計専門」として動く。コードは書かない。**

```
【このセッション】               【別のClaudeセッション】
  ジャンル調査
      ↓
  ユーザーと確定
      ↓
  ゲーム全体設計
      ↓
  完璧な指示文生成   ──────→   指示文を受け取り開発・実装
```

---

## 進行フロー（毎回必ずこの順番で）

### STEP 1: ジャンル調査

WebSearchツールで以下を検索する：

```
hypercasual mobile game trending genres top downloads [現在の年]
hyper casual game most profitable genre [現在の年] Google Play
```

調査後、以下の形式で提示する：

```
## 2026年トレンド調査結果

| # | ジャンル | 代表例 | 収益性 | 開発難易度 |
|---|---------|--------|--------|-----------|
| 1 | ...     | ...    | ★★★★★ | ★★☆      |
...

番号で教えてください。または「任せる」でおすすめを自動選択します。
```

---

### STEP 2: ジャンル確定

ユーザーが選んだ番号、または「任せる」の場合は収益性・難易度のバランスが最良のものを選ぶ。

選んだジャンルをユーザーに確認してから次のステップへ進む。

---

### STEP 3: ゲーム全体設計

以下の項目をすべて決定する。

#### 3-1. ゲーム基本情報
- ゲームタイトル（英語・キャッチーな名前）
- コアループ（1文で言える遊びの核）
- ターゲットユーザー
- 使用技術（Three.js / Phaser.js / 素のCanvas）

#### 3-2. ゲーム画面構成
```
① タイトル画面
② ゲームプレイ画面
③ ゲームオーバー画面
④ （任意）スコアランキング画面
```
各画面に何を表示するか詳細に決める。

#### 3-3. ゲームルール・仕様
- 操作方法（タップ / スワイプ / 傾き）
- 勝利・敗北条件
- スコア計算方法
- 難易度上昇の仕組み
- 特殊要素（コイン・パワーアップ・ボーナスなど）

#### 3-4. 広告配置設計（最重要）

以下の3種類すべての配置場所とタイミングを決定する：

| 広告タイプ | 収益単価 | 配置ルール |
|-----------|---------|-----------|
| リワード広告 | 約1.5〜3円/回 | ゲームオーバー後「続ける？」ボタン |
| インタースティシャル | 約0.8〜1.5円/回 | ゲームオーバー → スコア画面の間 |
| バナー広告 | 約0.2〜0.4円/回 | プレイ中・ホーム画面の下部に常時 |

広告フローのテンプレート：
```
ゲームオーバー
    ↓
「続ける？（広告を見る）」← リワード広告
    ↓ NO / スキップ
インタースティシャル広告（全画面）
    ↓
スコア・リザルト画面
    ↓
ホーム画面（バナー常時表示）
```

ゲームの性質に合わせてこのフローをカスタマイズする。

#### 3-5. ビジュアル・世界観
- カラーパレット（メインカラー・アクセントカラー）
- キャラクター・オブジェクトの見た目
- 背景・エフェクトの方針
- UIのスタイル（フォント・ボタンデザイン）

#### 3-6. 技術仕様
- ファイル構成
- Three.js / Phaser.js のバージョン・CDN URL
- 対応画面サイズ・向き（縦持ち / 横持ち）
- タッチ操作の詳細仕様

---

### STEP 4: 指示文（プロンプト）生成・保存

以下のテンプレートに設計内容を埋め込み、別のClaude Codeに渡す指示文を生成する。
**テンプレートの[...]をすべて埋めた状態で出力すること。空欄を残さない。**

生成した指示文は必ずファイルとして保存する：

```
保存先: instructions/[game-title-kebab-case].md
例: instructions/coin-rush.md
```

保存後、以下を伝える：
```
指示文を instructions/[ファイル名] に保存しました。
別のClaude Codeセッションにこのファイルの内容を貼り付けて開発を依頼してください。
```

---

## 指示文テンプレート

```
# [ゲームタイトル] 開発指示

## 最重要方針
- index.html 1ファイルで完全に動作するゲームを作る
- コードは最後まで書き切る。省略・TODO・「以下同様」は禁止
- スマホ縦持ちで快適に遊べることを最優先する
- 見た目・操作感・エフェクトすべてにこだわり、ストアに出せる品質にする

---

## 概要
[ゲーム概要を3〜5文で。コアループ・世界観・何が楽しいかを明記]

---

## 技術スタック
- 言語: JavaScript（ES2020+）
- 描画: [Three.js 0.165.0 / Phaser.js 3.80.0]
- CDN:
  [該当するCDNコードをそのまま記載]
- 対象: スマホブラウザ縦持ち（Capacitorでモバイル化予定）
- ファイル: index.html 1ファイルで完結

### 必須メタ設定（<head>内に必ず入れる）
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body { overflow: hidden; background: #000; touch-action: none; }
</style>

---

## コードアーキテクチャ

以下の状態管理パターンを必ず使うこと：

```js
const STATE = {
  current: 'title', // 'title' | 'playing' | 'gameover' | 'result'
  score: 0,
  bestScore: parseInt(localStorage.getItem('bestScore') || '0'),
  // [ゲーム固有の状態変数を追加]
};

function setState(newState) {
  STATE.current = newState;
  // 画面の表示切り替えはすべてここで管理
}
```

ゲームループは requestAnimationFrame を使い、deltaTime で物理を計算する：

```js
let lastTime = 0;
function gameLoop(timestamp) {
  const dt = Math.min((timestamp - lastTime) / 1000, 0.05); // 最大50ms
  lastTime = timestamp;
  if (STATE.current === 'playing') update(dt);
  render();
  requestAnimationFrame(gameLoop);
}
requestAnimationFrame(gameLoop);
```

---

## 画面構成

### 画面①：タイトル画面（state: 'title'）
- 背景: [詳細なビジュアル]
- 表示要素:
  - ゲームタイトル（大きく・インパクトある表示）
  - サブタイトルまたはキャッチコピー
  - ハイスコア表示（localStorageから取得）
  - STARTボタン（タップで即ゲーム開始）
  - 操作説明（アイコン付きで直感的に）
- 演出: [ループするアニメーション・パーティクルなど]

### 画面②：ゲームプレイ画面（state: 'playing'）
- ゲームキャンバス全画面
- HUD（常時表示）:
  - 現在スコア（上部中央 or 左上）
  - [ゲーム固有のUI要素: ライフ・タイム・コンボなど]
  - バナー広告エリア（下部60px・ゲームエリアはその上まで）
- [ゲームの詳細な見た目・オブジェクト・背景]

### 画面③：ゲームオーバー画面（state: 'gameover'）
- 暗転フェードイン（0.4秒）
- 表示要素:
  - 「GAME OVER」テキスト（アニメーション付き）
  - 今回スコア
  - ハイスコア（更新時は「NEW RECORD!」演出）
  - 「▶ 広告を見て続ける」ボタン（リワード広告）
  - 「最初からやり直す」ボタン（インタースティシャル広告 → リスタート）
- バナー広告（下部）

### 画面④：リザルト画面（state: 'result'）※任意
[詳細 or 不要なら削除]

---

## ゲームルール・仕様

### 操作
- [操作1]: [詳細・タッチ座標の処理方法まで記載]
- [操作2]: [詳細]
- キーボード対応（PC確認用）: [キー割り当て]

### タッチ実装（必ず以下のパターンを使う）
```js
let touchStartX = 0, touchStartY = 0;
canvas.addEventListener('touchstart', e => {
  e.preventDefault();
  touchStartX = e.touches[0].clientX;
  touchStartY = e.touches[0].clientY;
}, { passive: false });

canvas.addEventListener('touchend', e => {
  e.preventDefault();
  const dx = e.changedTouches[0].clientX - touchStartX;
  const dy = e.changedTouches[0].clientY - touchStartY;
  // [スワイプ判定・タップ判定のロジック]
}, { passive: false });
```

### ゲーム目的・勝敗条件
- 目的: [詳細]
- ゲームオーバー条件: [詳細]
- クリア条件: [あれば詳細・なければ「なし（エンドレス）」]

### スコア設計
- 基本スコア: [計算式・例: 1秒生存ごとに+10点]
- ボーナスポイント: [条件と加算量]
- コンボ倍率: [あれば詳細]
- ハイスコア保存: localStorage.setItem('bestScore', STATE.bestScore)

### 難易度カーブ（数値で明記）
| 経過時間 | スピード | 障害物間隔 | その他変化 |
|---------|---------|----------|----------|
| 0〜30秒  | [値]    | [値]      | [詳細]    |
| 30〜60秒 | [値]    | [値]      | [詳細]    |
| 60秒〜   | [値]    | [値]      | [詳細]    |
| 最大値   | [値]    | [値]      | [詳細]    |

### 特殊要素
- [アイテム・パワーアップ・ボーナスなど詳細]

---

## ゲームフィール（必ず実装すること）

ゲームの「気持ちよさ」を出すために以下を必ず入れる：

### 画面エフェクト
- **スクリーンシェイク**: ダメージ・衝突時に画面を振動させる
  ```js
  function screenShake(intensity = 8, duration = 0.3) {
    // canvas または camera に offset を加算して実現
  }
  ```
- **フラッシュ**: スコア加算・アイテム取得時に白フラッシュ
- **フェード**: 画面遷移時に必ずフェードイン/アウト（0.3〜0.5秒）

### パーティクルエフェクト
- [イベント1（例: コイン取得）]: [色・数・動き]
- [イベント2（例: ゲームオーバー）]: [色・数・動き]
- 実装はシンプルなArrayで管理：
  ```js
  const particles = [];
  function spawnParticles(x, y, color, count) {
    for (let i = 0; i < count; i++) {
      particles.push({ x, y, vx: (Math.random()-0.5)*5, vy: (Math.random()-2)*5,
                       life: 1, color, size: Math.random()*8+4 });
    }
  }
  ```

### サウンドフィードバック（Web Audio API）
Web Audio APIで以下の効果音を生成する（外部ファイル不要）：
```js
const AudioCtx = window.AudioContext || window.webkitAudioContext;
const audioCtx = new AudioCtx();

function playSound(freq, type, duration, vol = 0.3) {
  const o = audioCtx.createOscillator();
  const g = audioCtx.createGain();
  o.connect(g); g.connect(audioCtx.destination);
  o.type = type; o.frequency.setValueAtTime(freq, audioCtx.currentTime);
  g.gain.setValueAtTime(vol, audioCtx.currentTime);
  g.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duration);
  o.start(); o.stop(audioCtx.currentTime + duration);
}

// 使用例
// playSound(440, 'sine', 0.1);   // コイン取得
// playSound(120, 'sawtooth', 0.4); // ゲームオーバー
// playSound(880, 'square', 0.05); // ジャンプ
```
[ゲームに合った効果音の種類と音程を指定]

### アニメーション
- ボタン: ホバー/タップ時にスケール変化（0.95倍）
- スコア更新: 数字が跳ねるアニメーション
- キャラクター/オブジェクト: [詳細なアニメーション仕様]

---

## 広告配置（必ず実装すること）

### バナー広告（常時表示）
```html
<div id="ad-banner" style="
  position: fixed; bottom: 0; left: 0; right: 0; height: 60px;
  background: #1a1a1a; border-top: 1px solid #333;
  display: flex; align-items: center; justify-content: center;
  color: #666; font-size: 11px; letter-spacing: 1px; z-index: 1000;
">ADVERTISEMENT</div>
```
- ゲームキャンバスの高さ = window.innerHeight - 60px（バナー分を引く）

### リワード広告ボタン（ゲームオーバー画面）
```html
<button id="reward-btn" style="
  background: linear-gradient(135deg, #f5a623, #f76b1c);
  color: white; border: none; border-radius: 50px;
  padding: 16px 32px; font-size: 18px; font-weight: 700;
  cursor: pointer; box-shadow: 0 4px 15px rgba(247,107,28,0.5);
">▶ 広告を見て続ける</button>
```
- 動作: クリック → [残機+1 / 時間回復 / ゲーム継続 などゲームに合った恩恵]
- AdMob連携前: クリックで即座に恩恵付与（テスト用）

### インタースティシャル広告（ゲームオーバー確定時）
```js
function showInterstitial(callback) {
  // 暗転オーバーレイを表示（AdMob連携前の代替演出）
  const overlay = document.createElement('div');
  overlay.style.cssText = 'position:fixed;inset:0;background:#000;z-index:9999;display:flex;align-items:center;justify-content:center;color:#fff;font-size:16px;';
  overlay.textContent = '広告読み込み中...';
  document.body.appendChild(overlay);
  setTimeout(() => { overlay.remove(); callback(); }, 1500);
}
// ゲームオーバー時に呼び出す: showInterstitial(() => showResultScreen());
```

---

## ビジュアル仕様

### カラーパレット
- 背景色: [HEXコード]
- メインカラー: [HEXコード・用途]
- アクセントカラー: [HEXコード・用途]
- テキスト: [HEXコード]
- 危険/警告色: [HEXコード]

### フォント
```css
font-family: 'Segoe UI', 'Helvetica Neue', Arial, sans-serif;
```
- タイトル: [サイズ・太さ・色・テキストシャドウ]
- スコア: [サイズ・太さ・色]
- ボタン: [サイズ・太さ・色]

### UIコンポーネント
- ボタン: border-radius: 50px、グラデーション、box-shadow必須
- カード/パネル: backdrop-filter: blur(10px)、半透明背景
- 画面遷移: すべてopacityフェード（CSS transition 0.3s）

### ゲームオブジェクトのビジュアル
[各オブジェクトの色・形・サイズ・アニメーションを具体的に記載]

---

## パフォーマンス要件

- **目標: 60fps維持（スマホ低スペックでも）**
- オブジェクトプーリング: 頻繁に生成/削除するオブジェクトは再利用する
  ```js
  const pool = [];
  function getFromPool() { return pool.pop() || createNewObject(); }
  function returnToPool(obj) { obj.active = false; pool.push(obj); }
  ```
- 画面外オブジェクトは毎フレーム必ず削除またはプールに戻す
- テクスチャ・画像は使わず、プリミティブ（矩形・円）のみで描画する
- deltaTime必須: すべての移動・アニメーションに dt を乗算する

---

## モバイル最適化

- **縦画面固定**: `<meta name="screen-orientation" content="portrait">`
- スクロール無効: `touch-action: none` をbodyに適用
- タップ遅延除去: `touch-action: manipulation` をボタンに適用
- セーフエリア対応: `padding-bottom: env(safe-area-inset-bottom)`（iPhone対応）
- キャンバスサイズ: `devicePixelRatio`を考慮してシャープに描画
  ```js
  const dpr = window.devicePixelRatio || 1;
  canvas.width = window.innerWidth * dpr;
  canvas.height = (window.innerHeight - 60) * dpr; // バナー60px分引く
  ctx.scale(dpr, dpr);
  ```

---

## 実装の優先順位

以下の順番で実装すること（途中で止まらず最後まで完成させる）：

1. HTML骨格・CSS・広告エリアの配置
2. ゲームループ（requestAnimationFrame）の確立
3. タイトル画面の表示
4. コアゲームメカニクス（最低限遊べる状態）
5. ゲームオーバー判定・画面遷移
6. スコア・ハイスコア（localStorage）
7. 難易度上昇ロジック
8. パーティクル・エフェクト
9. サウンドフィードバック
10. UIの仕上げ・アニメーション
11. 全体のバランス調整・デバッグ

---

## 完成チェックリスト（すべてにチェックを入れてから納品）

### 機能
- [ ] タイトル画面でSTARTボタンを押すとゲームが始まる
- [ ] ゲームオーバー条件が正しく動作する
- [ ] スコアがリアルタイムで更新される
- [ ] ハイスコアがlocalStorageに保存・読み込みされる
- [ ] ゲームオーバー画面でスコアとハイスコアが表示される
- [ ] リスタートボタンで最初から遊べる
- [ ] 難易度が時間経過で正しく上昇する
- [ ] 特殊要素（[アイテム名など]）が機能する

### 広告
- [ ] バナー広告エリアが画面下部に常時表示される
- [ ] ゲームキャンバスがバナーと重なっていない
- [ ] リワード広告ボタンがゲームオーバー画面に表示される
- [ ] リワードボタンをタップすると恩恵が発生する
- [ ] インタースティシャル演出がゲームオーバー時に表示される

### 操作・UX
- [ ] スマホタッチで正しく操作できる
- [ ] スワイプ・タップ判定が誤作動しない
- [ ] ボタンのタップ領域が十分広い（最低44×44px）
- [ ] 画面外にはみ出すUIがない
- [ ] 縦持ちで全要素が正しく表示される

### 品質
- [ ] 60fps で動作する（低スペック端末想定）
- [ ] スクリーンシェイクが実装されている
- [ ] パーティクルエフェクトが実装されている
- [ ] サウンドフィードバックが実装されている（Web Audio API）
- [ ] 画面遷移にフェードアニメーションがある
- [ ] ハイスコア更新時に「NEW RECORD!」演出がある
- [ ] コンソールエラーが0件である
```

---

## 技術スタック（共通）

```
言語      : JavaScript
3D描画    : Three.js 0.165.0（CDN固定）
2D描画    : Phaser.js 3.80.0（CDN固定）
UI        : HTML / CSS
モバイル化 : Capacitor（別セッションで対応）
広告      : Google AdMob（ID設置のみ・実連携は別途）
ストア    : Google Play → 将来 App Store（Mac必要）
```

### Three.js CDN（バージョン固定）
```html
<script type="importmap">
{
  "imports": {
    "three": "https://cdn.jsdelivr.net/npm/three@0.165.0/build/three.module.js",
    "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.165.0/examples/jsm/"
  }
}
</script>
```

### Phaser.js CDN（バージョン固定）
```html
<script src="https://cdn.jsdelivr.net/npm/phaser@3.80.0/dist/phaser.min.js"></script>
```

---

## 広告収益の基礎知識

| 広告タイプ | 1回あたり収益 | 優先度 |
|-----------|------------|--------|
| リワード広告 | 約1.5〜3円 | 最高 |
| インタースティシャル | 約0.8〜1.5円 | 高 |
| バナー | 約0.2〜0.4円 | 常時 |

収益目標：
| 段階 | 本数 | 月収目安 |
|------|------|---------|
| 初期 | 1〜3本 | 1,000〜5,000円 |
| 中期 | 5〜10本 | 1〜3万円 |
| 安定 | 10本以上 | 3〜10万円 |

---

## 既存ゲーム（参考実装）

### Cloud Runner（3Dエンドレスランナー）
- ファイル：`index.html`
- 機能：3レーン・ジャンプ・スライド・障害物3種・コイン・ライフ制・スコア
- 操作：キーボード＋タッチスワイプ対応
- テンプレートとして流用可能

---

## 参考情報（2026年調査済み）

- Block Blast：3億DL超・パズル系の代表
- Pizza Ready：DL数1位・ミニゲーム系
- ハイブリッドカジュアル（軽い育成要素付き）が主流トレンド
- 広告収益が97〜99%を占める
- ハイブリッドカジュアルは2024年比37%収益成長
