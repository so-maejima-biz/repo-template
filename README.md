# プロジェクトテンプレート

AI開発フレンドリーな新規プロジェクト用テンプレート。

## 設計思想

**「AIへの指示」と「人間向けドキュメント」を分離する**

| 用途 | 置き場所 |
|------|----------|
| AIへの指示・手順 | `.claude/skills/` |
| 人間向け参考資料 | `docs/` |
| プロジェクト仕様 | `docs/` |

### ポイント
- **CLAUDE.md は短く**: 50-60行を目安に、最重要ルールのみ
- **詳細は Skills へ**: 必要なときだけ参照される
- **Skills は軽量に**: Claude は一般的な原則を既に知っている。プロジェクト固有の適用方法のみ記載
- **構成は動的確認**: 静的なツリー図より `tree` コマンドで最新を確認
- **トリガーを明記**: メタスキルやワークフローには発動条件を description に記載

## クイックスタート

### 1. このテンプレートから新規リポジトリを作成
GitHub で "Use this template" → "Create a new repository"

### 2. クローンして初期設定
```bash
git clone https://github.com/YOUR_USER/YOUR_REPO.git
cd YOUR_REPO
```

### 3. Pythonプロジェクトの場合
```powershell
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt  # あれば
```

## 構成

現在の構成を確認:
```bash
tree -I "node_modules|.venv|__pycache__|.git"
```

### 主要ファイル
| ファイル | 役割 |
|----------|------|
| `CLAUDE.md` | AI開発ガイドライン（必読） |
| `CLAUDE.local.md` | 個人用メモ（git管理外） |
| `.claude/skills/` | 詳細手順書 |

## Claude Code利用者向け

- `CLAUDE.md` がプロジェクトルールの起点
- 詳細な手順は `.claude/skills/` を参照
- 個人設定は `CLAUDE.local.md` に記載（git管理外）

## カスタマイズ

このテンプレートは最小構成です。プロジェクトに応じて追加してください：

- Python: `requirements.txt`, `pyproject.toml`, `pytest.ini`
- Node.js: `package.json`, `.nvmrc`
- Docker: `Dockerfile`, `docker-compose.yml`
