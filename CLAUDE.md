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

**技術スタック:** 純粋なHTML5 + CSS3 + バニラJS（フレームワークなし）

**多言語対応:** 日本語・英語・中国語（簡体字）の3言語を各HTMLファイル内にdata-lang属性で埋め込み。言語設定はlocalStorageで永続化。

**スタイリング:** CSS変数（カスタムプロパティ）でカラーテーマを管理。ポケモンイメージのレッド・イエロー・ブルー系配色。レスポンシブデザイン（Flexbox + CSS Grid）。

## 注意事項

- `app-ads.txt` のAdMob Publisher IDは `pub-XXXXXXXXXXXXXXXX` のプレースホルダーのまま。本番公開前に実際のIDへ置き換えること。
- 全ページで共通のヘッダー/フッター構造を持つが、共通コンポーネント化はされていない（各HTMLに直書き）。変更時は3ファイル全て更新すること。
