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
