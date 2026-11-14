# 04 フォルダ構成

状態: **設計案（承認待ち）**

## リポジトリ（Git 管理対象）
```
wan-stamp-studio/
├─ CLAUDE.md  README.md  SPECIFICATION.md  ROADMAP.md
├─ TASKS.md  UI_DESIGN.md  STAMP_RULES.md  .gitignore
├─ docs/                      設計書（本フォルダ）
├─ backend/
│  ├─ app/                    FastAPI 本体（01 参照）
│  ├─ migrations/             0001_init.sql ...
│  ├─ tests/                  pytest
│  ├─ requirements.txt        必須依存
│  └─ requirements-optional.txt  rembg など
├─ frontend/
│  ├─ src/ (pages, components, api, theme, i18n)
│  ├─ package.json  vite.config.ts  tsconfig.json
│  └─ tests/                  Vitest / Playwright
├─ scripts/                   setup.ps1 start.ps1 stop.ps1 update.ps1 backup.ps1
├─ setup.bat start.bat stop.bat update.bat backup.bat   （ps1 を呼ぶ薄いラッパー）
├─ .env.example               キー名のみ（値なし）
└─ samples/                   権利クリアなテスト用ダミー画像のみ
```

## 実行時データ（Git 除外・.gitignore 済み）
```
data/                         ← 既定のデータルート（設定で変更可）
├─ app.sqlite
├─ characters/<id>/refs/      参考写真
├─ projects/<id>/
│  ├─ originals/              取り込んだ元シート（上書き禁止）
│  ├─ cells/                  分割結果
│  ├─ masks/                  マスク
│  ├─ processed/              加工途中
│  └─ final/                  01.png … main.png tab.png
├─ fonts/                     利用者が追加（商用利用権は利用者が確認）
├─ exports/                   ZIP 既定出力先
└─ backups/                   バックアップ ZIP
.env                          APIキー等（Git 除外）
.venv/  node_modules/  dist/  *.log
```

## 補足・注意
- **要注意（.gitignore）**: 指示書の `.gitignore` は `data/` `characters/` `projects/` `exports/` `backups/` `fonts/` `dist/` を先頭スラッシュなしで書いているため、**どの階層の同名フォルダも Git から除外される**（例: `frontend/src/pages/projects/` や `backend/app/characters/` が無言で追跡されなくなる）。
  - 対応案A（推奨・要承認）: データ系パターンをルート固定にする（`/data/` `/characters/` `/projects/` `/exports/` `/backups/` `/fonts/`）。`dist/` `node_modules/` はそのまま。
  - 対応案B: 現状のまま、ソース側で同名フォルダを使わない（`*_router.py`、`pages/ProjectList.tsx` のように命名）。
  - 本設計書は案Aを前提にしている。
- 出力先は設定で変更可能（`~/Documents` 等）。日本語パス・空白入りパスを許可。
- 画像の DB 保存パスは `data/` 起点の相対のみ。データフォルダを丸ごと別 PC へ移せる。
- `samples/` には写真・商用フォントを置かない。
