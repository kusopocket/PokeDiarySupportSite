# PokeDiary サポートサイト Spotify風リデザイン 設計書

**作成日**: 2026-04-16  
**対象**: `support-site/index.html`, `support-site/privacy.html`, `support-site/terms.html`

---

## 概要

PokeDiary（ポケモンカードゲーム対戦記録iOS App）のGitHub Pages公開用サポートサイトを、Spotify風のダークデザインにフルリデザインする。既存の多言語対応（JP/EN/ZH）・セクション構成・data-lang属性による言語切り替えロジックはそのまま保持する。

---

## デザイントークン

### カラーパレット

| 変数名 | 値 | 用途 |
|--------|-----|------|
| `--bg` | `#121212` | ページ背景 |
| `--surface` | `#181818` | カード・FAQ背景 |
| `--surface-2` | `#282828` | フィーチャータイル・lang switcher |
| `--surface-3` | `#383838` | ホバー・active state |
| `--accent` | `#E8344A` | CTAボタン・ハイライトタイル・ボーダー |
| `--accent-dark` | `#C9283E` | アクセントホバー |
| `--text` | `#FFFFFF` | 本文・見出し |
| `--text-muted` | `#B3B3B3` | サブテキスト・説明文 |
| `--text-faint` | `#555555` | フッターコピーライト |
| `--border` | `rgba(255,255,255,0.06)` | ヘッダー下線など |
| `--accent-glow` | `rgba(232,52,74,0.25)` | ヒーローラジアルグロー |
| `--accent-shadow` | `rgba(232,52,74,0.4)` | CTAボタン影 |

### タイポグラフィ

| 言語 | フォント | Google Fonts ウェイト |
|------|---------|----------------------|
| 🇯🇵 日本語 | M PLUS 1p | 400, 700, 800 |
| 🇺🇸 英語 | DM Sans | 400, 700, 800 |
| 🇨🇳 中国語（簡体字） | Noto Sans SC | 400, 700 |

**フォント適用ルール**:
- ヘッダーロゴ・セクション見出し・ボタン・英字 → `DM Sans`
- 日本語テキスト（tagline・FAQ・説明文）→ `M PLUS 1p`
- 中国語テキスト → `Noto Sans SC`
- `font-family` の順序: `'M PLUS 1p', 'DM Sans', 'Noto Sans SC', sans-serif`（body共通）

### スペーシング・形状

| 変数名 | 値 |
|--------|-----|
| `--radius-sm` | `8px` |
| `--radius-md` | `12px` |
| `--radius-lg` | `16px` |
| `--radius-pill` | `28px` |

---

## レイアウト構成

### 共通（全3ファイル）

#### ヘッダー
- `position: sticky; top: 0` + `backdrop-filter: blur(12px)` + 半透明背景
- ロゴ: 赤い角丸アイコン（28×28px、`border-radius: 7px`） + "PokeDiary"テキスト（DM Sans 800）
- ナビ: "サポート" リンク（グレー、ホバーで`--surface-2`背景）
- 言語切り替え: JP / EN / 中 の3ボタン、`--surface-2`背景のピル型コンテナ、activeは`--surface-3`+白テキスト

---

### index.html のセクション構成

#### 1. ヒーロー（スプリットレイアウト）
- 背景: `#121212` + 左上から`radial-gradient`でアクセントカラーのグロー効果
- **左カラム（flex: 1）**:
  - eyebrow: "iOS App"（DM Sans 700、letter-spacing 2px、uppercase、`--accent`色）
  - h1: "PokeDiary"（DM Sans 800、34px、letter-spacing -1px）
  - tagline: 多言語（M PLUS 1p / DM Sans / Noto Sans SC、13px、`--text-muted`）
  - CTA: 既存のApp StoreバッジSVGを `badge-link` クラスでラップしたまま維持。バッジに `filter: brightness(1) drop-shadow(0 4px 14px rgba(232,52,74,0.35))` を適用してアクセント感を付与
- **右カラム（flex: 0 0 90px）**:
  - スマホフレームモックアップ（72×136px、`border: 2px solid #333`）
  - スクリーン: `linear-gradient(160deg, #E8344A, #7a1a26)` + 📒絵文字
  - ノッチ: 上部中央に小さい黒いバー

#### 2. 機能紹介（アルバムグリッド）
- セクションヘッダー: タイトル + 右に`linear-gradient`の細線（`--accent`→透明）
- `display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px`
- 6タイル（対戦記録・対戦統計・連戦モード・お気に入り・SNSシェア・iCloud）
- 各タイル: `aspect-ratio: 1`、絵文字アイコン（26px）＋ラベル（M PLUS 1p 700 10px）
- **最初の1タイルのみ** `background: --accent` + `box-shadow: 0 4px 18px --accent-shadow`
- 残り5タイル: `background: --surface-2`
- ホバー: `background: --surface-3` + `transform: scale(1.03)`
- ラベルの多言語対応: `data-lang`属性で各タイル内に3言語分を格納

#### 3. FAQ
- セクションヘッダー（上と同じパターン）
- 各FAQアイテム: `background: --surface`、`border-radius: --radius-md`、`border-left: 3px solid transparent`
- ホバーで `border-left-color: --accent`
- Q: 700ウェイト 白テキスト 13px / A: `--text-muted` 12px

#### 4. お問い合わせ
- カード: `background: --surface`、`border-radius: --radius-lg`、テキスト中央揃え
- ボタン: **アウトライン型**（`border: 1.5px solid --accent`、`color: --accent`）
- ホバーで `background: --accent; color: #fff`

#### 5. フッター
- `background: #0a0a0a`（さらに暗い）、`border-top: 1px solid --border`
- ロゴテキスト（DM Sans 800）＋ リンク（プライバシーポリシー・利用規約）＋コピーライト
- 免責文: "This app is not affiliated with or endorsed by Nintendo or The Pokémon Company."

---

### privacy.html / terms.html のスタイル

- 全体のカラートークン・ヘッダー・フッターはindex.htmlと完全一致
- コンテンツエリア: `max-width: 720px; margin: 0 auto; padding: 48px 24px`
- 見出し: `color: --text; font-family: DM Sans / M PLUS 1p`
- 本文: `color: --text-muted; line-height: 1.8; font-size: 14px`
- h2見出し前に `border-left: 3px solid --accent` アクセント
- リンク: `--accent`色

---

## 多言語対応（既存ロジック維持）

- `[data-lang] { display: none }` / `[data-lang].active { display: block }` のルールは変更しない
- 言語切り替えJSロジックはそのまま保持
- `localStorage`による言語設定の永続化も維持

---

## アセット・フォント読み込み

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;700;800&family=M+PLUS+1p:wght@400;700;800&family=Noto+Sans+SC:wght@400;700&display=swap" rel="stylesheet">
```

- `logo.png` / App Storeバッジ（JP/EN/ZH）は既存パスのまま使用
- App StoreバッジSVGは既存パスのまま使用し、`badge-link` のスタイルのみ更新（ホバーアニメーション・drop-shadow）

---

## 対象外・変更しないもの

- HTMLのセクション構造（`data-lang`属性、FAQ内容、お問い合わせ文、フッター免責文）
- `app-ads.txt` の内容
- JavaScriptの言語切り替えロジック

---

## 成功基準

- [ ] 3ファイルすべてでダークテーマが一貫して適用されている
- [ ] JP / EN / ZH の言語切り替えが正常に動作する
- [ ] ヒーローのスプリットレイアウトがモバイル（375px〜）でも崩れない
- [ ] フィーチャーグリッドは `320px〜479px` では `repeat(2, 1fr)` の2列にフォールバックする
- [ ] `python3 -m http.server 8000` でローカル確認済み
