# Gemini

Google が提供するマルチモーダル AI モデルです。

## 概要

Gemini は Google DeepMind が開発したマルチモーダル AI モデルで、
テキスト、画像、コードなど複数のモダリティを統合的に処理できます。

## 特徴

| 項目 | 説明 |
|-----|------|
| 提供元 | Google |
| アーキテクチャ | Transformer ベース |
| 特化領域 | マルチモーダル |
| バリエーション | Ultra, Pro, Nano |

## 強み

- **マルチモーダル**: テキスト、画像、コードの統合処理
- **推論能力**: 複雑な推論タスクへの対応
- **スケーラビリティ**: 複数のサイズバリエーション

## 制限事項

- 利用可能なリージョンやサービスに制限がある場合あり
- API レート制限への考慮が必要

## 評価結果

このプロジェクトでの Gemini の評価結果は [ベンチマーク](../../methodology/benchmarks.md) を参照してください。

## サブモジュール

Gemini の実装詳細は `submodules/gemini/` にあります。

```bash
cd submodules/gemini
cat .specify/memory/constitution.md
```

## 関連リンク

- [Google Gemini](https://deepmind.google/technologies/gemini/)
- [Gemini API](https://ai.google.dev/)
