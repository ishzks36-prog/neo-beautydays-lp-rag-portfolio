# 記事LP RAG ポートフォリオ

女性向けAGAクリニックの記事LP（neo-beautydays）をAIで157チャンクに分解し、
ファネル5段階に沿って「訴求・N1・売れる表現・改善施策」を可視化した分析ポートフォリオ。

## 🔗 ライブデモ

GitHub Pages で公開中（ビルド完了後に URL が有効化されます）。

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

## 5つのファネルタブ

1. **🎯 問題提起 & 共感** (Awareness)
2. **🔬 原因教育 & 認知転換** (Understanding)
3. **💊 解決策 & 作用機序** (Consideration)
4. **✅ 証拠 & 権威** (Trust)
5. **🚀 安心 & 行動喚起** (Conversion)

各タブに以下を収録:

- 📌 **訴求の核** — このセクションが伝えたい中心メッセージ
- 👤 **N1 ペルソナ** — 年齢 / 状況 / 心の声 / 欲求
- 💎 **売れる表現 Top 3** — 総合スコア上位のコピー
- 🖼 **ビジュアル訴求のハイライト** — スコアの高い画像
- 🔢 **数値エビデンス** — 抽出された実績 / %
- ⚡ **改善施策** — 不足要素・投入余地

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
