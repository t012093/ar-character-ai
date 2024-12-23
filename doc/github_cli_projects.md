GitHub CLI (gh) を使ってコマンドラインから GitHub Projects を操作する
このドキュメントでは、GitHub CLI (gh) を使用して、コマンドラインから GitHub Projects を操作する方法を説明します。Zenn の記事「GitHub Projects の自動化を CLI 経由で実現しよう」の内容をベースに、より詳細な情報と実践的な例を加えて、コマンド操作によるプロジェクト管理を網羅的に解説します。

対象読者

GitHub Projects を使用している開発者、プロジェクトマネージャー
コマンドライン操作に慣れている方
GitHub Actions などと連携して、プロジェクト管理を自動化したい方
前提条件

GitHub アカウント
GitHub CLI (gh) のインストール (https://cli.github.com/)
Git の基本的な知識
目次

GitHub CLI (gh) とは
GitHub Projects の基本概念
プロジェクトの操作
3.1. プロジェクトの作成 (gh project create)
3.2. プロジェクトの一覧表示 (gh project list)
3.3. プロジェクトの表示 (gh project view)
3.4. プロジェクトの編集 (gh project edit)
3.5. プロジェクトの削除 (gh project delete)
プロジェクトボードのアイテム操作
4.1. アイテムの追加 (gh project item-add)
4.1.1 Issue を追加する
4.1.2 Pull Request を追加する
4.1.3 新規アイテム（下書き）を追加する
4.2. アイテムの一覧表示 (gh project item-list)
4.3. アイテムの詳細表示
4.4. アイテムの編集 (gh project item-edit)
4.5. アイテムの削除 (gh project item-delete)
フィールドの操作
5.1 フィールドの追加
5.2 フィールドの取得
5.3 フィールドの更新
5.4 フィールドの削除
GitHub Actions との連携
トラブルシューティング
さらに詳しく知るには
1. GitHub CLI (gh) とは
GitHub CLI (gh) は、GitHub をコマンドラインから操作するための公式ツールです。gh コマンドを使うことで、Issue や Pull Request の作成、リポジトリの管理、GitHub Actions の操作など、様々な GitHub の機能をコマンドラインから直接実行できます。

2. GitHub Projects の基本概念
GitHub Projects は、GitHub 上でタスクやプロジェクトを管理するためのツールです。カンバンボード形式やテーブル形式で Issue や Pull Request を整理し、進捗状況を可視化できます。

プロジェクト: タスクやアイデアを管理するためのコンテナ。
アイテム: プロジェクト内の個々のタスクやアイデア。Issue、Pull Request、または下書きアイテム（Draft Issue）のいずれかになります。
フィールド: アイテムに追加情報を付与するための属性。ステータス、担当者、優先度など、自由にカスタマイズできます。
ビュー: プロジェクト内のアイテムを特定の形式で表示する方法。カンバンボード、テーブル、ロードマップなど、複数のビューを作成できます。
3. プロジェクトの操作
3.1. プロジェクトの作成 (gh project create)
新しいプロジェクトを作成します。

```bash
gh project create --owner <オーナー名> --title <プロジェクト名> --format [board|table]
```
--owner: プロジェクトのオーナー（ユーザー名または組織名）を指定します。
--title: プロジェクトのタイトルを指定します。
--format: プロジェクトのフォーマットを指定します。(オプション: board（カンバンボード）、table（テーブル）。デフォルトは board)
例:

```bash
gh project create --owner my-organization --title "My New Project" --format board
```
プロジェクト作成後の確認
プロジェクト作成後、gh project listコマンドで作成されたことを確認できます。
--ownerで所有者を指定します。指定しない場合、デフォルトの所有者が使われます。

```bash
naoyakusunoki@naoyanoMacBook-Air ept-github-project % gh project list --owner t012093
NUMBER  TITLE                   STATE   ID                  UPDATED AT
5       test-cli                open    PVT_xxx  2 minutes ago
4       ar-character-ai-project open    PVT_kwHOBciMQs4Auyl6 4 days ago
3       Unity-WebGL-Chatbot     open    PVT_kwHOBciMQs4AkYXL 3 weeks ago
2       NPO Project             open    PVT_kwHOBciMQs4AkV6M 3 weeks ago
1       testproject             open    PVT_kwHOBciMQs4AkV58 3 weeks ago
```
3.2. プロジェクトの一覧表示 (gh project list)
オーナーが持つプロジェクトの一覧を表示します。

```bash
gh project list --owner <オーナー名>
```
--owner: プロジェクトのオーナー（ユーザー名または組織名）を指定します。
例:

```bash
gh project list --owner my-organization
```
3.3. プロジェクトの表示 (gh project view)
特定のプロジェクトの詳細を表示します。

```bash
gh project view <プロジェクト番号> --owner <オーナー名>
```
--owner: プロジェクトのオーナー（ユーザー名または組織名）を指定します。
<プロジェクト番号> プロジェクトの番号を指定します。
例:
プロジェクト番号は、gh project listで確認できます。

```bash
gh project view 4 --owner t012093
```
3.4. プロジェクトの編集 (gh project edit)
プロジェクトの設定を編集します。

```bash
gh project edit <プロジェクト番号> --owner <オーナー名> --title <新しいタイトル> --description <新しい説明> --format [board|table] --public [true|false] --readme <新しいREADME>
```
--owner: プロジェクトのオーナー（ユーザー名または組織名）を指定します。
--title: プロジェクトの新しいタイトルを指定します。
--description: プロジェクトの新しい説明を指定します。
--public: プロジェクトを公開するかどうかを指定します。(オプション: true（公開）、false（非公開）。デフォルトはリポジトリの設定に従います)
--format: プロジェクトのフォーマットを指定します。(オプション: board、table)
--readme: プロジェクトの新しい README を指定します。
例:

```bash
gh project edit 4 --owner t012093 --title "My Updated Project" --description "This is the updated description."
```
3.5. プロジェクトの削除 (gh project delete)
プロジェクトを削除します。

```bash
gh project delete <プロジェクト番号> --owner <オーナー名>
```
--owner: プロジェクトのオーナー（ユーザー名または組織名）を指定します。
<プロジェクト番号> プロジェクトの番号を指定します。
例:

```bash
gh project delete 4 --owner t012093
```
4. プロジェクトボードのアイテム操作
4.1. アイテムの追加 (gh project item-add)
プロジェクトボードにアイテムを追加します。アイテムとして、Issue、Pull Request、または新規の下書きアイテムを追加できます。

4.1.1 Issue を追加する
既存のIssueをプロジェクトに追加するには、IssueのコンテンツIDが必要です。コンテンツIDは以下のコマンドで確認します。

```bash
gh api graphql -f query='
  query($issueNumber: Int!) {
    repository(owner: "your_username", name: "your_repository_name") {
      issue(number: $issueNumber) {
        id
      }
    }
  }
' -f issueNumber=1
```
your_usernameにはあなたのユーザーネーム
your_repository_nameにはリポジトリ名
issueNumber=1には追加したいIssueの番号 を入力してください。 出力から、idを確認します。この場合、I_kwDOJa7As85VTPhzです。
<!-- end list -->

```bash
gh project item-add <プロジェクト番号> --owner <オーナー名> --content-id <IssueのGraphQL ID>
```
--owner: プロジェクトのオーナー（ユーザー名または組織名）を指定します。
<プロジェクト番号>: プロジェクトの番号を指定します。
--content-id: 追加する Issue の GraphQL ID を指定します。
例：

```bash
gh project item-add 4 --owner t012093 --content-id I_kwDOJa7As85VTPhz
```
4.1.2 Pull Request を追加する
Pull Request も Issue と同様の手順で追加できます。まず、以下のコマンドでコンテンツIDを調べます。

```bash
gh api graphql -f query='
  query($prNumber: Int!) {
    repository(owner: "your_username", name: "your_repository_name") {
      pullRequest(number: $prNumber) {
        id
      }
    }
  }
' -f prNumber=<Pull Request番号>
```
your_username は、あなたのユーザー名または組織名に置き換えてください。
your_repository_name は、リポジトリ名に置き換えてください。
prNumber は、Pull Request 番号に置き換えてください。
その後、gh project item-add コマンドで追加します。

```bash
gh project item-add <プロジェクト番号> --owner <オーナー名> --content-id <Pull RequestのGraphQL ID>
```
4.1.3 新規アイテム（下書き）を追加する
```bash
gh project item-add <プロジェクト番号> --owner <オーナー名> --title <タイトル>
```
--owner: プロジェクトのオーナー（ユーザー名または組織名）を指定します。
<プロジェクト番号>: プロジェクトの番号を指定します。
--title: 新規アイテムのタイトルを指定します。
--body: 新規アイテムの本文を指定します。(オプション)
例:

```bash
gh project item-add 4 --owner t012093 --title "新しいタスク" --body "タスクの詳細説明"
```
4.2. アイテムの一覧表示 (gh project item-list)
プロジェクトボード内のアイテムを一覧表示します。

```bash
gh project item-list <プロジェクト番号> --owner <オーナー名>
```
--owner: プロジェクトのオーナー（ユーザー名または組織名）を指定します。
<プロジェクト番号>: プロジェクトの番号を指定します。
例:

```bash
gh project item-list 4 --owner t012093
```
オプション:

--limit <数値>: 取得するアイテムの最大数を指定します（デフォルトは 30）。
--format json: 出力形式を JSON にします。
-q, --jq expression: jq 式を使って、JSON 出力をフィルタリングします。
-t, --template string: Go テンプレートを使って、出力形式をカスタマイズします。
4.3. アイテムの詳細表示
特定のアイテムの詳細情報を取得するには、gh project item-list コマンドで --format json オプションを指定して JSON 形式で出力し、jq コマンドを使って必要な情報を抽出します。

例：アイテム番号 1 の詳細情報を取得する

```bash
gh project item-list 4 --owner t012093 --format json | jq '.[] | select(.number == 1)'
```
4.4. アイテムの編集 (gh project item-edit)
注意: 現時点では、gh project item-edit コマンドはフィールドの値の編集のみをサポートしています。アイテムのタイトルや本文などを編集することはできません。

アイテムのフィールドの値を編集します。

```bash
gh project item-edit <プロジェクトアイテム番号> --owner <オーナー名> --field-id <フィールドID> --value <新しい値>
```
--owner: プロジェクトのオーナー（ユーザー名または組織名）を指定します。
<プロジェクトアイテム番号>: 編集したいアイテムの番号を指定します。
--field-id: 編集したいフィールドの ID を指定します。
--value: フィールドに設定する新しい値を指定します。
--number: (数値フィールドの場合) フィールドに設定する新しい数値を指定します。
--date: (日付フィールドの場合) フィールドに設定する新しい日付を YYYY-MM-DD 形式で指定します。
--single-select-option-id: (単一選択フィールドの場合) フィールドに設定する新しいオプションの ID を指定します。
--iteration-id: (イテレーションフィールドの場合) フィールドに設定する新しいイテレーションの ID を指定します。
フィールドID、および選択肢がある場合の選択肢IDの確認方法
フィールドIDを確認するには、まず、プロジェクトのIDを確認します。

```bash
gh project list --owner t012093
```
```
NUMBER  TITLE                   STATE   ID                  UPDATED AT
5       test-cli                open    PVT_xxx  2 minutes ago
4       ar-character-ai-project open    PVT_kwHOBciMQs4Auyl6 4 days ago
3       Unity-WebGL-Chatbot     open    PVT_kwHOBciMQs4AkYXL 3 weeks ago
2       NPO Project             open    PVT_kwHOBciMQs4AkV6M 3 weeks ago
1       testproject             open    PVT_kwHOBciMQs4AkV58 3 weeks ago
```
対象プロジェクトのIDがPVT_kwHOBciMQs4Auyl6であることがわかりました。
次に、対象プロジェクトのフィールドIDを以下のコマンドで確認します。

```bash
gh api graphql -f query='
  query($project_id: ID!) {
    node(id: $project_id) {
      ... on ProjectV2 {
        fields(first: 100) {
          nodes {
            ... on ProjectV2Field {
              id
              name
            }
            ... on ProjectV2IterationField {
              id
              name
              configuration {
                iterations {
                  id
                  startDate
                }
              }
            }
            ... on ProjectV2SingleSelectField {
              id
              name
              options {
                id
                name
              }
            }
          }
        }
      }
    }
  }
' -f project_id=PVT_kwHOBciMQs4Auyl6
```
project_idに対象のプロジェクトIDを入力してください。

出力例

```json
{
  "data": {
    "node": {
      "fields": {
        "nodes": [
          {
            "id": "PVTF_lADOACl76s4Auyl6zgHwYTU",
            "name": "Title"
          },
          {
            "id": "PVTF_lADOACl76s4Auyl6zgHwYTk",
            "name": "Assignees"
          },
          {
            "id": "PVTF_lADOACl76s4Auyl6zgHwYTo",
            "name": "Status",
            "options": [
              {
                "id": "f75ad846",
                "name": "Todo"
              },
              {
                "id": "47fc9ee4",
                "name": "In Progress"
              },
              {
                "id": "98236657",
                "name": "Done"
              }
            ]
          },
          {
            "id": "PVTF_lADOACl76s4Auyl6zgLNv84",
            "name": "Repository"
          },
          {
            "id": "PVTF_lADOACl76s4Auyl6zgLZH-4",
            "name": "Milestone"
          },
          {
            "id": "PVTF_lADOACl76s4Auyl6zgLh5lM",
            "name": "test"
          },
          {
            "id": "PVTF_lADOACl76s4Auyl6zgL065U",
            "name": "Priority",
            "options": [
              {
                "id": "0564c876",
                "name": "High"
              },
              {
                "id": "58d54455",
                "name": "Medium"
              },
              {
                "id": "329e5496",
                "name": "Low"
              }
            ]
          },
          {
            "id": "PVTF_lADOACl76s4Auyl6zgL65sY",
            "name": "Syllabus",
            "options": [
              {
                "id": "75d79553",
                "name": "A"
              },
              {
                "id": "b67f47e6",
                "name": "B"
              },
              {
                "id": "d9f57731",
                "name": "C"
              }
            ]
          },
          {
            "id": "PVTF_lADOACl76s4Auyl6zgMCe_c",
            "name": "Reviewers"
          },
          {
            "id": "PVTF_lADOACl76s4Auyl6zgMUp4w",
            "name": "Linked pull requests"
          },
          {
            "id": "PVTF_lADOACl76s4Auyl6zgMli2A",
            "name": "Iteration",
            "configuration": {
              "iterations": [
                {
                  "id": "67d5589d",
                  "startDate": "2023-11-19"
                },
                {
                  "id": "23059454",
                  "startDate": "2023-11-26"
                },
                {
                  "id": "0442745f",
                  "startDate": "2023-12-03"
                },
                {
                  "id": "f9d99795",
                  "startDate": "2023-12-10"
                },
                {
                  "id": "055999b5",
                  "startDate": "2023-12-17"
                },
                {
                  "id": "d9e47994",
                  "startDate": "2023-12-24"
                },
                {
                  "id": "5aa44552",
                  "startDate": "2023-12-31"
                },
                {
                  "id": "c6659715",
                  "startDate": "2024-01-07"
                },
                {
                  "id": "9d789729",
                  "startDate": "2024-01-14"
                },
                {
                  "id": "fe9a6573",
                  "startDate": "2024-01-21"
                }
              ]
            }
          }
        ]
      }
    }
  }
}
```
この出力結果から、各フィールドの id と name、そして選択式フィールドの場合は options の id と name も確認できます。

例えば：

"Status" フィールドの ID は PVTF_lADOACl76s4Auyl6zgHwYTo
"Status" フィールドの "Todo" オプションの ID は f75ad846
"Priority" フィールドの ID は PVTF_lADOACl76s4Auyl6zgL065U
"Priority" フィールドの "High" オプションの ID は 0564c876
"Iteration" フィールドの ID は PVTF_lADOACl76s4Auyl6zgMli2A
"Iteration" フィールドの 2023-11-19 開始のイテレーションの ID は 67d5589d
例：アイテム番号１のステータスを「In Progress」に変更する

```bash
gh project item-edit 1 --owner t012093 --field-id PVTF_lADOACl76s4Auyl6zgHwYTo --single-select-option-id 47fc9ee4
```
1はアイテム番号です。gh project item-list <プロジェクト番号> --owner <オーナー名>で確認できます。
PVTF_lADOACl76s4Auyl6zgHwYTo は "Status" フィールドの ID
47fc9ee4 は "In Progress" オプションの ID
例：アイテム番号１の優先度を「High」に変更する

```bash
gh project item-edit 1 --owner t012093 --field-id PVTF_lADOACl76s4Auyl6zgL065U --single-select-option-id 0564c876
```
PVTF_lADOACl76s4Auyl6zgL065U は "Priority" フィールドの ID
0564c876 は "High" オプションの ID
例：アイテム番号１のイテレーションを 2023-11-19 開始のイテレーションに変更する

```bash
gh project item-edit 1 --owner t012093 --field-id PVTF_lADOACl76s4Auyl6zgMli2A --iteration-id 67d5589d
```
PVTF_lADOACl76s4Auyl6zgMli2A は "Iteration" フィールドの ID
67d5589d は 2023-11-19 開始のイテレーションの ID
注意:

現在、gh project item-edit コマンドでは、テキストフィールド（例：タイトル、説明）や複数選択フィールドの値を直接編集することはできません。これらの値を編集するには、GitHub の Web インターフェースを使用するか、GitHub API を直接操作する必要があります。
4.5. アイテムの削除 (gh project item-delete)
プロジェクトボードからアイテムを削除します。

```bash
gh project item-delete <プロジェクトアイテム番号> --owner <オーナー名>
```
--owner: プロジェクトのオーナー（ユーザー名または組織名）を指定します。
<プロジェクトアイテム番号>: 削除したいアイテムの番号を指定します。
例:

```bash
gh project item-delete 1 --owner t012093
```
注意:

このコマンドは、プロジェクトボードからアイテムを削除するだけで、Issue や Pull Request 自体は削除しません。
5. フィールドの操作
プロジェクトのフィールドを追加、取得、更新、削除できます。

5.1 フィールドの追加
Zenn の記事で解説されているのは、プロジェクト作成時にフィールドを追加する方法です。
プロジェクト作成後にフィールドを追加する方法は今後のアップデートで実装される可能性があります。

5.2 フィールドの取得
4.4で解説した方法で、プロジェクトに設定されているフィールドの一覧とそれぞれのフィールドの設定値を確認することができます。

5.3 フィールドの更新
4.4で解説した方法で、プロジェクトに設定されているフィールドの値を更新することができます。

5.4 フィールドの削除
Zenn の記事で解説されているのは、プロジェクト作成時にフィールドを削除する方法です。
プロジェクト作成後にフィールドを削除する方法は今後のアップデートで実装される可能性があります。

6. GitHub Actions との連携
gh コマンドは GitHub Actions のワークフローから呼び出すことができます。これにより、Issue や Pull Request の作成、更新、マージなどのイベントをトリガーとして、プロジェクトボードを自動的に更新するなどのワークフローを実現できます。

例：新しい Issue が作成されたら、自動的にプロジェクトボードに追加する

```yaml
name: Add new issues to project

on:
  issues:
    types: [opened]

jobs:
  add-to-project:
    runs-on: ubuntu-latest
    steps:
      - name: Add issue to project
        run: |
          gh project item-add 4 --content-id ${{ github.event.issue.node_id }} --owner t012093
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
このワークフローでは、新しい Issue が作成されたときに、gh project item-add コマンドを使って、その Issue をプロジェクトボードに追加しています。github.event.issue.node_id は、作成された Issue の GraphQL ID を表す変数です。

7. トラブルシューティング
invalid number: ... エラー: プロジェクト番号ではなく、プロジェクト ID を指定している可能性があります。gh project list コマンドでプロジェクト番号を確認してください。
unknown flag: --project エラー: 古いバージョンの gh コマンドを使用している可能性があります。gh --version でバージョンを確認し、最新版にアップデートしてください。
failed to run git: fatal: not a git repository ... エラー: Git リポジトリのルートディレクトリ以外で gh コマンドを実行している可能性があります。cd コマンドで Git リポジトリのルートディレクトリに移動してから、再度実行してください。
アイテムがプロジェクトボードに反映されない: GitHub 側の問題、キャッシュの問題、権限の問題、実行ユーザーの違いなどが考えられます。「7. トラブルシューティング」の項目と、その前に記載されている「gh project item-list コマンドが成功したように見えるのに、プロジェクトに反映されていない場合のトラブルシューティング」の確認手順を参照してください。
8. さらに詳しく知るには
GitHub CLI の公式ドキュメント: https://cli.github.com/manual/
GitHub Projects のドキュメント: https://docs.github.com/en/issues/planning-and-tracking-with-projects
GitHub Actions のドキュメント: https://docs.github.com/en/actions
GitHub GraphQL API のドキュメント: https://docs.github.com/en/graphql
このドキュメントが、gh コマンドを使って GitHub Projects を効果的に活用する一助となれば幸いです。
