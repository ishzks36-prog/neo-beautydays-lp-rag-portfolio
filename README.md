# 記事LP 分析レンズ・ポートフォリオ v2

女性向けAGAクリニックの記事LP（neo-beautydays）をAIで157チャンクに分解し、
**5つの分析レンズ**で意思決定に必要な断面を可視化した分析ポートフォリオ。

## 🔗 ライブデモ

- URL: https://ishzks36-prog.github.io/neo-beautydays-lp-rag-portfolio/
- Password: `neoragu-4242`
- 有効期間: 無期限（運用中に変更の場合あり）

## v1 → v2 の変更点

- **CTA判定を本質化**: 従来の text-embedding ベース（cta=48件と過剰分類）から、**DOM上の `<a href>` + ボタン形状** に限定（cta=4件 = 真のCTAのみ）
- **タブ構成を再設計**: 「ファネル5段階」ではなく **「5つの分析レンズ」** — 全体 / コピー / ビジュアル / エビデンス / CTA
- **パスワードゲート追加**: 公開URL即不特定閲覧を防ぐ SHA-256 クライアントサイド認証

## 仕組み

```
記事LP (HTML)
  ↓ DOM順ウォーク
157 ブロック型チャンク (image / text / video)
  ↓ Gemini Embedding 2 (3072D)
ベクトル化
  ↓ 20 販売ライティングタグ分類 + 5軸スコアリング
RAG (db.json + embeddings.npy + BM25)
  ↓ ファネル 5段階クラスタリング
  ↓ Gemini 2.5 Pro によるセクション解釈
ポートフォリオ HTML（このサイト）
```

## 5つの分析レンズ

LPを「ファネル構造」で分けるのではなく、「マーケが見たい断面」ごとに再構成:

1. **📊 全体プロファイル** — LPの基本スペック・構成・スコアを俯瞰
2. **💎 勝ちコピー分析** — 高スコア表現に現れる型・語彙パターン
3. **🖼 ビジュアル戦略** — 画像・動画の機能タイプ（ビフォーアフター/バッジ/人物/施術/CTA）
4. **🔢 エビデンス・権威マップ** — 数値・実績・バッジ・権威語の集約
5. **🎯 CTA動線分析** — `<a href>`付きボタン形式のCTAのみを対象とした配置分析

各レンズに以下を収録:

- 📌 **このレンズが教えてくれること** — Gemini 2.5 Pro による本質要約
- 👤 **N1 インサイト** — 「誰の心が動くか / 心の声 / なぜ刺さるか」
- 💎 **勝ちパターン** — このレンズ視点での動作可能な勝ちの型 × 3
- ⚡ **改善施策** — 追加/再設計/削除の具体提案 × 3
- 💡 **レンズ固有の気づき** — 他では見えない1つのシグナル

## スコアリング (5軸 + 総合)

| 軸 | 重み | 内容 |
|---|---|---|
| role_strength | 0.35 | タグ centroid への cosine類似度（検索可能性） |
| funnel_position | 0.25 | LP内位置の V字カーブ（FV/CTA重視） |
| evidence_density | 0.20 | 数値・固有名・引用の密度 |
| uniqueness | 0.20 | 1 − 同LP他チャンク平均類似度 |
| block_multiplier | × | video 1.15 / 画像+OCR 1.10 / text 1.00 |

## Credits

- AI: Gemini Embedding 2 (gemini-embedding-2-preview, 3072D), Gemini 2.5 Pro, Gemini 2.5 Flash (OCR)
- Orchestrator: Claude Opus 4.7
- Analyzed LP: https://neo-beautydays.com/ab/WYIeoqPmdkIDETflxgg

## ライセンス

本ポートフォリオは分析・学習目的。分析対象LPの著作権は各原著作権者に帰属します。
