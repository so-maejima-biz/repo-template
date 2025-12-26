---
name: python-dev
description: Python開発環境（.venv）のセットアップ、実行、テスト、フォーマット手順。Windows環境での注意点を含む
---

# Python Development Skill

## 仮想環境 (.venv)

### 新規作成
```powershell
# Windows
python -m venv .venv
```

### アクティベート
```powershell
# Windows PowerShell
.venv\Scripts\Activate.ps1

# Windows Command Prompt
.venv\Scripts\activate.bat

# Git Bash on Windows
source .venv/Scripts/activate
```

### 確認方法
```powershell
# Python のパスを確認
where python
# 期待: C:\...\project\.venv\Scripts\python.exe

# sys.prefix で確認
python -c "import sys; print(sys.prefix)"
# 期待: C:\...\project\.venv
```

## Windows での Troubleshooting

### ExecutionPolicy エラー
PowerShell で `.ps1` スクリプトが実行できない場合：
```powershell
# 現在のポリシー確認
Get-ExecutionPolicy

# 一時的に許可（現在のセッションのみ）
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass

# または .bat を使用
.venv\Scripts\activate.bat
```

### venv が作成できない
```powershell
# Python のバージョン確認
python --version

# pip を最新化
python -m pip install --upgrade pip

# venv モジュール確認
python -m venv --help
```

## 依存管理

### requirements.txt
```bash
# インストール
pip install -r requirements.txt

# 現在の環境を出力
pip freeze > requirements.txt

# 開発用と本番用を分ける場合
pip install -r requirements-dev.txt
```

### 注意点
- `pip freeze` は全パッケージを出力するため、必要なものだけ手動で管理する方が安全
- バージョン固定: `package==1.2.3` を推奨

## テスト実行

```bash
# pytest
python -m pytest
python -m pytest tests/ -v
python -m pytest --cov=src  # カバレッジ付き

# unittest
python -m unittest discover
```

## コードフォーマット

```bash
# Black (フォーマッタ)
pip install black
black .

# isort (import整理)
pip install isort
isort .

# flake8 (リンター)
pip install flake8
flake8 .
```

## よくあるエラー

### ModuleNotFoundError
```bash
# 仮想環境がアクティブか確認
where python

# パッケージがインストールされているか確認
pip list | grep package_name
```

### pip が古い
```bash
python -m pip install --upgrade pip
```
