---
name: sync-skills
description: スキルファイルをGitHubリポジトリ（mextexco/skills）と同期する。「スキルをpushして」「他のPCに同期して」「skillsを更新して」などと言われたときに使う。
---

# Sync Skills

このスキルは `~/.agents/skills/` の内容を GitHub リポジトリ `mextexco/skills` と同期する。
`~/.claude/skills/` はシンボリックリンクのため、リポジトリには含めない。

## リポジトリ情報

- GitHub: `git@github.com:mextexco/skills.git`
- ローカルの作業ディレクトリ: `~/.claude/jobs/` 配下の適当なパス、またはユーザーが指定したパス
- スキル実体: `~/.agents/skills/`
- スキルリンク: `~/.claude/skills/`

## When to Use This Skill

ユーザーが以下のようなことを言ったとき：

- 「スキルをpushして」「GitHubに同期して」「skillsをアップデートして」
- 「他のPCでスキルを使いたい」「スキルを復元して」「スキルをpullして」
- 「sync-skills」と直接呼んだとき

## 操作モード

### モード1: Push（このPC → GitHub）

現在のスキルをGitHubにpushする。

```bash
# 作業用ディレクトリに移動（既存のskills-repoがあればそこへ）
REPO_DIR="$HOME/.claude/jobs/skills-sync-repo"

# なければclone、あればpull
if [ ! -d "$REPO_DIR/.git" ]; then
  git clone git@github.com:mextexco/skills.git "$REPO_DIR"
else
  git -C "$REPO_DIR" pull
fi

# スキルの実体をコピー（シンボリックリンクを解決して実体をコピー）
cp -rL ~/.agents/skills/. "$REPO_DIR/agents-skills/"
cp ~/.claude/CLAUDE.md "$REPO_DIR/CLAUDE.md"

# コミットしてpush
cd "$REPO_DIR"
git add .
git diff --cached --stat
git commit -m "sync skills: $(date '+%Y-%m-%d %H:%M')"
git push
```

### モード2: Pull（GitHub → このPC）

GitHubからスキルを取得してこのPCに展開する。

```bash
REPO_DIR="$HOME/.claude/jobs/skills-sync-repo"

# clone or pull
if [ ! -d "$REPO_DIR/.git" ]; then
  git clone git@github.com:mextexco/skills.git "$REPO_DIR"
else
  git -C "$REPO_DIR" pull
fi

# スキルを展開
mkdir -p ~/.agents/skills ~/.claude/skills
cp -r "$REPO_DIR/agents-skills/." ~/.agents/skills/

# シンボリックリンクを再作成
for skill in ~/.agents/skills/*/; do
  name=$(basename "$skill")
  ln -sf "$skill" ~/.claude/skills/"$name"
done

# CLAUDE.md をコピー（既存があれば確認してからコピー）
cp "$REPO_DIR/CLAUDE.md" ~/.claude/CLAUDE.md

echo "同期完了！インストール済みスキル:"
ls ~/.claude/skills/
```

## 実行手順

1. ユーザーが push か pull かを確認する（文脈から明らかなら確認不要）
2. 上記のコマンドを Bash ツールで実行する
3. 結果を報告する（追加・更新されたスキル一覧など）

## 注意事項

- SSH鍵が設定されていない場合、`git@github.com:...` の接続が失敗する。その場合はユーザーにSSH鍵の設定を促す。
- `cp -rL` でシンボリックリンクの実体をコピーしているため、リンク先が消えていても問題ない。
- Pull時、既存のスキルは上書きされる。カスタマイズしている場合は事前に確認する。
