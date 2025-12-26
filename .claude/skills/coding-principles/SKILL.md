---
name: coding-principles
description: 設計・実装の基本原則。12 Factor App、Clean Code、SOLID、DRY/KISS/YAGNIの要点と適用方法
---

# コーディング原則

## 設計の格言

| 原則 | 意味 | 適用 |
|------|------|------|
| **DRY** | Don't Repeat Yourself | 同じロジックを2箇所以上に書かない。ただし、早すぎる抽象化は避ける |
| **KISS** | Keep It Simple, Stupid | 複雑な解法より単純な解法を選ぶ。読めないコードは正しくない |
| **YAGNI** | You Aren't Gonna Need It | 今必要ない機能を作らない。将来の拡張性より現在の単純さ |

## 12 Factor App（設定と環境）

クラウドネイティブアプリの原則。特に重視するもの：

### Config（設定）
```python
# ✗ ハードコード
API_URL = "https://api.example.com"

# ○ 環境変数
API_URL = os.environ.get("API_URL")
```
- 設定は `.env` で管理、コードに書かない
- `.env.example` をコミットし、実際の `.env` はコミットしない

### Dependencies（依存関係）
```bash
# 明示的に宣言
pip freeze > requirements.txt

# バージョン固定
requests==2.31.0  # ○
requests>=2.0     # ✗ 範囲指定は避ける
```

### Logs（ログ）
```python
# ✗ ファイルに直接書く
with open("app.log", "a") as f:
    f.write(message)

# ○ 標準出力
print(message)  # または logging モジュール
```
- ログはイベントストリームとして標準出力へ
- ログの収集・保存はインフラ層の責務

> 詳細: https://12factor.net/ja/

## Clean Code（可読性）

### 命名
```python
# ✗ 曖昧
def calc(d):
    return d * 1.1

# ○ 意図が明確
def calculate_price_with_tax(base_price):
    TAX_RATE = 1.1
    return base_price * TAX_RATE
```
- 変数名・関数名で意図を伝える
- 略語は避ける（`usr` → `user`）
- 検索可能な名前をつける

### 関数
```python
# ✗ 長すぎる、複数の責務
def process_user_data(user):
    # バリデーション（50行）
    # DB保存（30行）
    # メール送信（20行）
    pass

# ○ 単一責務、短い
def validate_user(user): ...
def save_user(user): ...
def send_welcome_email(user): ...
```
- 1関数 = 1責務
- 20行を超えたら分割を検討
- 引数は3つ以下が理想

### コメント
```python
# ✗ 何をしているか（コードを読めばわかる）
# ユーザーを取得する
user = get_user(id)

# ○ なぜそうしているか（コードからはわからない）
# キャッシュを使わない: リアルタイム性が重要なため
user = get_user(id, use_cache=False)
```
- 「何を」ではなく「なぜ」を書く
- コメントが必要なコードより、コメント不要な明確なコードを目指す

## SOLID原則

オブジェクト指向設計の5原則。すべてを厳密に守る必要はないが、意識する：

| 原則 | 要点 | 違反の兆候 |
|------|------|------------|
| **S** Single Responsibility | 1クラス = 1責務 | クラスが肥大化、変更理由が複数 |
| **O** Open/Closed | 拡張に開き、修正に閉じる | 新機能追加で既存コード修正が必要 |
| **L** Liskov Substitution | 派生クラスは基底クラスと置換可能 | 継承で例外的な振る舞いが必要 |
| **I** Interface Segregation | インターフェースは小さく分割 | 使わないメソッドの実装を強制 |
| **D** Dependency Inversion | 具象ではなく抽象に依存 | テストしにくい、モック困難 |

### 実践例（Dependency Inversion）
```python
# ✗ 具象に依存
class UserService:
    def __init__(self):
        self.db = PostgresDatabase()  # 具体的なDBに依存

# ○ 抽象に依存
class UserService:
    def __init__(self, db: Database):  # インターフェースに依存
        self.db = db
```

## アンチパターン

避けるべき設計・実装：

| パターン | 問題 | 対策 |
|----------|------|------|
| God Class | 1クラスに責務集中 | 責務ごとに分割 |
| Magic Number | 意味不明な数値 | 定数化して名前をつける |
| Deep Nesting | 深いif/forのネスト | 早期リターン、関数分割 |
| Copy-Paste | コード複製 | 共通化（ただし早すぎる抽象化は避ける） |
| Premature Optimization | 早すぎる最適化 | まず動くコード、計測してから最適化 |

## チェックリスト

コードレビュー・PR前に確認：

- [ ] 命名は意図を伝えているか
- [ ] 関数は短く、単一責務か
- [ ] ハードコードされた設定値がないか
- [ ] 重複したロジックがないか
- [ ] 不要な複雑さがないか（KISS）
- [ ] 使われていないコードがないか（YAGNI）
