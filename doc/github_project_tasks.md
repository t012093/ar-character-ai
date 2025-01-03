## GitHub Project タスク一覧

ar-character-ai プロジェクトを円滑に進めるために、GitHub Projectで管理すべきタスクを、開発フェーズごとにまとめました。

**プロジェクトの目標:** ARグラス、またはスマホやPCから、可愛らしい、愛嬌とユーモアのあるパーソナルAIをUnityのARで現実に配置し、家族の一員のような、友達のようなAIを作成する。

**GitHub Project の設定例**

* **Project Name:** ar-character-ai
* **Project Template:** Automated Kanban または Automated Kanban with reviews (お好みで)
* **Columns:**
    * To Do: これから着手するタスク
    * In Progress: 現在進行中のタスク
    * Blocked: 何らかの理由で中断しているタスク
    * Review: レビュー待ちのタスク
    * Done: 完了したタスク

**タスク一覧**

**Phase 0: 企画 & 調査**

**[目標・要求の具体化]**
* [ ] ターゲットユーザーのペルソナを作成
* [ ] 競合するAIの調査・分析
* [ ] 要求仕様書を作成
**[技術選定]**
* [ ] 音声認識APIの比較・選定 (Azure Speech vs Whisper)
* [ ] 音声合成APIの比較・選定
* [ ] 感情認識技術の調査 (使用するライブラリ、モデルなど)
* [ ] 使用するARフレームワークの選定 (ARCore, ARKit, ARFoundation, Vuforiaなど)
**[キャラクター設定]**
* [ ] AIフレンドのキャラクター設定を詳細に決める (外見、性格、口調、一人称など)
* [ ] AIフレンドの名前を決める

**Phase 1: 環境構築 & プロトタイプ開発**

**[環境構築]**
* [ ] Unityのインストール (バージョン: 2022.3.16f1)
* [ ] Pythonのインストール (バージョン: 3.9以上を推奨)
* [ ] 必要なライブラリのインストール (requirements.txt を作成し、pip install -r requirements.txt でインストール)
    * [ ] OpenAI Pythonライブラリ
    * [ ] Flask または FastAPI
    * [ ] その他必要なライブラリがあれば追記
* [ ] OpenAI APIキーの取得
* [ ] Azure Cognitive Services for Speech または OpenAI Whisper APIキーの取得（どちらかを使用する場合）
* [ ] UnityでAR Foundationパッケージをインストール
* [ ] UnityでARCore/ARKitの設定
* [ ] Git, GitHub Desktopのインストールと初期設定
* [ ] GitHubリポジトリの作成 (ar-character-ai)
* [ ] README.md の作成 (プロジェクト概要、特徴、インストール手順などを記述)
* [ ] .gitignore の設定 (Unityプロジェクト用のテンプレートを適用)
* [ ] LICENSE ファイルの追加 (例：MIT License)
* [ ] 開発用ブランチ develop の作成

**[プロトタイプ開発]**
* [ ] Unityでシンプルな球体モデルを作成
* [ ] Pythonサーバーのセットアップ (FlaskまたはFastAPI)
* [ ] UnityからPythonサーバーへのHTTPリクエストの実装 (UnityWebRequestを使用)
* [ ] OpenAI APIとの連携機能を実装 (Pythonサーバー側)
* [ ] Unityで会話を表示するUIの作成
* [ ] Unityで音声認識を開始、停止するボタンの作成
* [ ] 音声合成APIを連携し、応答を音声で出力する機能を実装 (Pythonサーバー側)
* [ ] Unityで、音声認識APIを使用して、音声データをテキストに変換する機能を実装 (クライアント側で音声認識する場合)
* [ ] Pythonサーバーで、音声認識APIを使用して、音声データをテキストに変換する機能を実装 (サーバー側で音声認識する場合)
* [ ] UnityからPythonサーバーへ、音声データを送信する機能を実装 (サーバー側で音声認識する場合)
* [ ] OpenAI APIから応答を取得し、UnityのUIに表示する
* [ ] Unityで、OpenAI APIからの応答に応じて、球体を拡大縮小させる機能を実装

**Phase 2: 基本機能開発**

**[会話機能]**
* [ ] 会話履歴をデータベース(Supabase)に保存する機能を実装 (Pythonサーバー側/会話管理モジュール)
* [ ] 会話履歴を考慮した応答生成機能を実装 (Pythonサーバー側/会話管理モジュール、OpenAI APIのプロンプトを工夫)
* [ ] 会話APIの単体テストを実装
**[感情認識機能]**
* [ ] 音声から感情を認識するモジュールを開発 (Pythonサーバー側/感情認識モジュール)
* [ ] 使用する技術の選定 (例：librosa, scikit-learn)
* [ ] 感情モデルの学習データを収集
* [ ] 感情認識モデルのアーキテクチャを決定
* [ ] 感情認識モデルを学習させる
* [ ] 感情認識モデルを評価する（精度、再現率、F値など）
* [ ] 感情認識モデルのパラメータを調整し、精度を向上させる
* [ ] 認識した感情を会話管理モジュールに渡す機能を実装 (Pythonサーバー側/感情認識モジュール)
* [ ] 感情認識APIの単体テストを実装
**[共感応答機能]**
* [ ] 共感表現パターンを定義 (Pythonサーバー側/共感応答モジュール)
* [ ] ユーザーの発言と感情に合わせて、共感的な応答を生成する機能を実装 (Pythonサーバー側/共感応答モジュール)
* [ ] 共感応答の単体テストを実装
**[キャラクター表現 (愛嬌 & ユーモア)]**
* [ ] キャラクターの性格設定を作成 (口調、一人称など) (Pythonサーバー側/キャラクター表現モジュール)
* [ ] ユーモア表現パターンを定義 (Pythonサーバー側/キャラクター表現モジュール)
* [ ] キャラクター設定に基づいた応答生成機能を実装 (Pythonサーバー側/キャラクター表現モジュール、OpenAI APIのプロンプトを工夫)
* [ ] ユーモアのある応答を生成する機能を実装 (Pythonサーバー側/キャラクター表現モジュール)
* [ ] キャラクター表現の単体テストを実装
**[機能結合]**
* [ ] 会話管理モジュールから、共感応答モジュール、キャラクター表現モジュールを呼び出す機能を実装
* [ ] 各機能を結合して、一連の会話フローとして動作することを確認

**Phase 3: AR & UI/UX 強化**

**[AR表示機能]**
* [ ] BlenderでAIフレンドの3Dモデルを作成
* [ ] AIフレンドの3Dモデルにアニメーションを追加 (待機、会話、喜び、悲しみなど)
* [ ] 3DモデルをUnityにインポートし、ARで表示する機能を実装
* [ ] ユーザーが画面をタップしたら、AIフレンドが近づいてくる機能を実装
* [ ] ユーザーが「バイバイ」と言ったら、AIフレンドが手を振る機能を実装
* [ ] その他のユーザー操作とAIフレンドのリアクションの対応を実装
**[UI/UX]**
* [ ] 会話UIのデザインを改善
* [ ] AR表示の操作性を改善
* [ ] ユーザーが使いやすいように、設定画面などを追加

**Phase 4: ユーザー管理**

**[ユーザー管理機能]**
* [ ] メールアドレスの形式とパスワードの強度を検証する機能を実装（ユーザー管理モジュール）
* [ ] パスワードをハッシュ化してデータベースに保存する機能を実装（ユーザー管理モジュール）
* [ ] Googleログインを使用する場合は、専用のタスクを追加
* [ ] Supabase Authを使用して、ユーザー登録、ログイン、ログアウト機能を実装（認証モジュール）
* [ ] ユーザー情報を取得するAPIを実装（ユーザー管理モジュール）
* [ ] ユーザー管理機能の単体テストを実装
* [ ] ユーザー管理機能の結合テストを実装
**[認証機能]**
* [ ] JWTを使用して、APIの認証機能を実装（認証モジュール）
* [ ] 認証機能の単体テストを実装
* [ ] 認証機能の結合テストを実装

**Phase 5: テスト & リリース**

**[テスト]**
* [ ] テスト計画を作成
* [ ] 結合テストを実施
* [ ] システムテストを実施
* [ ] ユーザーテストを実施
**[リリース]**
* [ ] 各プラットフォーム (ARグラス、スマホ、PC) 向けにビルド
* [ ] 各プラットフォーム向けの動作確認
* [ ] リリースノートの作成
* [ ] アプリストアへの申請 (スマホの場合)

**Phase 6: 運用 & 保守**

**[運用]**
* [ ] サーバーの監視
* [ ] ユーザーからのフィードバックの収集
**[保守]**
* [ ] バグ修正
* [ ] 機能改善

**タスク管理のポイント**

* **優先順位:** 優先度高、優先度中、優先度低 などのラベルで管理する。
* **担当者:** 各タスクの担当者を明確にしましょう。
* **見積もり:** 各タスクの工数を見積もり、スケジュールを立てましょう。
* **期日:** 各タスクに期日を設定する。
* **依存関係:** タスク間の依存関係を明確にし、順序立てて進めましょう。
* **進捗確認:** 定期的に進捗を確認し、必要に応じて計画を修正しましょう。

この修正・追加によって、より詳細で抜け漏れのないタスク一覧になったと思います。

このタスク一覧をベースに、GitHubのIssueやProjectsを活用して、開発を進めていきましょう！
応援しています！
