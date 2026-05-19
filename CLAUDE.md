# CLAUDE.md

**必ず日本語で回答すること。**

## Obsidian Vault
パス: `/Users/shu/Library/Mobile Documents/iCloud~md~obsidian/Documents/Obsidian Vault`
主なフォルダ: `Notion/アカウント・認証情報/`, `Notion/開発・APIキー/`, `Notion/mexicosou のノートブック/` (レシピ・キャンプ・登山), `Clippings/`

## Claude × Obsidian 連携ルール
MCP経由でVaultを「外部脳」として読み書きし、セッションを跨いで知識を引き継ぐ。

**セッション開始時:** `Knowledge/mistakes.md` と `Preferences/` を読み、質問に関連するVaultを検索して回答に反映する（単発の無関係質問はスキップ可）。

**書き込み条件（その場で即時書く）:**
- Knowledge/: バグ解決・新発見・ハマり解消
- Decisions/: 選択肢から1つを選んだ判断
- Projects/: プロジェクト状態の変化
- Preferences/: ユーザーの好み・スタイルの発見

**フォーマット:** YAMLフロントマター必須（date/tags/project/related）。命名: Knowledge=`topic-subtopic.md`, Decisions=`YYYY-MM-DD-topic.md`

**mistakes.md追記条件:** ①ユーザーからの明示的訂正 ②再発しうるパターン ③「する/しない」で書けるもの。形式: `YYYY-MM-DD: [NG Action] → [Correct Action] / Trigger`

**報告:** 読み書きしたら必ず明示する（例:「Obsidian: Knowledge/xxx.md を読みました」）

## デザインシステム
UI作業時は `~/design.md/design.md` を参照（Warp Future Emerald V3: 背景`#061a0c`、アクセント`#00ff41`、サイバーパンクCLIスタイル）

## 作業スタイル
シンプル優先・冗長な説明省く・既存パターンに合わせる・動作確認は自分で完結
