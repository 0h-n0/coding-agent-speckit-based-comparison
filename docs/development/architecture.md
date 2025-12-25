# アーキテクチャ

Coding Agent Comparison プロジェクトのアーキテクチャについて説明します。

## プロジェクト構造

```
coding-agent-comparison/
├── docs/                    # ドキュメント（MkDocs）
│   ├── index.md
│   ├── getting-started/
│   ├── features/
│   ├── agents/
│   │   ├── codex/
│   │   ├── gemini/
│   │   └── claude/
│   ├── methodology/
│   ├── development/
│   └── reference/
├── submodules/              # エージェント実装（Git サブモジュール）
│   ├── codex/
│   ├── gemini/
│   └── claude/
├── specs/                   # 機能仕様
│   └── 001-mkdocs-github-pages/
├── .github/
│   └── workflows/           # GitHub Actions
├── .specify/                # Speckit 設定
│   └── memory/
├── mkdocs.yml              # MkDocs 設定
└── .gitignore
```

## コンポーネント

### ドキュメントサイト

- **技術スタック**: MkDocs + Material for MkDocs
- **ホスティング**: GitHub Pages
- **デプロイ**: GitHub Actions

### エージェント実装

各エージェントは独立した Git サブモジュールとして管理：

```
submodules/{agent}/
├── .specify/
│   └── memory/
│       └── constitution.md
└── (agent-specific files)
```

### 仕様管理

Speckit ベースの仕様管理：

```
specs/{feature}/
├── spec.md           # 機能仕様
├── plan.md           # 実装計画
├── tasks.md          # タスク一覧
├── research.md       # 調査結果
├── data-model.md     # データモデル
└── contracts/        # API 契約
```

## データフロー

```
入力
  ↓
エージェント実行
  ↓
出力収集
  ↓
評価処理
  ↓
レポート生成
```

## 技術選定

| 領域 | 技術 | 理由 |
|-----|------|------|
| ドキュメント | MkDocs | Markdown ベース、テーマ充実 |
| テーマ | Material | 機能豊富、日本語対応 |
| CI/CD | GitHub Actions | GitHub 統合、無料枠 |
| バージョン管理 | Git Submodules | 独立した管理が可能 |

## 関連ドキュメント

- [貢献ガイド](contributing.md)
- [変更履歴](changelog.md)
