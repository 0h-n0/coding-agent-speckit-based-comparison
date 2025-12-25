# Quickstart: MkDocs GitHub Pages Documentation Site

**Date**: 2025-12-25
**Feature**: 001-mkdocs-github-pages

## Prerequisites

- Python 3.11以上がインストールされていること
- pipが利用可能であること
- GitHubリポジトリへのプッシュ権限があること

## Quick Setup (5分)

### 1. 依存関係のインストール

```bash
pip install mkdocs-material
```

### 2. ローカルでドキュメントを確認

```bash
# プロジェクトルートで実行
mkdocs serve
```

ブラウザで http://127.0.0.1:8000 を開く

### 3. ドキュメントの編集

`docs/` ディレクトリ内のマークダウンファイルを編集:

```bash
# 例: ホームページを編集
vim docs/index.md
```

変更は自動的にブラウザに反映される（ホットリロード）

### 4. 手動デプロイ（初回のみ）

```bash
mkdocs gh-deploy
```

### 5. GitHub Pagesの有効化

1. GitHubリポジトリの Settings → Pages に移動
2. Source を "Deploy from a branch" に設定
3. Branch を "gh-pages" / "(root)" に設定
4. Save をクリック

## 自動デプロイの確認

mainブランチにプッシュすると、GitHub Actionsが自動的にデプロイを実行:

1. `.github/workflows/docs.yml` がトリガーされる
2. ドキュメントがビルドされる
3. gh-pagesブランチにデプロイされる
4. GitHub Pagesが自動更新される

## ドキュメントの追加

### 新しいページを追加

1. `docs/` 配下に新しい `.md` ファイルを作成
2. `mkdocs.yml` の `nav` セクションにエントリを追加
3. コミットしてプッシュ

### 例: 新しいエージェントを追加

```bash
# 1. ディレクトリとファイルを作成
mkdir -p docs/agents/new-agent
cat > docs/agents/new-agent/index.md << 'EOF'
# New Agent

新しいエージェントのドキュメント。
EOF

# 2. mkdocs.yml を編集して nav に追加
# nav:
#   - Agents:
#     - New Agent: agents/new-agent/index.md

# 3. コミットしてプッシュ
git add docs/agents/new-agent/ mkdocs.yml
git commit -m "docs: add new agent documentation"
git push
```

## トラブルシューティング

### ビルドエラー

```bash
# ビルドを実行してエラーを確認
mkdocs build --strict
```

### リンク切れ

```bash
# 開発サーバーでリンク切れ警告を確認
mkdocs serve --strict
```

### GitHub Pages が更新されない

1. Actions タブでワークフローの実行状況を確認
2. gh-pages ブランチが存在することを確認
3. Settings → Pages で正しいブランチが設定されているか確認

## 次のステップ

- [ ] `docs/index.md` をプロジェクト概要で更新
- [ ] 各セクションのプレースホルダーを実際のコンテンツで置き換え
- [ ] サブモジュールのドキュメントを `docs/agents/` に反映
