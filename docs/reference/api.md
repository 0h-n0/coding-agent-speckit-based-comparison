# API リファレンス

!!! note "準備中"
    API は将来のバージョンで提供予定です。

## 概要

Coding Agent Comparison API は、プログラムから評価を実行するためのインターフェースを提供します。

## 予定されている機能

### 評価 API

```python
# 将来の API 例
from coding_agent_comparison import evaluate

result = evaluate(
    agent="codex",
    task="generate_api_client",
    config={"language": "python"}
)

print(result.score)
print(result.metrics)
```

### レポート API

```python
# 将来の API 例
from coding_agent_comparison import report

report.generate(
    results=[result1, result2, result3],
    format="markdown",
    output="report.md"
)
```

## エンドポイント（予定）

| メソッド | エンドポイント | 説明 |
|---------|---------------|------|
| POST | /api/evaluate | 評価を実行 |
| GET | /api/results/{id} | 結果を取得 |
| GET | /api/agents | エージェント一覧 |

## 認証（予定）

API キーによる認証を予定しています。

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     https://api.example.com/api/evaluate
```

## 関連ドキュメント

- [CLI リファレンス](cli.md)
- [用語集](glossary.md)
