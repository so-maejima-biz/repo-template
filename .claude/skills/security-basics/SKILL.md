---
name: security-basics
description: 機密情報の取り扱い、認証情報管理、最小権限の原則、依存関係のセキュリティ
---

# Security Basics Skill

## 機密情報の取り扱い

### コードに書いてはいけないもの
- APIキー、シークレットトークン
- パスワード、認証情報
- データベース接続文字列（認証情報含む）
- 秘密鍵、証明書
- 内部URL、IPアドレス

### 正しい管理方法

#### 環境変数を使用
```python
import os

# Good
api_key = os.environ.get("API_KEY")
if not api_key:
    raise ValueError("API_KEY environment variable is required")

# Bad
api_key = "sk-1234567890abcdef"  # Never do this
```

#### .env ファイル（ローカル開発用）
```bash
# .env (git管理外)
API_KEY=your-actual-key
DATABASE_URL=postgres://user:pass@localhost/db

# .env.example (git管理対象、値はダミー)
API_KEY=your-api-key-here
DATABASE_URL=postgres://user:pass@host/db
```

Python で読み込み:
```python
from dotenv import load_dotenv
load_dotenv()
```

## ログに出力してはいけない情報

```python
# Bad
print(f"Connecting with password: {password}")
logger.info(f"API Key: {api_key}")

# Good
print("Connecting to database...")
logger.info("API request successful")

# マスキングする場合
logger.debug(f"Using API key: {api_key[:4]}****")
```

## 最小権限の原則

### API キーのスコープ
- 必要な権限のみを持つキーを作成
- 読み取り専用で済むなら書き込み権限は不要
- 本番/開発で別のキーを使用

### ファイル権限
```bash
# 秘密鍵は所有者のみ読み取り可能に
chmod 600 private_key.pem

# Windows の場合は ACL で設定
```

### データベース接続
- アプリケーションユーザーには必要最小限の権限
- DROP, TRUNCATE は通常不要
- 本番DBへの直接アクセスは避ける

## 依存関係のセキュリティ

### 定期的な更新
```bash
# 脆弱性チェック
pip install safety
safety check

# npm の場合
npm audit
```

### バージョン固定
```
# requirements.txt
requests==2.31.0  # バージョン固定推奨

# 範囲指定は脆弱性混入のリスク
requests>=2.0  # 避ける
```

### 信頼できるソースのみ
- 公式パッケージリポジトリ (PyPI, npm) を使用
- プライベートリポジトリは明示的に設定

## チェックリスト

### コードレビュー時
- [ ] ハードコードされた秘密情報がないか
- [ ] ログに機密情報が出力されていないか
- [ ] 環境変数が適切に使用されているか
- [ ] エラーメッセージに内部情報が含まれていないか

### デプロイ前
- [ ] `.env` が `.gitignore` に含まれているか
- [ ] 本番用の認証情報が開発環境と分離されているか
- [ ] 依存関係に既知の脆弱性がないか

## インシデント発生時

秘密情報を誤ってコミットした場合：
1. **即座に** キー/パスワードを無効化・ローテーション
2. git履歴から削除（`git filter-branch` または BFG Repo-Cleaner）
3. force push（チームに事前連絡）
4. 影響範囲の調査

**注意**: git履歴からの削除は複雑。まずキーの無効化を優先。
