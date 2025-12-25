# Codex

OpenAI が提供するコード生成に特化した AI モデルです。

## 概要

Codex は GPT アーキテクチャをベースに、コード生成タスクに最適化されたモデルです。
GitHub Copilot の基盤技術としても使用されています。

## 特徴

| 項目 | 説明 |
|-----|------|
| 提供元 | OpenAI |
| ベースモデル | GPT |
| 特化領域 | コード生成 |
| 対応言語 | 多数のプログラミング言語 |

## 強み

- **コード補完精度**: 高精度なコード補完
- **多言語サポート**: Python, JavaScript, TypeScript, Go など多数対応
- **コンテキスト理解**: 既存コードベースの理解

## 制限事項

- 最新の API や ライブラリへの対応に遅れがある場合あり
- 長いコンテキストでの整合性維持が課題になることも

## 評価結果

このプロジェクトでの Codex の評価結果は [ベンチマーク](../../methodology/benchmarks.md) を参照してください。

## サブモジュール

Codex の実装詳細は `submodules/codex/` にあります。

```bash
cd submodules/codex
cat .specify/memory/constitution.md
```

## 関連リンク

- [OpenAI Codex](https://openai.com/blog/openai-codex)
- [GitHub Copilot](https://github.com/features/copilot)
