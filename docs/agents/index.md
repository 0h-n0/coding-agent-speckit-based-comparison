# エージェント概要

このセクションでは、比較対象となる AI コーディングエージェントについて説明します。

## 対応エージェント

| エージェント | 提供元 | 特徴 |
|-------------|--------|------|
| [Codex](codex/index.md) | OpenAI | コード生成に特化 |
| [Gemini](gemini/index.md) | Google | マルチモーダル対応 |
| [Claude](claude/index.md) | Anthropic | 会話型アシスタント |

## エージェントの特性

### コード生成能力

各エージェントは異なるアプローチでコードを生成します：

- **Codex**: GPT ベースのコード特化モデル
- **Gemini**: マルチモーダル入力をサポート
- **Claude**: 長いコンテキストウィンドウと詳細な説明

### ドキュメント生成スタイル

各エージェントのドキュメント生成スタイルを比較します：

```
Codex   → 簡潔・技術的
Gemini  → バランス型
Claude  → 詳細・説明的
```

## 評価フレームワーク

各エージェントは以下の観点から評価されます：

1. **正確性** - 生成されたコード/ドキュメントの正確さ
2. **完全性** - 要件の網羅度
3. **可読性** - 読みやすさ・理解しやすさ
4. **一貫性** - スタイルの統一性

## サブモジュール構成

各エージェントの実装は Git サブモジュールとして管理されています：

```
submodules/
├── codex/    # OpenAI Codex 実装
├── gemini/   # Google Gemini 実装
└── claude/   # Anthropic Claude 実装
```

## 詳細ドキュメント

- [Codex](codex/index.md) - OpenAI Codex の詳細
- [Gemini](gemini/index.md) - Google Gemini の詳細
- [Claude](claude/index.md) - Anthropic Claude の詳細
