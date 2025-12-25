# 設定

このページでは、Coding Agent Comparison プロジェクトの設定オプションについて説明します。

## MkDocs 設定

プロジェクトのドキュメント設定は `mkdocs.yml` ファイルで管理されています。

### 基本設定

```yaml
site_name: Coding Agent Comparison
site_url: https://0h-n0.github.io/coding-agent-speckit-based-comparison/
site_description: Documentation for comparing coding agents
```

### テーマ設定

Material for MkDocs テーマを使用しています：

```yaml
theme:
  name: material
  language: ja
  features:
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - search.suggest
    - search.highlight
    - content.code.copy
```

### カラーパレット

ライトモードとダークモードの切り替えをサポートしています：

```yaml
palette:
  - scheme: default
    primary: indigo
    accent: indigo
    toggle:
      icon: material/brightness-7
      name: Switch to dark mode
  - scheme: slate
    primary: indigo
    accent: indigo
    toggle:
      icon: material/brightness-4
      name: Switch to light mode
```

## サブモジュール設定

各エージェントの実装は Git サブモジュールとして管理されています：

| サブモジュール | パス | 説明 |
|---------------|------|------|
| Codex | `submodules/codex/` | OpenAI Codex 実装 |
| Gemini | `submodules/gemini/` | Google Gemini 実装 |
| Claude | `submodules/claude/` | Anthropic Claude 実装 |

### サブモジュールの更新

```bash
git submodule update --remote --merge
```

## 環境変数

現在、必須の環境変数はありません。将来的に API キーなどが必要になる場合は、
`.env` ファイルを使用して管理します。

## 次のステップ

- [クイックスタート](quickstart.md) - 最初のステップ
- [機能](../features/index.md) - 機能の概要
