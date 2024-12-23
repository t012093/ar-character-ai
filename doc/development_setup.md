# 開発環境構築手順

## 必要なソフトウェア

1.  Unityをインストールします (バージョン: 2022.3.16f1)
2.  Pythonをインストールします (バージョン: 3.9以上を推奨)
3.  Git, GitHub Desktopをインストールし、初期設定を行います

## 必要なライブラリ

以下のライブラリをインストールします。

```bash
pip install -r requirements.txt
```

*   OpenAI Pythonライブラリ
*   Flask または FastAPI
*   その他必要なライブラリがあれば追記

## APIキーの取得

1.  OpenAI APIキーを取得します
2.  Azure Cognitive Services for Speech または OpenAI Whisper APIキーを取得します（どちらかを使用する場合）

## Unityの設定

1.  UnityでAR Foundationパッケージをインストールします
2.  UnityでARCore/ARKitの設定を行います

## GitHubの設定

1.  GitHubリポジトリを作成します (ar-character-ai)
2.  .gitignore を設定します (Unityプロジェクト用のテンプレートを適用)

## イシューの取り掛かりから成果物リンクまでの実行手順

1.  GitHubのイシュー一覧を確認し、取り組むべきイシューを選択します。
2.  イシューに関連するドキュメント（例: `doc/planning/competitive_analysis.md`, `doc/requirements_definition.md`, `doc/api_definition.md`）を作成または編集します。
3.  作業内容をドキュメントにまとめます。
4.  ドキュメントの変更をGitにコミットします。
5.  GitHub CLI (`gh`) を使用して、イシューをクローズします。
    ```bash
    gh issue close <issue_number>
    ```
6.  GitHub CLI (`gh`) を使用して、イシューに成果物へのリンクを追記します。
    ```bash
    gh issue edit <issue_number> --body "<成果物名>: [<成果物へのリンク>]"
    ```
    例:
    ```bash
    gh issue edit 3 --body "競合分析: [doc/planning/competitive_analysis.md]"
    ```
7.  必要に応じて、`doc/github_issues.md` にも成果物へのリンクを追記します。
8.  `doc/github_issues.md` の変更をGitにコミットします。
