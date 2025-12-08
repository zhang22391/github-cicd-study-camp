## Chapter 02: GitHub Actionsの基本概念

### 2.1 コード

```yaml
name: Hello team-x                # ワークフロー名
on: push                          # イベント（プッシュ時に起動）
jobs:                             # ジョブの定義
  hello:                          # ジョブID
    runs-on: ubuntu-latest        # ランナー（Ubuntuで実行）
    steps:                        # ステップの定義
      - run: echo "Hello, world"  # シェルコマンドの実行
      - uses: actions/checkout@v4 # アクションの呼び出し
```

### 2.2 コード

```yaml
name: Workflow error team-x
on: push
jobs:
  run:
    run-on: ubuntu-latest
    steps:
      - run: date
```


### 2.3 コード

```yaml
name: YAML error team-x
on: push
jobs:
  run:
    runs-on: ubuntu-latest
      steps:                          # インデントが2文字ズレている！
      - run: echo "YAML Syntax Error"
```

### 2.4 コード

- `main` ブランチの`Manual CI` 直接使用してください。
- 手順：
  1. GitHub リポジトリのトップページへ移動
  2. 上部メニューの `Actions` タブをクリック
  3. 左サイドバーで `All Workflows` を選択
  4. `Manual CI` ワークフローを選択
  5. 右上の `Run workflow` ボタンをクリック
  6. 表示されるダイアログでブランチを選択し、`Run workflow` をクリック


### 2.5 コード

```yaml
name: Schedule team-x
on:
  schedule:                # 定期実行イベント
    - cron: '*/15 * * * *' # 15分ごとに起動するcron式
jobs:
  run:
    runs-on: ubuntu-latest
    steps:
      - run: date
```

### 2.7 URL

- Runner-images: https://github.com/actions/runner-images

### 2.8 URL

- Market place: https://github.com/marketplace?verification=verified_creator&type=actions