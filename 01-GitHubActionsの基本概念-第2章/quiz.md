## 📝 第2章：GitHub Actions 基礎理解チェック

以下の設問は『GitHub CI/CD 実践ガイド』第2章の内容をもとに作成されています。  
**単一選択 / 複数選択** が混在しています。  
（※ 複数選択の場合は【複数選択】と明記しています）

---

### Q1. GitHub Actions における **Workflow** とは何ですか？

A. GitHub 上で動作する仮想マシン  
B. イベントをトリガーとして実行される一連の Job 定義  
C. 単一の shell コマンド  
D. GitHub Marketplace に公開されている Action

---

### Q2. GitHub Actions の Workflow は、リポジトリ内のどこに定義しますか？

A. `/.github/actions/`  
B. `/actions/`  
C. `/.github/workflows/`  
D. `/workflows/`

---

### Q3（複数選択）Workflow を実行する **トリガー（on:）** として正しいものはどれですか？

A. `push`  
B. `pull_request`  
C. `merge`  
D. `workflow_dispatch`  
E. `checkout`

---

### Q4. Workflow 内で、**並列または順序制御された処理単位**を表すものはどれですか？

A. Step  
B. Runner  
C. Job  
D. Action

---

### Q5（複数選択）Job と Step の関係について正しいものはどれですか？

A. Job は 1 つ以上の Step から構成される  
B. Step は必ず別々の Runner で実行される  
C. 同一 Job 内の Step は同じ Runner 上で実行される  
D. Step 単体で Workflow を構成できる

---

### Q6. `runs-on: ubuntu-latest` の意味として正しいものはどれですか？

A. 最新の Ubuntu が常にローカル PC にインストールされる  
B. Ubuntu 環境の GitHub-hosted Runner 上で Job を実行する  
C. 任意の OS を自動判別して実行する  
D. Docker コンテナを必ず使用する設定

---

### Q7（複数選択）GitHub-hosted Runner の特徴として正しいものはどれですか？

A. GitHub が管理・提供している  
B. 実行ごとにクリーンな環境が用意される  
C. 永続的にデータが保存される  
D. 利用時間に制限や課金がある場合がある

---

### Q8. 以下のうち、**Action** の説明として最も適切なものはどれですか？

A. Workflow 全体をまとめた YAML ファイル  
B. Job を実行する仮想マシン  
C. 再利用可能な処理単位（Step から呼び出される）  
D. GitHub の UI 上の操作

---

### Q9（複数選択）Workflow の `steps` 内で実行できる処理として正しいものはどれですか？

A. `run:` を使った shell コマンドの実行  
B. `uses:` を使った Action の呼び出し  
C. 別リポジトリへの直接 push  
D. JavaScript / Docker ベースの Action 実行

---

### Q10. `workflow_dispatch` を使用する主な目的は何ですか？

A. PR 作成時のみ Workflow を実行するため  
B. 他の Workflow からのみ呼び出すため  
C. GitHub UI から手動で Workflow を実行するため  
D. Runner を指定するため