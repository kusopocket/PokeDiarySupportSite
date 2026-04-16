# PokeDiary Spotify風リデザイン 実装計画

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `support-site/` 配下の3つのHTMLファイルをSpotifyインスパイアのダークテーマ（#121212背景 + #E8344Aアクセント）にリデザインする。

**Architecture:** 各HTMLファイルのインラインCSSブロックを完全に置き換え、HTMLの構造的変更（ヒーロー・機能グリッド）を加える。ビルドシステムなし・依存ライブラリなし（Google Fontsのみ追加）。多言語ロジック（data-lang属性・localStorage）は既存のまま維持。

**Tech Stack:** 純粋なHTML5 + CSS3 + バニラJS、Google Fonts（DM Sans / M PLUS 1p / Noto Sans SC）

---

## ファイルマップ

| ファイル | 変更種別 | 内容 |
|----------|---------|------|
| `support-site/index.html` | CSS完全置換 + HTML一部変更 | `<style>`ブロック全取替え、hero/features HTMLを新レイアウトに書き換え |
| `support-site/privacy.html` | CSS完全置換 | `<style>`ブロック全取替え（HTMLは保持） |
| `support-site/terms.html` | CSS完全置換 | `<style>`ブロック全取替え（HTMLは保持） |

---

## Task 1: index.html — Google Fontsリンクを追加

**Files:**
- Modify: `support-site/index.html`（`<meta name="viewport">` 直後）

- [ ] **Step 1: Editツールで `<meta name="viewport">` 行の直後にフォントリンクを挿入する**

```html
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,400;0,9..40,700;0,9..40,800&family=M+PLUS+1p:wght@400;700;800&family=Noto+Sans+SC:wght@400;700&display=swap" rel="stylesheet">
```

（`old_string` は `  <meta name="viewport" content="width=device-width, initial-scale=1.0">` のみ）

- [ ] **Step 2: ブラウザで `http://localhost:8000` を開き、フォントが読み込まれていることをDevToolsのNetworkタブで確認する**

```bash
cd "support-site" && python3 -m http.server 8000
```

---

## Task 2: index.html — CSSブロックを完全置換

**Files:**
- Modify: `support-site/index.html`（行8〜293の `<style>...</style>` ブロック）

- [ ] **Step 1: Editツールで `<style>` から `</style>` までを以下で全置換する**

`old_string` の先頭: `  <style>` から始まる既存CSSブロック全体（`  </style>` まで）。
`new_string` は以下：

```html
  <style>
    :root {
      --bg: #121212;
      --surface: #181818;
      --surface-2: #282828;
      --surface-3: #383838;
      --accent: #E8344A;
      --accent-dark: #C9283E;
      --text: #FFFFFF;
      --text-muted: #B3B3B3;
      --text-faint: #555555;
      --border: rgba(255,255,255,0.06);
      --accent-glow: rgba(232,52,74,0.25);
      --accent-shadow: rgba(232,52,74,0.4);
      --radius-sm: 8px;
      --radius-md: 12px;
      --radius-lg: 16px;
      --radius-pill: 28px;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'M PLUS 1p', 'DM Sans', 'Noto Sans SC', sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.6;
    }

    /* ===== HEADER ===== */
    header {
      background: rgba(18,18,18,0.92);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--border);
      position: sticky;
      top: 0;
      z-index: 100;
    }
    .header-inner {
      max-width: 900px;
      margin: 0 auto;
      padding: 0 24px;
      height: 56px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .logo-link {
      display: flex;
      align-items: center;
      gap: 8px;
      text-decoration: none;
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
      font-weight: 800;
      font-size: 16px;
      letter-spacing: -0.3px;
    }
    .logo-link img { width: 28px; height: 28px; border-radius: 7px; object-fit: cover; }
    nav { display: flex; gap: 4px; align-items: center; }
    nav a {
      text-decoration: none;
      color: var(--text-muted);
      font-family: 'DM Sans', sans-serif;
      font-size: 13px;
      padding: 5px 10px;
      border-radius: var(--radius-sm);
      transition: background 0.15s, color 0.15s;
    }
    nav a:hover { background: var(--surface-2); color: var(--text); }

    /* ===== LANG SWITCHER ===== */
    .lang-switcher {
      display: flex;
      gap: 2px;
      background: var(--surface-2);
      border-radius: var(--radius-sm);
      padding: 3px;
      margin-left: 8px;
    }
    .lang-btn {
      border: none;
      background: transparent;
      padding: 3px 8px;
      border-radius: 5px;
      font-family: 'DM Sans', sans-serif;
      font-size: 11px;
      font-weight: 600;
      cursor: pointer;
      color: var(--text-muted);
      transition: all 0.15s;
    }
    .lang-btn.active { background: var(--surface-3); color: var(--text); }

    /* ===== HERO ===== */
    .hero {
      max-width: 900px;
      margin: 0 auto;
      padding: 56px 24px 64px;
      position: relative;
      overflow: hidden;
    }
    .hero::before {
      content: '';
      position: absolute;
      top: -80px; left: -80px;
      width: 360px; height: 360px;
      background: radial-gradient(circle, var(--accent-glow) 0%, transparent 70%);
      pointer-events: none;
    }
    .hero-inner {
      display: flex;
      align-items: center;
      gap: 32px;
      position: relative;
    }
    .hero-text { flex: 1; }
    .hero-eyebrow {
      font-family: 'DM Sans', sans-serif;
      font-size: 11px;
      font-weight: 700;
      letter-spacing: 2px;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 10px;
    }
    .hero h1 {
      font-family: 'DM Sans', sans-serif;
      font-size: clamp(32px, 5vw, 48px);
      font-weight: 800;
      letter-spacing: -1px;
      line-height: 1.1;
      margin-bottom: 12px;
      color: var(--text);
    }
    .hero-tagline {
      font-size: 14px;
      color: var(--text-muted);
      line-height: 1.6;
      margin-bottom: 28px;
      max-width: 340px;
    }
    .badge-link {
      display: inline-block;
      text-decoration: none;
      transition: transform 0.2s, filter 0.2s;
    }
    .badge-link:hover { transform: scale(1.05); }
    .badge-link svg {
      height: 52px;
      width: auto;
      display: block;
      filter: drop-shadow(0 4px 14px var(--accent-shadow));
    }
    .hero-phone {
      flex: 0 0 110px;
      display: flex;
      justify-content: center;
    }
    .phone-frame {
      width: 88px;
      height: 168px;
      background: #1a1a1a;
      border-radius: 20px;
      border: 2px solid #333;
      overflow: hidden;
      position: relative;
      box-shadow: 0 20px 60px rgba(0,0,0,0.7), 0 0 0 1px rgba(255,255,255,0.04);
    }
    .phone-screen {
      width: 100%;
      height: 100%;
      background: linear-gradient(160deg, var(--accent) 0%, #7a1a26 100%);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 40px;
    }
    .phone-notch {
      position: absolute;
      top: 8px; left: 50%;
      transform: translateX(-50%);
      width: 28px; height: 6px;
      background: #333;
      border-radius: 3px;
      z-index: 1;
    }

    /* ===== SECTIONS ===== */
    .section {
      max-width: 900px;
      margin: 0 auto;
      padding: 48px 24px;
    }
    .section-header {
      display: flex;
      align-items: center;
      gap: 12px;
      margin-bottom: 24px;
    }
    .section-title {
      font-family: 'DM Sans', sans-serif;
      font-size: 20px;
      font-weight: 800;
      letter-spacing: -0.3px;
      color: var(--text);
      white-space: nowrap;
    }
    .section-line {
      flex: 1;
      height: 1px;
      background: linear-gradient(90deg, rgba(232,52,74,0.4) 0%, transparent 100%);
    }

    /* ===== FEATURES ALBUM GRID ===== */
    .features-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 12px;
    }
    .feature-tile {
      aspect-ratio: 1;
      border-radius: var(--radius-md);
      background: var(--surface-2);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 8px;
      transition: background 0.15s, transform 0.15s;
      cursor: default;
    }
    .feature-tile:hover { background: var(--surface-3); transform: scale(1.03); }
    .feature-tile.highlight {
      background: var(--accent);
      box-shadow: 0 4px 20px var(--accent-shadow);
    }
    .feature-tile.highlight:hover { background: var(--accent-dark); }
    .feature-icon { font-size: 28px; }
    .feature-label {
      font-size: 11px;
      font-weight: 700;
      color: var(--text);
      text-align: center;
      line-height: 1.3;
      padding: 0 6px;
    }

    /* ===== FAQ ===== */
    .faq-list { display: flex; flex-direction: column; gap: 10px; }
    details {
      background: var(--surface);
      border-radius: var(--radius-md);
      overflow: hidden;
      border-left: 3px solid transparent;
      transition: border-color 0.15s;
    }
    details:hover { border-left-color: var(--accent); }
    details[open] { border-left-color: var(--accent); }
    summary {
      padding: 16px 20px;
      font-weight: 700;
      font-size: 14px;
      color: var(--text);
      cursor: pointer;
      display: flex;
      justify-content: space-between;
      align-items: center;
      list-style: none;
      gap: 12px;
    }
    summary::-webkit-details-marker { display: none; }
    summary::after {
      content: '+';
      font-size: 20px;
      color: var(--accent);
      font-weight: 300;
      flex-shrink: 0;
      transition: transform 0.2s;
    }
    details[open] summary::after { transform: rotate(45deg); }
    .faq-answer {
      padding: 14px 20px 16px;
      font-size: 13px;
      color: var(--text-muted);
      line-height: 1.7;
      border-top: 1px solid var(--border);
    }

    /* ===== CONTACT ===== */
    .contact-card {
      background: var(--surface);
      border-radius: var(--radius-lg);
      padding: 32px;
      text-align: center;
    }
    .contact-card p {
      color: var(--text-muted);
      font-size: 13px;
      line-height: 1.7;
      margin-bottom: 20px;
    }
    .contact-link {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      background: transparent;
      border: 1.5px solid var(--accent);
      color: var(--accent);
      text-decoration: none;
      padding: 11px 24px;
      border-radius: var(--radius-pill);
      font-family: 'DM Sans', sans-serif;
      font-weight: 700;
      font-size: 13px;
      transition: background 0.15s, color 0.15s;
    }
    .contact-link:hover { background: var(--accent); color: var(--text); }

    /* ===== FOOTER ===== */
    footer {
      background: #0a0a0a;
      color: var(--text-faint);
      text-align: center;
      padding: 32px 24px;
      font-family: 'DM Sans', sans-serif;
      font-size: 12px;
      line-height: 1.8;
      border-top: 1px solid var(--border);
    }
    .footer-logo {
      font-size: 15px;
      font-weight: 800;
      letter-spacing: -0.3px;
      color: var(--text);
      margin-bottom: 12px;
    }
    .footer-links { margin-bottom: 12px; }
    footer a { color: var(--text-muted); text-decoration: none; margin: 0 10px; transition: color 0.15s; }
    footer a:hover { color: var(--text); }
    .footer-notice { font-size: 11px; color: var(--text-faint); max-width: 600px; margin: 12px auto 0; }

    /* ===== DIVIDER ===== */
    .section-divider { border: none; border-top: 1px solid var(--border); margin: 0; }

    /* ===== LANGUAGE ===== */
    [data-lang] { display: none; }
    [data-lang].active { display: block; }

    /* ===== RESPONSIVE ===== */
    @media (max-width: 600px) {
      .header-inner { padding: 0 16px; }
      nav a { display: none; }
      .hero { padding: 40px 20px 48px; }
      .hero-inner { gap: 20px; }
      .hero-phone { flex: 0 0 80px; }
      .phone-frame { width: 66px; height: 126px; }
      .phone-screen { font-size: 30px; }
      .section { padding: 36px 16px; }
    }
    @media (max-width: 479px) {
      .features-grid { grid-template-columns: repeat(2, 1fr); }
    }
  </style>
```

---

## Task 3: index.html — ヒーローセクションのHTML書き換え

**Files:**
- Modify: `support-site/index.html`（hero section）

既存の `<section class="hero">` ブロックは中央揃えのレイアウト。これをスプリットレイアウトに変更する。

- [ ] **Step 1: Editツールで `<section class="hero">` 全体を以下に置換する**

`old_string`: `<!-- ===== HERO ===== -->\n<section class="hero">` から `</section>` の最初（`<!-- ===== FEATURES =====` の手前）まで。具体的には以下の `old_string` を使う:

```html
<!-- ===== HERO ===== -->
<section class="hero">
  <div class="hero-icon">
```

`new_string`:

```html
<!-- ===== HERO ===== -->
<section class="hero">
  <div class="hero-inner">
    <div class="hero-text">
      <div class="hero-eyebrow">iOS App</div>
      <h1>PokeDiary</h1>
```

次に、`</h1>` の直後にある `<p class="hero-tagline"...>` ブロック群とバッジリンク群は維持しつつ、それらを `</div><!-- /.hero-text -->` と `<div class="hero-phone">...</div>` で囲む形に変更する。

**具体的な対応手順:**

1. 最初のEditで hero-icon divをhero-innerに変更:

old_string:
```html
<!-- ===== HERO ===== -->
<section class="hero">
  <div class="hero-icon">
    <img src="data:image/png;base64,
```
new_string:
```html
<!-- ===== HERO ===== -->
<section class="hero">
  <div class="hero-inner">
    <div class="hero-text">
      <div class="hero-eyebrow">iOS App</div>
      <h1>PokeDiary</h1>
      <!-- logo hidden (phone mockup used instead) -->
      <div style="display:none"><img src="data:image/png;base64,
```

2. 次のEditで `alt="PokeDiary">` の直後を修正:

old_string:
```
" alt="PokeDiary">
  </div>
  <h1>PokeDiary</h1>
```
new_string:
```
" alt="PokeDiary"></div>
```

3. バッジリンク群の末尾 `</section>` の直前に `hero-phone` divを追加:

old_string（最後のバッジリンク直後）:
```html
  </a>

</section>
```
new_string:
```html
    </a>
    </div><!-- /.hero-text -->
    <div class="hero-phone">
      <div class="phone-frame">
        <div class="phone-notch"></div>
        <div class="phone-screen">📒</div>
      </div>
    </div>
  </div><!-- /.hero-inner -->
</section>
```

- [ ] **Step 2: ブラウザをリロードしてヒーローが左右スプリットで表示されることを確認する**

---

## Task 4: index.html — 機能グリッドのHTML書き換え

**Files:**
- Modify: `support-site/index.html`（features section）

既存の `feature-card` グリッドをアルバムタイル形式に変換する。

- [ ] **Step 1: セクションタイトル部分を `section-header` 構造に変更する**

old_string:
```html
  <h2 class="section-title">
    <span data-lang="ja active">主な機能</span>
    <span data-lang="en">Features</span>
    <span data-lang="zh">主要功能</span>
  </h2>
```
new_string:
```html
  <div class="section-header">
    <h2 class="section-title">
      <span data-lang="ja active">主な機能</span>
      <span data-lang="en">Features</span>
      <span data-lang="zh">主要功能</span>
    </h2>
    <div class="section-line"></div>
  </div>
```

- [ ] **Step 2: feature-cardグリッド全体をアルバムグリッドに置換する**

old_string（`<div class="features-grid">` から閉じ `</div>` まで）:
```html
  <div class="features-grid">
```
…（feature-card 6つ）…
最後の feature-card の `</div>` 直後の `</div>` まで。

new_string:
```html
  <div class="features-grid">

    <div class="feature-tile highlight">
      <div class="feature-icon">⚔️</div>
      <div class="feature-label">
        <span data-lang="ja active">対戦記録</span>
        <span data-lang="en">Battle Records</span>
        <span data-lang="zh">对战记录</span>
      </div>
    </div>

    <div class="feature-tile">
      <div class="feature-icon">📊</div>
      <div class="feature-label">
        <span data-lang="ja active">対戦統計</span>
        <span data-lang="en">Battle Stats</span>
        <span data-lang="zh">对战统计</span>
      </div>
    </div>

    <div class="feature-tile">
      <div class="feature-icon">🔁</div>
      <div class="feature-label">
        <span data-lang="ja active">連戦モード</span>
        <span data-lang="en">Multi-Battle</span>
        <span data-lang="zh">多场对战</span>
      </div>
    </div>

    <div class="feature-tile">
      <div class="feature-icon">⭐</div>
      <div class="feature-label">
        <span data-lang="ja active">お気に入り</span>
        <span data-lang="en">Favorites</span>
        <span data-lang="zh">收藏管理</span>
      </div>
    </div>

    <div class="feature-tile">
      <div class="feature-icon">📤</div>
      <div class="feature-label">
        <span data-lang="ja active">SNSシェア</span>
        <span data-lang="en">SNS Share</span>
        <span data-lang="zh">SNS 分享</span>
      </div>
    </div>

    <div class="feature-tile">
      <div class="feature-icon">☁️</div>
      <div class="feature-label">
        <span data-lang="ja active">iCloud</span>
        <span data-lang="en">iCloud</span>
        <span data-lang="zh">iCloud</span>
      </div>
    </div>

  </div>
```

- [ ] **Step 3: FAQ・お問い合わせのセクションタイトルも `section-header` 構造に統一する**

FAQのセクション:
old_string:
```html
  <h2 class="section-title">
    <span data-lang="ja active">よくある質問</span>
    <span data-lang="en">FAQ</span>
    <span data-lang="zh">常见问题</span>
  </h2>
```
new_string:
```html
  <div class="section-header">
    <h2 class="section-title">
      <span data-lang="ja active">よくある質問</span>
      <span data-lang="en">FAQ</span>
      <span data-lang="zh">常见问题</span>
    </h2>
    <div class="section-line"></div>
  </div>
```

お問い合わせのセクション:
old_string:
```html
  <h2 class="section-title">
    <span data-lang="ja active">お問い合わせ</span>
    <span data-lang="en">Contact</span>
    <span data-lang="zh">联系我们</span>
  </h2>
```
new_string:
```html
  <div class="section-header">
    <h2 class="section-title">
      <span data-lang="ja active">お問い合わせ</span>
      <span data-lang="en">Contact</span>
      <span data-lang="zh">联系我们</span>
    </h2>
    <div class="section-line"></div>
  </div>
```

- [ ] **Step 4: `[data-lang].active` のルールを `block` から `inline` に変更（feature-label内スパン対応）**

Task 2で追加したCSSの以下の行を更新する:

old_string (CSSブロック内):
```css
    [data-lang].active { display: block; }
```
new_string:
```css
    [data-lang].active { display: block; }
    .feature-label [data-lang].active { display: inline; }
```

- [ ] **Step 5: ブラウザをリロードしてアルバムグリッドが3×2で表示され、最初のタイルが赤いことを確認する**

---

## Task 5: index.html — フッターに `.footer-logo` クラスを追加

**Files:**
- Modify: `support-site/index.html`（footer）

既存フッターの "PokeDiary" テキストに `.footer-logo` クラスを適用する。

- [ ] **Step 1: フッター内のロゴテキストを更新する**

既存フッターの構造を確認してから、ロゴテキストに `class="footer-logo"` を追加する。フッターの `<footer>` タグ直後を以下のように変更する:

old_string（フッター内、既存のロゴ/テキスト部分）:
```html
<footer>
```
new_string:
```html
<footer>
  <div class="footer-logo">PokeDiary</div>
```

（すでに存在するフッター内テキストの重複を確認し、既存の "PokeDiary" テキストは削除する）

- [ ] **Step 2: ブラウザで全セクションをスクロールして確認する**
  - ヘッダーが sticky でスクロール時に半透明になること
  - ヒーローのラジアルグローが左上に出ていること
  - フィーチャーグリッドが正方形タイルであること
  - FAQが左ボーダー付きのダークカードであること
  - フッターが `#0a0a0a` で表示されていること

---

## Task 6: privacy.html — Google Fonts追加 + CSS完全置換

**Files:**
- Modify: `support-site/privacy.html`

- [ ] **Step 1: Google Fontsリンクを追加する（Task 1と同じ方法）**

old_string:
```html
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
```
new_string:
```html
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,400;0,9..40,700;0,9..40,800&family=M+PLUS+1p:wght@400;700;800&family=Noto+Sans+SC:wght@400;700&display=swap" rel="stylesheet">
```

- [ ] **Step 2: Editツールで `<style>` から `</style>` 全体を以下で置換する**

```html
  <style>
    :root {
      --bg: #121212;
      --surface: #181818;
      --surface-2: #282828;
      --surface-3: #383838;
      --accent: #E8344A;
      --accent-dark: #C9283E;
      --text: #FFFFFF;
      --text-muted: #B3B3B3;
      --text-faint: #555555;
      --border: rgba(255,255,255,0.06);
      --radius-sm: 8px;
      --radius-md: 12px;
      --radius-lg: 16px;
      --radius-pill: 28px;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'M PLUS 1p', 'DM Sans', 'Noto Sans SC', sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.6;
    }

    /* ===== HEADER ===== */
    header {
      background: rgba(18,18,18,0.92);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--border);
      position: sticky;
      top: 0;
      z-index: 100;
    }
    .header-inner {
      max-width: 800px;
      margin: 0 auto;
      padding: 0 24px;
      height: 56px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .logo-link {
      display: flex;
      align-items: center;
      gap: 8px;
      text-decoration: none;
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
      font-weight: 800;
      font-size: 16px;
      letter-spacing: -0.3px;
    }
    .logo-link img { width: 28px; height: 28px; border-radius: 7px; object-fit: cover; }

    /* ===== LANG SWITCHER ===== */
    .lang-switcher {
      display: flex;
      gap: 2px;
      background: var(--surface-2);
      border-radius: var(--radius-sm);
      padding: 3px;
    }
    .lang-btn {
      border: none;
      background: transparent;
      padding: 3px 8px;
      border-radius: 5px;
      font-family: 'DM Sans', sans-serif;
      font-size: 11px;
      font-weight: 600;
      cursor: pointer;
      color: var(--text-muted);
      transition: all 0.15s;
    }
    .lang-btn.active { background: var(--surface-3); color: var(--text); }

    /* ===== PAGE HERO ===== */
    .page-hero {
      background: linear-gradient(135deg, #1a0a0d 0%, #2a1218 100%);
      border-bottom: 1px solid var(--border);
      color: var(--text);
      padding: 48px 24px;
      text-align: center;
      position: relative;
      overflow: hidden;
    }
    .page-hero::before {
      content: '';
      position: absolute;
      top: -60px; left: 50%; transform: translateX(-50%);
      width: 300px; height: 300px;
      background: radial-gradient(circle, rgba(232,52,74,0.2) 0%, transparent 70%);
      pointer-events: none;
    }
    .page-hero h1 {
      font-family: 'DM Sans', sans-serif;
      font-size: 26px;
      font-weight: 800;
      margin-bottom: 8px;
      position: relative;
    }
    .page-hero .updated {
      font-family: 'DM Sans', sans-serif;
      font-size: 13px;
      color: var(--text-muted);
      margin-top: 8px;
      position: relative;
    }

    /* ===== CONTENT ===== */
    .content-wrap {
      max-width: 800px;
      margin: 0 auto;
      padding: 40px 24px 64px;
    }

    .policy-card {
      background: var(--surface);
      border-radius: var(--radius-lg);
      padding: 28px 32px;
      margin-bottom: 12px;
    }
    .policy-card h2 {
      font-family: 'DM Sans', sans-serif;
      font-size: 15px;
      font-weight: 700;
      color: var(--text);
      margin-bottom: 12px;
      padding-bottom: 10px;
      border-bottom: 1px solid var(--border);
      padding-left: 10px;
      border-left: 3px solid var(--accent);
    }
    .policy-card p {
      font-size: 13px;
      color: var(--text-muted);
      line-height: 1.8;
      white-space: pre-line;
    }

    .back-link {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      color: var(--accent);
      text-decoration: none;
      font-family: 'DM Sans', sans-serif;
      font-size: 13px;
      font-weight: 700;
      margin-bottom: 24px;
      transition: opacity 0.15s;
    }
    .back-link:hover { opacity: 0.75; }

    [data-lang] { display: none; }
    [data-lang].active { display: block; }

    /* ===== FOOTER ===== */
    footer {
      background: #0a0a0a;
      color: var(--text-faint);
      text-align: center;
      padding: 24px;
      font-family: 'DM Sans', sans-serif;
      font-size: 12px;
      border-top: 1px solid var(--border);
    }
    footer a { color: var(--text-muted); text-decoration: none; margin: 0 8px; transition: color 0.15s; }
    footer a:hover { color: var(--text); }

    @media (max-width: 600px) {
      .content-wrap { padding: 24px 16px 48px; }
      .policy-card { padding: 20px; }
    }
  </style>
```

- [ ] **Step 3: `http://localhost:8000/privacy.html` でダークテーマが正しく表示されることを確認する**

---

## Task 7: terms.html — Google Fonts追加 + CSS完全置換

**Files:**
- Modify: `support-site/terms.html`

- [ ] **Step 1: Google Fontsリンクを追加する（Task 6 Step 1と同一手順）**

- [ ] **Step 2: Editツールで `<style>` から `</style>` 全体をTask 6 Step 2と同一のCSSで置換する**

（terms.htmlとprivacy.htmlのCSSは完全に同じ内容）

- [ ] **Step 3: `http://localhost:8000/terms.html` でダークテーマが正しく表示されることを確認する**

---

## Task 8: 言語切り替え・レスポンシブの動作確認

**Files:** なし（確認のみ）

- [ ] **Step 1: index.htmlで言語切り替えを確認する**

  `http://localhost:8000` でJA→EN→ZH→JAと切り替え、以下がすべて切り替わることを確認:
  - ヒーローのタグライン
  - ヒーローのApp Storeバッジ（JP/US-UK/CN）
  - 機能グリッドのラベル（対戦記録 / Battle Records / 对战记录）
  - FAQの質問・回答テキスト
  - お問い合わせ文

- [ ] **Step 2: レスポンシブを確認する**

  DevToolsでブラウザ幅を375pxに設定し:
  - ヒーローがスプリットレイアウトのまま崩れないこと
  - 機能グリッドが3列で表示されること
  
  320pxに設定し:
  - 機能グリッドが2列にフォールバックすること（`@media (max-width: 479px)` のルール）

- [ ] **Step 3: privacy.html・terms.htmlの言語切り替えを確認する**

  JA→EN→ZH→JAで全言語の本文が切り替わること。

---

## Task 9: Gitセットアップとコミット

**Files:** なし（gitコマンドのみ）

- [ ] **Step 1: プロジェクトルートにgitリポジトリを初期化する（未設定の場合）**

```bash
cd "/Users/maiki-dantyou/Main Folder/Programming/AntiGravity/PokeDiary SupportSite"
git init
git add support-site/index.html support-site/privacy.html support-site/terms.html docs/
git commit -m "デザインをSpotify風ダークテーマにリデザイン

- カラーテーマをダーク (#121212) + ポケモンレッド (#E8344A) に変更
- ヒーローをスプリットレイアウト (左テキスト＋右スマホモックアップ) に変更
- 機能グリッドをSpotify風アルバムタイル (3×2) に変更
- フォントをDM Sans (EN) / M PLUS 1p (JP) / Noto Sans SC (ZH) に変更
- privacy.html・terms.htmlも同一テーマで統一

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>"
```

- [ ] **Step 2: リモートが設定されている場合はpushする**

```bash
git push -u origin main
```

（リモートが未設定の場合はスキップ）
