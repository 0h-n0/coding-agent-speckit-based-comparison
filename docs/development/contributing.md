# 貢献ガイド

Coding Agent Comparison プロジェクトへの貢献方法について説明します。

## 貢献の方法

### Issue の作成

- バグ報告
- 機能リクエスト
- ドキュメント改善提案

### Pull Request

1. リポジトリをフォーク
2. フィーチャーブランチを作成
3. 変更を実装
4. テストを実行
5. Pull Request を作成

## 開発ワークフロー

### ブランチ戦略

```
main
├── feature/xxx    # 新機能
├── fix/xxx        # バグ修正
└── docs/xxx       # ドキュメント
```

### コミットメッセージ

Conventional Commits 形式を使用：

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Type:**
- `feat`: 新機能
- `fix`: バグ修正
- `docs`: ドキュメント
- `style`: フォーマット
- `refactor`: リファクタリング
- `test`: テスト
- `chore`: その他

## コードスタイル

### Python

- PEP 8 準拠
- Black でフォーマット
- isort でインポート整理

### Markdown

- 日本語コンテンツは日本語で
- コードブロックには言語指定

## レビュープロセス

1. CI チェックのパス
2. コードレビュー
3. 承認後マージ

## 質問・サポート

- GitHub Issues で質問
- Discussion で議論

## 関連ドキュメント

- [アーキテクチャ](architecture.md)
- [変更履歴](changelog.md)
