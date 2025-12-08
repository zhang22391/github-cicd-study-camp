## Git CI/CD 勉強会

『GitHub CI/CD 実践ガイド』第2章〜第13章を対象に、全10回（各3時間）で **輪読＋モブプロ形式** の勉強会を行います。

- 各回は  **主催者の説明 → チーム内交代音読 → モブプロでハンズオン** という流れで進行します。  
- チームごとに独立したブランチ・環境で実習を行い、最後に **学習メモ (.md)** を `main` ブランチへまとめます。

---

### 🧭 基本ルール

```bash
# チームAの例
git checkout team-a     # チーム用ブランチへ切り替え
# コード修正・検証
git add .
git commit -m "Add: team-a work for chapter X"
git push -u origin team-a
```
- 各チームは `team-a / team-b / team-c …` のように独立したブランチを使用します。
- `main` には 学習メモ (.md)のみを反映し、コードは `team` ブランチに残します。

### 📁 リポジトリ構成イメージ
- #### 各チームブランチ（例：team-a）
```bash
team-a
├─ teams/team-a/                # チーム内コード & デモとか
│  ├─ ch03/ ...
│  └─ ch04/ ...
└─ .github/workflows/
   └─ your-ci-name.yml             # チームA専用ワークフロー
```
**注意**：
1. Workflow ファイル名（例：`your-ci-name.yml`）は自由ですが、CIファイル内の`name: xxx`には必ずチーム名を含めてください。
例：
```
name: my-ci-team-a
```
2. `.github/workflows/` 配下には「現在利用中（アクティブ）」の Workflow のみを置き、<br> 実行後に`teams/team-a/ch01`などの各チーム用フォルダへ移動してください。<br>
`.github/workflows/`に置いたままだと再び実行される可能性があるため。

3. 一度実行された CI は、GitHub Actions の「All Workflows」に履歴として残ります。
   - `.yml`ファイルを削除しても過去の実行履歴は削除できません（GitHub の仕様です）。
   - 一覧に残っていても、ファイルが存在しなければ今後は実行されませんのでご安心ください。

- #### mainブランチ
```bash
main
├─ README.md
├─ .github/
│  └─ workflows/
│     ├─ manual.yml              # 手動トリガーCIを流す練習用
│     ├─ auto-merge-docs.yml     # .mdのみ自動マージ
│     └─ guard-main.yml          # mainはdocs/**のみ許可
└─ docs/
   ├─ 第01回_第2章_Actionsの基本/
   │  ├─ teamA_第01回.md
   │  ├─ teamB_第01回.md
   │  └─ teamC_第01回.md
   ├─ 第02回_第3章_ワークフロー基礎/
   │  ├─ teamA_第02回.md
   │  └─ ...
   └─ ...
```

**注意**：コードは各チームブランチにのみ残し、`main` ブランチには **学習メモ（.md）** のみをコミットします。

### 🧱 各回の手順

1. 主催者が当日の章と目的を説明 (5~10min)

2. チーム内で交代しながら音読（輪読）

3. モブプロ形式でハンズオン実施
   - `git checkout -b team-<番号(小文字)>`
   - `./github/workflows/` 以下にチーム用 workflow を作成、チームブランチへpush・検証
   - 毎章のコードは `docs/0x-章名/CODE.md` で参照可能
   - 使用完了のコードは `teams/team-<番号>/chXX/` 以下に移動
   - 毎回終了後に `docs/0x-章名/` 以下に学習メモを作成（`例：team-<番号(小文字)>.md`）
   - 学習メモは以下の Template を参考にしてください
   - 学習メモ作成したら `main` ブランチへ PR

4. 必要に応じて専用の[ GPT ](https://chatgpt.com/g/g-691dce579db881b8ad74c52b837a7427-cicd4ke-mian-qiang-hui-zhuan-yong-kasutamugpt)または Slack で質問

5. 各チームごとに学習メモを作成

6. `main` ブランチへ学習メモ（.md）のみ PR

### 🧩 学習メモTemplate

- **パス**：`docs/03-継続的インテグレーションの実践(第4章)/`
- **ファイル名**：`team-a.md`
```markdown
# 3-継続的インテグレーションの実践(第4章)- Team A

## 学んだこと
- GitHub Actionsのworkflow設計方法
- キャッシュとartifactの使い方
- CIの再利用と権限管理

## 実施内容（モブプロ）
- pushトリガーでテスト自動実行
- Slack通知の設定
- 実際にworkflowを編集・検証

## 質問・課題
- matrix設定の最適化方法
- キャッシュキーのベストプラクティス
- Secretsの共有管理について

## 補足・Tips
- `${{ github.ref_name }}` の利用が便利
- jobの並列実行は `needs:` で制御可能

## Feedback
- GPTを使った質問が便利だった
- モブプロ中の交代タイミングをもう少し短くしてもよさそう
```

### ⚠️ チーム間での衝突リスクと解決方案 (chapter7〜13)

| 章                              | 衝突リスク                                                                      | 解決方案                                                                                                           |
| ------------------------------ |----------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| **第7章：リポジトリ運用**                | - `Dependabot`が`main`に自動`PR`を送るため、各チームの運用に影響する可能性<br> - リポジトリ設定まわりの変更は全体に影響 | - Dependabot は main のみで管理（練習は任意）<br> - 設定変更系は主催者以外触らない                                                      |
| **第8章：依存関係アップデート**             | - 自動生成`PR`が複数チームにとってノイズになる可能性                                              | - 必要なとき以外 Dependabot 無効化 or main のみ対象                                                                          |
| **第9章：Git Tag・Release**        | - `Tag`名は`repo`全体で共有 → 別チームが同じ`tag`名を`push`すると衝突                           | **必ず team prefix を付ける：**`team-a-v1.0.0` / `team-b-v1.0.0`<br>Release も同じ prefix を使う                        |
| **第10章：GitHub Packages（GHCR）** | - GHCRの`package`名 / `tag`名は`repo`全体共有 → 上書きリスク                             | **team prefix を付ける：**`ghcr.io/org/repo/team-a-app:v1`<br>または `${{ github.ref_name }}-${{ github.run_number }}` |
| **第11章：OIDC + AWS 連携**         | - `Environment`名 / `Secrets`名は`repo`全体で共有 → 衝突リスク                          | **環境・Secrets も team prefix 化：**<br>`team-a-dev` / `team-b-dev`<br>`AWS_TEAM_A_ROLE` / `AWS_TEAM_B_ROLE`        |
| **第12章：デプロイ戦略**                | - deploy 先が共通の場合、複数チームが同時に書き換える危険                                          | - **必ず team ごとの deploy 先を分離**（S3 バケット / ECS サービス / Lambda など）                                              |
| **第13章：Action 公開**             | - Marketplace に公開する`Action`は`repo`内で重複したり干渉する可能性                            | - `Action`は各チーム`branch`のみで作り、**公開**（publish）は 個人 fork のみ                                                       |