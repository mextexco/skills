# skills

市原崇（mextexco）の Claude Code スキル管理リポジトリ。

複数のMacで同じスキルと設定を使えるように、スキルファイルをGitHubで同期する。

## 構成

```
skills/
├── agents-skills/        # ~/.agents/skills/ の実体
│   ├── excalidraw-diagram-generator/
│   ├── find-skills/
│   └── sync-skills/      # このリポジトリを同期するスキル
├── claude-skills/        # ~/.claude/skills/ のコピー（シンボリックリンクを解決済み）
└── CLAUDE.md             # Claude Code のグローバル設定
```

## 初回セットアップ（新しいPC）

```bash
# 1. cloneする
git clone git@github.com:mextexco/skills.git ~/skills-repo

# 2. スキルを配置
mkdir -p ~/.agents/skills ~/.claude/skills
cp -r ~/skills-repo/agents-skills/* ~/.agents/skills/

# 3. シンボリックリンクを作成
for skill in ~/.agents/skills/*/; do
  ln -sf "$skill" ~/.claude/skills/$(basename "$skill")
done

# 4. CLAUDE.md をコピー
cp ~/skills-repo/CLAUDE.md ~/.claude/CLAUDE.md
```

## 日常的な同期

初回セットアップ後は `sync-skills` スキルが使えるようになる。

- **このPCのスキルをGitHubにpush**：「スキルをpushして」
- **GitHubから最新のスキルをpull**：「スキルをpullして」

## スキル一覧

| スキル名 | 説明 |
|---|---|
| `find-skills` | スキルマーケットプレイスの検索・インストール |
| `excalidraw-diagram-generator` | Excalidraw図の自動生成 |
| `sync-skills` | このリポジトリとのスキル同期 |
