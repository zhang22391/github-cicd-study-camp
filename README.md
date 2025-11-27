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
git commit -m "feat: ch03 workflow hands-on"
git push -u origin team-a
```
- 各チームは `team-a / team-b / team-c …` のように独立したブランチを使用します。
- `main` には 学習メモ (.md)のみを反映し、コードは `team` ブランチに残します。

### 📁 リポジトリ構成イメージ
各チームブランチ（例：team-a）
```bash
team-a
├─ teams/team-a/                # チーム内コード & デモ
│  ├─ ch03/ ...
│  └─ ch04/ ...
└─ .github/workflows/
   └─ team-a-ci.yml             # チームA専用ワークフロー
```
```bash
main
├─ README.md
├─ .github/
│  └─ workflows/
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

4. 必要に応じて専用の[ GPT ](https://chatgpt.com/g/g-691dce579db881b8ad74c52b837a7427-cicd4ke-mian-qiang-hui-zhuan-yong-kasutamugpt)または Slack で質問

5. 各チームごとに学習メモを作成

6. `main` ブランチへ学習メモ（.md）のみ PR

### 🧩 学習メモTemplate

- **パス**：docs/3-継続的インテグレーションの実践(第4章)
- **ファイル名**：teamA_第03回.md
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

### 💬 Feedback & 次回予告

- 各回の最後に、簡単なアンケートまたは口頭でフィードバックを集めます。
- 進行スピードやモブプロ形式の改善提案、GPTの使い勝手や次回に取り上げたい内容など
- 余裕がある場合は全体共有を行い、時間がなければ次回冒頭でまとめて振り返ります。