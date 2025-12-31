# CLAUDE.md - AI開発ガイドライン

## 基本原則
- **秘密情報をコードに書かない**: `.env` で管理し、`.env` は git にコミットしない
- **破壊的なgit操作を避ける**: `--force`, `reset --hard` は明示的な許可なく実行しない
- **最小限の変更**: 必要な箇所のみ修正。過剰な設計をしない（KISS, YAGNI）
- **コミット前に確認**: ステージされた変更を必ずレビューしてからコミット

## 禁止事項
- **後方互換の名目で未使用コードを残さない**: 削除予定のコードは即座に削除
- **未使用の変数・引数・関数を残さない**: 使われていないものは削除
- **進捗コメントを書かない**: `// TODO: 完了`, `// 削除予定` 等は書かずに実行する

> 設計原則の詳細は `.claude/skills/coding-principles/` を参照

## コード編集の前に
- 目的とスコープを明確にしてからファイルに触れる
- 複数ファイルや破壊的な変更の場合は、計画を示して承認を待つ
- 編集後は、何を変更したか要約する

> 詳細な手順は `.claude/skills/code-edit-workflow/` を参照

## Pythonプロジェクト (.venv)
Python作業を開始する前に、仮想環境に入っているか確認:
```powershell
where python              # .venv のパスが表示されるはず
python -c "import sys; print(sys.prefix)"  # .venv ディレクトリが表示されるはず
```
未アクティベートの場合: `.venv\Scripts\activate` (Windows)

> 詳細なセットアップは `.claude/skills/python-dev/` を参照

## よく使うコマンド
```bash
# Git
git status                 # コミット前に必ず確認
git diff --cached          # ステージされた変更を確認

# Python (.venv アクティベート後)
pip install -r requirements.txt
python -m pytest           # テスト実行
```

## PR前チェックリスト
- [ ] ハードコードされた秘密情報がないか
- [ ] `.gitignore` で機密ファイルが除外されているか
- [ ] テストが通るか（該当する場合）
- [ ] プロジェクトの規約に沿っているか

## プロジェクトスキル
詳細な手順は `.claude/skills/` にあります。
現在のスキル一覧は `ls .claude/skills/` で確認。

## ドキュメント更新トリガー
以下の場合、このファイルまたはREADME.mdの更新を検討:
- `.claude/skills/` にスキルを追加/削除したとき
- プロジェクトの基本ルールが変わったとき

## 個人用メモ
端末固有の設定は `CLAUDE.local.md` に記載（git管理外）。
