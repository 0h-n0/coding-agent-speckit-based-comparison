# Claude

Anthropic が提供する会話型 AI アシスタントです。

## 概要

Claude は Anthropic が開発した AI アシスタントで、
安全性と有用性のバランスを重視して設計されています。

## 特徴

| 項目 | 説明 |
|-----|------|
| 提供元 | Anthropic |
| 設計思想 | 安全性重視 |
| 特化領域 | 会話・分析 |
| コンテキスト | 長いコンテキストウィンドウ |

## 強み

- **長いコンテキスト**: 大量のテキストを一度に処理可能
- **詳細な説明**: 丁寧で詳細な回答を生成
- **コード理解**: 既存コードの分析・説明

## 制限事項

- 一部の地域でサービス利用に制限あり
- リアルタイム情報へのアクセスに制限

## 評価結果

このプロジェクトでの Claude の評価結果は [ベンチマーク](../../methodology/benchmarks.md) を参照してください。

## サブモジュール

Claude の実装詳細は `submodules/claude/` にあります。

```bash
cd submodules/claude
cat .specify/memory/constitution.md
```

## 関連リンク

- [Anthropic Claude](https://www.anthropic.com/claude)
- [Claude API](https://docs.anthropic.com/)
