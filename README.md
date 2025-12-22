## Git CI/CD 勉強会

『GitHub CI/CD 実践ガイド』第2章〜第13章を対象に、全10回（各3時間）で **輪読＋モブプロ形式** の勉強会を行います。

- 各回は 主催者の説明 → チーム内交代音読 → モブプロでハンズオン という流れで進行します。  
- チームごとに独立したブランチ・環境で実習を行います。

---

### 🧭 基本ルール

- 各チームは `team-a / team-b / team-c …` のように独立したブランチを使用します。 
- `.github/workflows/` 配下には「現在利用中（アクティブ）」の Workflow のみを置き、<br>
    実行後に`第0x回/team-x/`などの各チーム用フォルダへ移動してください。<br>
    今回は、Helloワークフローがpushのたびに実行されるので、役割を終えたらフォルダ移動or削除してください。
- **ワークフロー名の後にチーム名いれること忘れずに** ‼️
- main では以下の制限を設けています： 
  - **main ブランチの修正は 主催者のみ許可**
  → 参加者の PR に 必ず 主催者がレビュー・マージを行います。

### 📁 リポジトリ構成イメージ

- #### mainブランチ
```bash
main
├─ README.md
├─ .github/
│  └─ workflows/
│     ├─ manual.yml              # 手動トリガーCIを流す練習用
│     └─ schedule.md             # Cron CIを流す練習用
├─ 第01回_第2章_Actionsの基本/
│  ├─ team-a/your-ci-name-a.yml
│  ├─ team-b/your-ci-name-b.yml
│  └─ team-c/your-ci-name-c.yml
├─ 第02回_第3章_ワークフロー基礎/
│  ├─ team-a/your-ci-name-a.yml
│  └─ ...
└─ ...
```

- #### 各チームブランチ（例：team-a）
```bash
team-a
├─ README.md
├─ .github/
│  └─ workflows/
│     ├─ manual.yml              # 手動トリガーCIを流す練習用
│     └─ schedule.md             # Cron CIを流す練習用
│     └─ your-ci-name-a.yml      # ワークフロー、実行が終わったら、第0x回/team-x/へ移動
├─ 第01回_第2章_Actionsの基本/
│  └─ team-a/...
├─ 第02回_第3章_ワークフロー基礎/
│  └─ team-a/...
└─ ...
```
**注意**：
1. Workflow ファイル名（例：`your-ci-name.yml`）は自由ですが、**CIファイル内の`name: xxx`には必ずチーム名を含めて**ください ‼️
例：
```yaml
name: my-ci-name-team-a
```
2. `.github/workflows/` 配下には「現在利用中（アクティブ）」の Workflow のみを置き、<br> 実行後に`第0x回/team-x/`などの各チーム用フォルダへ移動してください。<br>
`.github/workflows/`に置いたままだと再び実行される可能性があるため。

3. 一度実行された CI は、GitHub Actions の「All Workflows」に履歴として残ります。
   - `.yml`ファイルを削除しても過去の実行履歴は削除できません（GitHub の仕様です）。
   - 一覧に残っていても、ファイルが存在しなければ今後は実行されませんのでご安心ください。

### 🧱 各回の手順

1. 主催者が当日の章と目的を説明＆前回Quizの振り返り (5~10min)

2. チーム内で交代しながら音読（輪読）

3. モブプロ形式でハンズオン実施
   - `git checkout -b team-<番号(小文字)>`
   - `./github/workflows/` 以下にチーム用 workflow を作成、チームブランチへpush・検証
   - 毎章のコードは [ こちら ](https://github.com/tmknom/example-github-cicd?tab=readme-ov-file) で参照可能
   - **終了後はコードを `章節名/team-<番号>/` に移動**
   - 毎回学習完了したら、Quizを実行して理解度チェック

4. 必要に応じて専用の[ GPT ](https://chatgpt.com/g/g-691dce579db881b8ad74c52b837a7427-cicd4ke-mian-qiang-hui-zhuan-yong-kasutamugpt)または Slack で質問

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