# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

新潟西歯科クリニック矯正歯科サイトの WordPress → Astro 移行プロジェクト。
GitHub Pages で静的サイトとしてホスティングする。

**移行元**: WordPress (Xserver: uwls01.xsrv.jp/tem/)
**移行先**: Astro SSG → GitHub Pages

※ 資格情報（WPログイン、サーバー情報等）は Claude Code のメモリに保存済み。CLAUDE.md には記載しない。

## Build Commands

```bash
npm install          # 依存パッケージのインストール
npm run dev          # 開発サーバー起動 (localhost:4321)
npm run build        # 本番ビルド (出力: ./dist)
npm run preview      # 本番ビルドのプレビュー
```

## Architecture

```
src/
  pages/           # Astro ページ (.astro) - ファイルベースルーティング
  layouts/         # ページレイアウト (BaseLayout 等)
  components/      # 再利用可能な UI コンポーネント
  styles/          # グローバル CSS
public/
  images/          # WordPress から移行した画像
  favicons/        # ファビコン一式
astro.config.mjs   # Astro 設定 (site, base, output: 'static')
```

## WordPress Site Structure (移行元)

サイト名: 新潟西歯科クリニック（矯正歯科専門サイト）
テーマ: uwltpl（カスタムテーマ）
投稿: なし（固定ページのみのサイト）

### 固定ページ一覧 (14ページ)

| slug | 内容 | ナビ表示 |
|------|------|---------|
| home | トップページ（スライダー付き） | - |
| about | 矯正歯科について | ○ |
| orthodoctor | 矯正担当医 | ○ |
| flow | 治療の流れ | ○ |
| adult | 大人の矯正 | ○ |
| children | 子どもの矯正 | ○ |
| price | 費用案内 | ○ |
| access | アクセス | ○ |
| susumeru | 矯正歯科を薦める理由 | - |
| kamiawase | 噛み合わせの大切さ | - |
| waruihanarabi | 悪い歯並びで起こること | - |
| bracketindex | 選べるブラケット（矯正装置） | - |
| test | テストページ（移行不要） | - |
| partial | 部分矯正 | - |

### デザイン・技術要素

- **フォント**: Kosugi Maru (Google Fonts)
- **アイコン**: Font Awesome 5
- **スライダー**: uw-slider（カスタム実装）
- **遅延読み込み**: lazyload パターン（data-src / data-srcset）
- **レスポンシブ**: flexbox ベース、独自ブレークポイント (md, sm, xl)
- **Google Analytics**: UA-182523702-1, G-RZX3603KQL

### 外部リンク先

- 新潟西歯科クリニック本院: https://www.niigatanishi-dc.jp/
- 予約・お問い合わせ: https://www.niigatanishi-dc.jp/contact/

## WP REST API

コンテンツ取得用の REST API エンドポイント（Basic Auth: demo/demo が必要）:

```
GET /tem/wp-json/wp/v2/pages?per_page=100    # 全固定ページ
GET /tem/wp-json/wp/v2/pages/{id}            # 個別ページ（rendered HTML含む）
GET /tem/wp-json/wp/v2/media?per_page=100    # メディア一覧
```

## Deployment (GitHub Pages)

`astro.config.mjs` に以下を設定:
- `site`: GitHub Pages の URL
- `base`: リポジトリ名（カスタムドメイン未使用時）
- `output: 'static'`

GitHub Actions でビルド＆デプロイを自動化する。

## Migration Checklist

1. WordPress の各ページコンテンツを REST API で取得
2. HTML を Astro コンポーネント (.astro) に変換
3. 画像をダウンロードして public/images/ に配置
4. カスタムテーマの CSS を再現（styles/ に整理）
5. ナビゲーション・ヘッダー・フッターをレイアウトコンポーネント化
6. スライダー機能の実装
7. レスポンシブデザインの再現
8. GitHub Actions デプロイ設定
