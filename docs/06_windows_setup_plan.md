# 06 Windows セットアップ計画

状態: **設計案（承認待ち）** / 対象: Windows 11・PowerShell 優先

## 1. 前提ソフト
| ソフト | 版 | 備考 |
|---|---|---|
| Python | 3.12 系 | PATH 登録済みの `py` ランチャー推奨 |
| Node.js | 20 以上（LTS） | **開発・ビルド時のみ**。配布用に `dist/` を同梱できれば利用時は不要 |
| Git | 任意 | `update.bat` で使用 |
| ブラウザ | Edge / Chrome | |

## 2. スクリプト（`.bat` は `scripts/*.ps1` を呼ぶ薄いラッパー）
| ファイル | 動作 |
|---|---|
| `setup.bat` | 前提確認 → `.venv` 作成 → `pip install -r backend/requirements.txt` → `npm ci && npm run build` → `data/` 作成 → `.env` を `.env.example` から作成（既存なら触らない）→ DB 初期化 |
| `start.bat` | `.venv` で uvicorn を `127.0.0.1:8765` 起動 → PID を `data/run/app.pid` に記録 → ブラウザで `http://127.0.0.1:8765` を開く。ポート使用中なら案内して終了 |
| `stop.bat` | `app.pid` の PID（とその子プロセス）だけ停止。他の Python/Node は止めない。PID ファイルが古ければ削除 |
| `update.bat` | 先に自動バックアップ → `git pull` → 依存更新 → ビルド → マイグレーション。失敗時は復元手順を表示 |
| `backup.bat` | `data/` を日時付き ZIP にして `backups/` へ。世代は直近 10 件（設定可）|

## 3. 仕様
- 日本語パス・空白パスで動くよう、PowerShell ではパスを必ず引用符で扱い、`chcp 65001` / UTF-8 を設定。
- 実行ポリシー対策: `.bat` 内で `powershell -NoProfile -ExecutionPolicy Bypass -File …` を使う（システム全体の設定は変更しない）。
- 管理者権限は不要。ファイアウォール許可ダイアログが出ない（127.0.0.1 のみ）。
- CPU 動作。rembg は任意（`pip install -r backend/requirements-optional.txt`）。
- ログは `data/logs/`（APIキー・個人情報をマスク）。
- `.env` に `OPENAI_API_KEY` 等（初期は空・API 無効）。

## 4. 初回導入手順（利用者向け・予定）
1. リポジトリを取得（ZIP または `git clone`）
2. `setup.bat` をダブルクリック
3. `start.bat` をダブルクリック → ブラウザが開く
4. 設定画面で保存先・フォント・テーマを確認

## 5. 障害時の方針
- 各スクリプトは失敗時に**日本語で原因と次の手順**を表示して停止。既存データには触れない。
- `setup.bat` は再実行しても安全（冪等）。
- 更新・移行の前には必ずバックアップを取る。

## 6. 開発者（Claude Code）向けメモ
- 実装フェーズでは Windows 実機での検証が必要。クラウド側（Linux）では起動スクリプトの実動作確認ができないため、**未検証**として報告する。
