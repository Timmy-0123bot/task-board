# CLAUDE.md

このファイルは、本リポジトリで作業する Claude Code へのガイドです。

## プロジェクト概要

- プロジェクト名: task-board
- 概要: タスク管理ボードアプリケーション（タスクの追加・完了切り替え・削除）
- 構成:
  - `src/App.jsx` — タスクボード本体（状態管理は `useState`、タスクは localStorage の `task-board.tasks` に保存）
  - `src/App.css` — スタイル
  - `src/main.jsx` — エントリーポイント

## 技術スタック

| 分類 | 使用技術 |
| --- | --- |
| UI ライブラリ | React 19（関数コンポーネント + Hooks） |
| 言語 | JavaScript（JSX）。TypeScript は未使用 |
| ビルドツール | Vite 8（`@vitejs/plugin-react`） |
| スタイル | 素の CSS（`src/App.css`）。CSS フレームワークは未使用 |
| データ保存 | ブラウザの localStorage（サーバー・DB なし） |
| 実行環境 | Node.js 24 |
| ホスティング | GitHub Pages（GitHub Actions で自動デプロイ） |

- 外部ライブラリの追加は必要最小限にし、追加する場合は理由をユーザーに伝える

## 開発コマンド

- 依存関係のインストール: `npm install`
- 開発サーバーの起動: `npm run dev`
- ビルド: `npm run build`
- テスト: 未導入
- Lint / フォーマット: 未導入

## デプロイ先

https://timmy-0123bot.github.io/task-board/

- GitHub Pages で公開している
- `main` へのプッシュで `.github/workflows/deploy.yml` が実行され、`dist` が自動デプロイされる
- `vite.config.js` の `base: './'` は Pages のサブパス配下で動かすための設定なので変更しない

## コーディング規約

- 既存コードの命名・書き方・コメント量に合わせる
- 変更は依頼された範囲に留め、無関係なリファクタリングは行わない
- 秘密情報（APIキー、パスワード、`.env` の中身など）はコミットしない

### コンポーネントの命名規約

| 対象 | 規約 | 例 |
| --- | --- | --- |
| コンポーネント名 | PascalCase | `App`, `TaskItem` |
| コンポーネントのファイル | コンポーネント名と同じ PascalCase + `.jsx` | `App.jsx`, `TaskItem.jsx` |
| 定義の書き方 | `export default function 名前()` の関数宣言で、1ファイル1コンポーネント | `export default function App()` |
| state | `[名詞, set名詞]` | `[tasks, setTasks]`, `[text, setText]` |
| イベント処理関数 | 動詞 + 名詞の camelCase | `addTask`, `toggleTask`, `deleteTask` |
| 親から渡すイベント props | `on` + 動詞 | `onToggle`, `onDelete` |
| コンポーネント外の関数 | camelCase | `loadTasks` |
| 定数 | UPPER_SNAKE_CASE | `STORAGE_KEY` |
| CSS クラス | kebab-case | `add-form`, `task-list` |
| 状態を表す CSS クラス | 基本クラスに追加する | `task done` |

- コンポーネントを分割するときは `src/components/` に置く（例: `src/components/TaskItem.jsx`）
- UI の文言・`aria-label` は日本語で書く

## Git 運用ルール

### 基本方針

- **コードを変更するたびに、コミットして GitHub にプッシュすること。**
  - 1つの作業（機能追加・バグ修正・設定変更など）が完了したら、その都度 `git add` → `git commit` → `git push` まで行う
  - 変更をローカルに溜め込まない

### 手順

```bash
git status                 # 変更内容を確認
git add <変更したファイル>   # 意図したファイルのみステージする（git add -A は避ける）
git commit -m "<メッセージ>"
git push
```

### コミットメッセージ

- 日本語で、何を・なぜ変更したかを簡潔に書く
- 1行目は要約（50文字程度まで）、必要なら空行を挟んで詳細を書く
- 例: `タスクカードのドラッグ&ドロップ移動を追加`

### 注意事項

- プッシュ前に、テストや Lint があれば実行して通ることを確認する
- `git push --force`、`git reset --hard` などの破壊的な操作は、ユーザーの明示的な許可なしに行わない
- フックの回避（`--no-verify`）は行わない
- プッシュに失敗した場合（リモートとの競合など）は、無理に解決せずユーザーに報告する
