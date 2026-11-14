# Claude Code 開発指示
最初に HANDOFF.md と index.html を読み、現在のモバイルMVPを維持してユーザーの依頼に沿って開発する。以下の旧Windows設計方針は将来の大規模移行に適用し、既存MVPの小規模修正を妨げない。

README.md、SPECIFICATION.md、ROADMAP.md、TASKS.md、UI_DESIGN.md、STAMP_RULES.md を読む。
Windows版への大規模移行は初回設計のみ。docs/ にシステム構成、画面遷移、DB、フォルダ構成、テスト計画、Windowsセットアップ計画を作成し承認を待つ。承認前に実装や有料API呼び出しをしない。
Windows 11 / PowerShell優先。React/TypeScript、Python/FastAPI、SQLite、Pillow/OpenCVを基本案とする。変更時は理由を提示。
日本語UI、レスポンシブ、CPU動作、日本語パス対応。初期サーバーは127.0.0.1のみ。APIキーは環境変数、ログに出さない。公開GitHubに写真や個人情報をコミットしない。
機能ごとにテストし結果と未検証事項を報告。既存データを破壊しない。TASKS.mdを更新。外部公開・課金・販売申請は承認が必要。
