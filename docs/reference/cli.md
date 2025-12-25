# CLI リファレンス

Coding Agent Comparison で使用するコマンドラインインターフェースのリファレンスです。

## MkDocs コマンド

### mkdocs serve

ローカル開発サーバーを起動します。

```bash
mkdocs serve
```

**オプション:**

| オプション | 説明 |
|-----------|------|
| `-a, --dev-addr` | アドレス指定（デフォルト: 127.0.0.1:8000） |
| `--strict` | 警告をエラーとして扱う |
| `--theme` | テーマを指定 |

### mkdocs build

静的サイトを生成します。

```bash
mkdocs build
```

**オプション:**

| オプション | 説明 |
|-----------|------|
| `-c, --clean` | ビルド前にサイトディレクトリをクリーン |
| `-d, --site-dir` | 出力ディレクトリを指定 |
| `--strict` | 警告をエラーとして扱う |

### mkdocs gh-deploy

GitHub Pages にデプロイします。

```bash
mkdocs gh-deploy
```

**オプション:**

| オプション | 説明 |
|-----------|------|
| `-b, --remote-branch` | リモートブランチを指定 |
| `-r, --remote-name` | リモート名を指定 |
| `--force` | 強制プッシュ |

## Git サブモジュールコマンド

### サブモジュールの初期化

```bash
git submodule update --init --recursive
```

### サブモジュールの更新

```bash
git submodule update --remote --merge
```

## 関連ドキュメント

- [API リファレンス](api.md)
- [用語集](glossary.md)
