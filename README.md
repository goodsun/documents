# Documents Vault

開発案件のドキュメントを蓄積する Obsidian Vault。**人間が読んで繋げる**ことと、**Claude Code が文脈として読み書きする**ことの両方を目的とする。

## フォルダ構成

| フォルダ | 役割 | フロー/ストック |
|---------|------|----------------|
| `00_inbox/` | 未整理の走り書き。後で各案件へ振り分ける | フロー |
| `10_projects/` | 案件ごとのドキュメント | 両方 |
| `20_knowledge/` | 案件をまたぐ横断ナレッジ・Tips・コマンド集 | ストック |
| `90_templates/` | 新規ノート用テンプレート | - |
| `_attachments/` | 画像などの添付ファイル | - |

## 運用ルール

### フロー / ストックを分ける
- **フロー**（日々動くもの）: 作業ログ・調査メモ → 案件の `log/` に日付ファイルで
- **ストック**（確定した知識）: 仕様・設計・経緯 → 案件の `spec/` に

### 案件 README = 入口（MOC）
各案件フォルダの `README.md` はその案件の **Map of Content**。概要・関連リポジトリ・主要ノートへのリンク・現状を集約する。新しい案件を触るときも、Claude Code を起動するときも、まずここを読む。

### 全ノートにフロントマターを付ける
`status` / `tags` / `project` を付けておくと、Dataview で横断集約できる（後述）。

### リンクは積極的に張る
関連ノートは `[[ノート名]]` で繋ぐ。グラフビューとバックリンクで知識が育つ。

## 新しい案件の作り方

`10_projects/sample_proj/` が骨格の **お手本** です。これを見れば README(MOC) / `spec/` / `log/` の書き分けが分かります。

1. `10_projects/<案件名>/` を作る（`sample_proj` をコピーするのが早い）
2. `90_templates/案件README.md` をコピーして `README.md` に（または sample_proj の README を流用）
3. 規模に応じて:
   - **小案件**: フォルダ直下にノートをフラットに置く
   - **中案件以上**: `spec/`（仕様）と `log/`（作業ログ）を切る

## 推奨 Obsidian プラグイン

- **Dataview**: フロントマターを使って「進行中の全案件」「未完了タスク一覧」を自動集約
- **Templates**（コア）: `90_templates/` をテンプレフォルダに設定

### Dataview クエリ例

進行中の案件一覧:
````
```dataview
TABLE status, updated
FROM "10_projects"
WHERE file.name = "README" AND status = "active"
SORT updated DESC
```
````

全案件の未完了タスク:
````
```dataview
TASK
FROM "10_projects"
WHERE !completed
GROUP BY file.folder
```
````

## Claude Code との連携

このVault直下の `CLAUDE.md` に、AIがどこに何を書くかのルールを定義している。AIに調査・分析を依頼する際は、結果を該当案件の `log/` や `spec/` に残させること。
