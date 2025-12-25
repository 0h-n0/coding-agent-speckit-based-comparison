# Coding Agent Comparison

コーディングエージェントを比較・評価するためのドキュメントサイトです。

## 概要

Coding Agent Comparison は、異なるAIコーディングエージェントが生成する
ドキュメントの品質を比較・評価するためのフレームワークです。

### プロジェクトの目的

- 複数のコーディングエージェントを公平に比較
- 透明性のある評価方法論を提供
- 再現可能な比較結果を実現
- 自動化されたワークフローで効率的に評価

## 対応エージェント

| エージェント | 提供元 | 説明 |
|-------------|--------|------|
| [Codex](agents/codex/index.md) | OpenAI | コード生成に特化したモデル |
| [Gemini](agents/gemini/index.md) | Google | マルチモーダルAIモデル |
| [Claude](agents/claude/index.md) | Anthropic | 会話型AIアシスタント |

## はじめに

1. [インストール](getting-started/installation.md) - セットアップ手順
2. [設定](getting-started/configuration.md) - 設定オプション
3. [クイックスタート](getting-started/quickstart.md) - 最初のステップ

## 主な機能

- **拡張性**: 新しいエージェントやテストケースを簡単に追加
- **透明性**: 評価方法論とメトリクスを完全に公開
- **再現性**: 同じ入力で同じ結果を得られる
- **自動化**: CI/CDパイプラインとの統合

## ドキュメント構成

- [機能](features/index.md) - 機能の概要
- [エージェント](agents/index.md) - 各エージェントの詳細
- [方法論](methodology/evaluation-criteria.md) - 評価基準
- [開発](development/contributing.md) - 貢献ガイド
- [リファレンス](reference/cli.md) - CLIとAPIリファレンス
