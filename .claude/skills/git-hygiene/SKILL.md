---
name: git-hygiene
description: Git運用ルール、.gitignore管理、秘密情報の防止、PR前チェック手順
---

# Git Hygiene Skill

## .gitignore 管理方針

### 除外すべきもの
- 仮想環境: `.venv/`, `venv/`, `node_modules/`
- ビルド成果物: `dist/`, `build/`, `__pycache__/`
- IDE設定: `.idea/` (ただし `.vscode/` は共有設定として残す場合あり)
- OS生成ファイル: `.DS_Store`, `Thumbs.db`
- 秘密情報: `.env`, `*.pem`, `*.key`, `credentials.json`
- 個人設定: `CLAUDE.local.md`

### 追跡済みファイルを除外する手順
既にgit管理されているファイルを `.gitignore` に追加しても無視されない。以下で対処：

```bash
# 1. .gitignore に追記
echo "unwanted_file.txt" >> .gitignore

# 2. git のインデックスから削除（ファイル自体は残る）
git rm --cached unwanted_file.txt

# 3. コミット
git commit -m "Remove unwanted_file.txt from tracking"
```

ディレクトリの場合:
```bash
git rm -r --cached directory_name/
```

## 秘密情報の禁止

### 絶対にコミットしてはいけないもの
- APIキー、トークン
- パスワード、認証情報
- 秘密鍵 (`.pem`, `.key`)
- 環境変数ファイル (`.env`)

### 代替手段
```bash
# 環境変数で管理
export API_KEY="your-key"

# .env.example をコミット（値は空またはダミー）
API_KEY=your-api-key-here
DATABASE_URL=postgres://user:pass@localhost/db
```

## コミット前チェック

```bash
# 1. 変更内容を確認
git status
git diff

# 2. 秘密情報が含まれていないか確認
git diff --cached | grep -i -E "(api_key|password|secret|token)"

# 3. 意図しないファイルがないか確認
git diff --cached --name-only
```

## PR前チェックリスト

- [ ] `git status` でコミット漏れがないか
- [ ] `.gitignore` で秘密情報が除外されているか
- [ ] 大きなバイナリファイルがないか
- [ ] デバッグ用コード（print文、console.log）が残っていないか
- [ ] テストが通るか（該当する場合）

## 禁止されたgit操作

以下は明示的な許可なく実行しない：
- `git push --force` (履歴破壊)
- `git reset --hard` (変更消失)
- `git clean -fd` (untracked削除)
- `git rebase` on shared branches
