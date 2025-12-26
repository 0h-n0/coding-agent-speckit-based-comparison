# Quickstart: STT Implementation Comparison

**Feature**: 003-stt-implementation-comparison
**Date**: 2025-12-26

## Overview

このドキュメントはSTT実装比較機能の開発クイックスタートガイドです。

## Prerequisites

- Git (submodule管理)
- MkDocs + Material for MkDocs (ドキュメントビルド)
- Python 3.11+ (MkDocsビルド用)

## Quick Setup

### 1. Submodule更新

```bash
# リポジトリルートで実行
git submodule update --remote --merge
```

### 2. 比較データ収集

各submoduleのSTT関連ファイルを確認:

```bash
# Codex STT仕様書
cat submodules/codex/specs/001-stt-api/spec.md

# Gemini STT仕様書
cat submodules/gemini/specs/002-stt-japanese-api/spec.md

# Claude STT仕様書
cat submodules/claude/specs/002-stt-api/spec.md
```

### 3. 実装コード確認

```bash
# Codex実装
wc -l submodules/codex/backend/app/api/stt.py
cat submodules/codex/backend/app/api/stt.py

# Gemini実装
wc -l submodules/gemini/backend/src/api/v1/endpoints/stt.py
cat submodules/gemini/backend/src/api/v1/endpoints/stt.py

# Claude実装
wc -l submodules/claude/backend/src/api/stt.py
cat submodules/claude/backend/src/api/stt.py
```

### 4. ドキュメント作成

```bash
# メイン比較ページ作成
touch docs/methodology/stt-comparison.md

# エージェント個別ページ作成
touch docs/agents/codex/stt-analysis.md
touch docs/agents/gemini/stt-analysis.md
touch docs/agents/claude/stt-analysis.md
```

### 5. ナビゲーション更新

`mkdocs.yml` に以下を追加:

```yaml
nav:
  - Methodology:
    - STT Comparison: methodology/stt-comparison.md
  - Agents:
    - Codex:
      - STT Analysis: agents/codex/stt-analysis.md
    - Gemini:
      - STT Analysis: agents/gemini/stt-analysis.md
    - Claude:
      - STT Analysis: agents/claude/stt-analysis.md
```

### 6. ビルド検証

```bash
mkdocs build --strict
```

## Key Files

| Path | Description |
|------|-------------|
| `docs/methodology/stt-comparison.md` | メイン比較ページ |
| `docs/agents/codex/stt-analysis.md` | Codex STT詳細分析 |
| `docs/agents/gemini/stt-analysis.md` | Gemini STT詳細分析 |
| `docs/agents/claude/stt-analysis.md` | Claude STT詳細分析 |
| `specs/003-stt-implementation-comparison/research.md` | 比較分析データ |

## Comparison Categories

1. **仕様書比較** (Spec Comparison)
   - 機能要件対照表
   - 成功基準比較
   - ユーザーストーリー比較

2. **実装比較** (Implementation Comparison)
   - ファイル構造
   - APIエンドポイント設計
   - エラーハンドリング
   - WebSocket実装
   - テストカバレッジ

3. **仕様遵守度** (Alignment)
   - 各FRの実装状況
   - 達成率計算

## Next Steps

1. `research.md` の分析データをドキュメントに反映
2. 比較テーブルを作成
3. 仕様遵守度チェックリストを作成
4. MkDocsビルドで検証
