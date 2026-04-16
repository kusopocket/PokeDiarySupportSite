# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

PokeDiary（ポケモンカードゲームの対戦記録アプリ）のサポートサイト。静的HTMLサイトで、ビルドシステムは不要。

## ローカルでの起動

```bash
cd support-site
python3 -m http.server 8000
# → http://localhost:8000 にアクセス
```

## アーキテクチャ

**ファイル構成:**
- `support-site/index.html` — メインのランディングページ（機能紹介・App Storeリンク）
- `support-site/privacy.html` — プライバシーポリシー
- `support-site/terms.html` — 利用規約
- `support-site/app-ads.txt` — Google AdMob設定（Publisher IDのプレースホルダーあり）
- `support-site/assets/` — ロゴ画像・App Storeバッジ（JP/US-UK/中国向け）
- `docs/superpowers/specs/` — デザインスペック
- `docs/superpowers/plans/` — 実装プラン

**技術スタック:** 純粋なHTML5 + CSS3 + バニラJS（フレームワークなし）

**多言語対応:** 日本語・英語・中国語（簡体字）の3言語を各HTMLファイル内に `data-lang` 属性で埋め込み。言語設定は `localStorage`（キー: `poke-lang`）で永続化。

## デザインシステム

**テーマ:** Spotify風ダーク（2026年リデザイン）

**カラートークン（CSS変数）:**
| 変数 | 値 | 用途 |
|------|----|------|
| `--bg` | `#121212` | ページ背景 |
| `--surface` | `#181818` | カード背景 |
| `--surface-2` | `#282828` | カード内要素 |
| `--surface-3` | `#383838` | ホバー状態など |
| `--accent` | `#E8344A` | Pokemon Red（CTAボタン・アクセント） |
| `--accent-dark` | `#C9283E` | アクセントホバー色 |
| `--text` | `#FFFFFF` | 本文 |
| `--text-muted` | `#B3B3B3` | サブテキスト |
| `--text-faint` | `#555555` | 非アクティブ要素 |
| `--border` | `rgba(255,255,255,0.06)` | ボーダー |

**フォント（Google Fonts）:**
- 日本語: `M PLUS 1p` (wght 400/700/800)
- 英語: `DM Sans` (wght 400/700/800)
- 中国語: `Noto Sans SC` (wght 400/700)

**主要コンポーネント:**
- `.hero-inner` — スプリットヒーロー（左: テキスト+CTA / 右: フォンモックアップ）
- `.features-grid` — アルバム風3×2グリッド（最初タイルは `.highlight` クラスで赤背景）
- `.feature-tile` / `.feature-label` — グリッドタイル（ラベルは `display: inline` ルール必須）
- `.section-header` — セクションタイトル + グラデーションライン
- `header` — `backdrop-filter: blur(12px)` の半透明ヘッダー
- `.footer-logo` — フッター先頭のブランドロゴ（accent色）

**レスポンシブ:**
- `@media (max-width: 600px)` — `.hero-inner` を縦並び（`flex-direction: column`）
- `@media (max-width: 479px)` — `.features-grid` を2カラムに縮小

**多言語 CSS ルール:**
```css
[data-lang] { display: none; }
[data-lang].active { display: block; }
.feature-label [data-lang].active { display: inline; } /* インライン要素用 */
```

## 注意事項

- `app-ads.txt` のAdMob Publisher IDは `pub-XXXXXXXXXXXXXXXX` のプレースホルダーのまま。本番公開前に実際のIDへ置き換えること。
- 全ページで共通のヘッダー/フッター構造を持つが、共通コンポーネント化はされていない（各HTMLに直書き）。変更時は3ファイル全て更新すること。
- デザイントークン（CSS変数）はすべての `.html` ファイルにそれぞれ `:root` ブロックとして記述されている。変更時は3ファイル全て同期すること。
