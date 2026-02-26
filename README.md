# articles

Zenn + Qiita cross-posting repo. Write once, publish everywhere.

## Structure

```
articles/          Zenn articles (auto-published via GitHub integration)
books/             Zenn books
qiita/public/      Qiita articles (auto-published via GitHub Actions)
images/            Shared images
```

## Initial Setup

### 1. Install dependencies

```bash
bun install
```

### 2. Zenn GitHub integration

1. https://zenn.dev/dashboard/deploys を開く
2. "リポジトリを連携する" をクリック
3. `r1cA18/articles` を選択
4. 連携完了 -> 以降 `articles/` への push で自動公開

### 3. Qiita API token

1. https://qiita.com/settings/applications を開く
2. "新しいトークンを発行する" をクリック
3. スコープ: `read_qiita`, `write_qiita` にチェック
4. トークンをコピー

GitHub Secrets に登録:

```bash
gh secret set QIITA_TOKEN
# プロンプトが出たらトークンを貼り付け
```

ローカルでも使う場合:

```bash
bunx qiita login
# トークンを入力
```

### 4. X Premium

X Articles を使うには Premium ($8/month) への課金が必要。
https://x.com/i/premium_sign_up

## Daily Workflow

### Zenn article

```bash
# 1. Create
bun run zenn:new
# -> articles/xxxxx.md が生成される

# 2. Edit
# 好きなエディタで articles/xxxxx.md を編集
# - title, topics を設定
# - published: false のまま書き進める

# 3. Preview
bun run zenn:preview
# -> http://localhost:8000 でプレビュー確認

# 4. Publish
# published: true に変更して push
git add articles/xxxxx.md
git commit -m "feat: add article about ..."
git push
# -> Zenn に自動公開
```

### Qiita article

```bash
# 1. Create
bun run qiita:new
# -> qiita/public/xxxxx.md が生成される

# 2. Edit
# qiita/public/xxxxx.md を編集

# 3. Preview
bunx qiita preview
# -> localhost でプレビュー

# 4. Publish
git add qiita/public/xxxxx.md
git commit -m "feat: add qiita article about ..."
git push
# -> GitHub Actions が自動で Qiita に投稿
```

### X Articles

1. Zenn/Qiita 用に書いた Markdown をベースに
2. https://x.com/compose/article で新規作成
3. Markdown の内容をコピペ + X用にフォーマット調整
4. 公開

### Cross-posting (same article to both)

```bash
# 1. Zenn で先に書く
bun run zenn:new
# articles/my-article.md を書く

# 2. Qiita 用にコピー + frontmatter 変換
cp articles/my-article.md qiita/public/my-article.md
# frontmatter を Qiita 形式に手動変換:
#   emoji -> 削除
#   type -> 削除
#   topics -> tags に変更
#   published -> private に反転

# 3. Push (both published at once)
git add .
git commit -m "feat: add article about ..."
git push
```

## Frontmatter Reference

### Zenn

```yaml
---
title: "Article title"
emoji: "..."
type: "tech" # tech | idea
topics: ["swift", "ios"] # max 5
published: true # false = draft
---
```

### Qiita

```yaml
---
title: "Article title"
tags:
  - swift
  - ios
private: false # true = draft (inverse of Zenn's published)
---
```

## Commands

| Command                                 | Description                             |
| --------------------------------------- | --------------------------------------- |
| `bun run zenn:new`                      | Create new Zenn article                 |
| `bun run zenn:preview`                  | Preview Zenn articles at localhost:8000 |
| `bun run qiita:new`                     | Create new Qiita article                |
| `bun run qiita:publish`                 | Manually publish to Qiita               |
| `bunx zenn new:article --slug my-slug`  | Create with custom slug                 |
| `bunx zenn new:article --title "Title"` | Create with title                       |
