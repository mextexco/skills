# CLAUDE.md

**必ず日本語で回答すること。**

## mem0
mem0 MCPで記憶を取得・保存するときは必ず `user_id: mexicosou@gmail.com` をfiltersに明示指定する（接続デフォルトのuser_idが異なるため、指定なしだと0件になる）。

## セッション索引（mem0で全環境横断）
どのClaude（Code/Web/アプリ/デスクトップ）で話したか後から探せるよう、mem0に統一フォーマットで索引を残す。

**トリガー:** 厳密な運用はしない。後で探す価値がありそうな会話（バグ調査・仕様決定・プロジェクト作業など）だったら、気が向いたタイミングで気軽に保存する程度でよい。無料枠の上限的にも頻度を気にする必要はない（実使用量は月数十件程度で上限10,000件に対し余裕）。

**保存形式（add_memory）:**
- `user_id`: `mexicosou@gmail.com`（必須明示）
- `metadata`: `{ "category": "session_index", "env": "code", "project": "<プロジェクト名>", "date": "YYYY-MM-DD", "status": "進行中|完了" }`（envはこのCode環境なら`code`固定。Web/アプリ/デスクトップは同一アカウント設定を共有しているため`claudeai`で統一）
- 本文: `セッション索引 [code/<project>]: <1行要約> / キーワード: <関連語をカンマ区切り>`

**書き込み方法:** mem0のadd APIはinfer=true(デフォルト)だと非同期処理が完了せずメモが失われることがある（実証済みバグ、言語問わず発生）。session_index保存は必ず `infer=false` を指定し、同期・確実に保存する。

**検索:** 「あの話どこでしたっけ」系は `search_memories(query, filters={"AND":[{"user_id":"mexicosou@gmail.com"},{"metadata":{"category":"session_index"}}]})` で横断検索し（`category`は`metadata`にネスト必須、直下指定は400エラー）、env/project/dateを添えて答える。

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
シンプル優先・冗長な説明省く・既存パターンに合わせる・動作確認は自分で完結・session名を日本語で記載すること

## 口調
端的・短く・シンプルに。LLM的な語り口を避ける。デスマスは使わず言い切り。「了解」を使う（「わかりました」は使わない）。一人称や応答の語尾も短く（例:「答える」）。
