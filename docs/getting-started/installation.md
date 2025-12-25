# インストール

このページでは、Coding Agent Comparison プロジェクトのセットアップ方法を説明します。

## 前提条件

- Python 3.8 以上
- pip (Python パッケージマネージャー)
- Git

## インストール手順

### 1. リポジトリのクローン

```bash
git clone https://github.com/0h-n0/coding-agent-speckit-based-comparison.git
cd coding-agent-speckit-based-comparison
```

### 2. サブモジュールの初期化

```bash
git submodule update --init --recursive
```

### 3. Python 仮想環境の作成（推奨）

```bash
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# または
.venv\Scripts\activate  # Windows
```

### 4. 依存関係のインストール

```bash
pip install mkdocs-material
```

## ローカルでのドキュメント確認

インストールが完了したら、以下のコマンドでローカルサーバーを起動できます：

```bash
mkdocs serve
```

ブラウザで `http://127.0.0.1:8000` にアクセスしてドキュメントを確認してください。

## 次のステップ

- [設定](configuration.md) - プロジェクトの設定オプション
- [クイックスタート](quickstart.md) - 最初のステップ
