# 詳細設計書

## プロジェクト名

AI Friend - 愛嬌とユーモアのあるパーソナルAI

## 1. システムアーキテクチャ

システムの全体構成を示す。

```mermaid
graph LR
    subgraph クライアント
        arglass([ARグラス])
        smartphone([スマートフォン])
        pc([PC])
    end
    subgraph Unity
        ar_module([AR表示モジュール])
        ui([UI])
    end
    subgraph Pythonサーバー
        conversation([会話管理モジュール])
        emotion([感情認識モジュール])
        empathy([共感応答モジュール])
        character([キャラクター表現モジュール])
        user_mgmt([ユーザー管理モジュール])
        auth([認証モジュール])
        database(Supabase)
    end
    openai(OpenAI API)
    speech_api(音声認識API)
    tts_api(音声合成API)

    arglass --> ar_module : 音声
    smartphone --> ar_module : 音声
    pc --> ui : 音声

    ar_module --> conversation : テキスト、感情データ
    ui --> conversation : テキスト、感情データ

    conversation --> openai : テキスト
    openai --> conversation : テキスト
    conversation --> speech_api : 音声
    speech_api --> conversation : テキスト
    conversation --> tts_api : テキスト
    tts_api --> conversation : 音声
    conversation --> database : 会話履歴

    emotion --> conversation : 感情データ

    empathy --> conversation : 共感情報

    character --> conversation : キャラクター情報

    user_mgmt --> database : ユーザーデータ
    auth --> database : ユーザーデータ
```

## 2. 各機能の詳細設計

### 2.1 会話機能

* **モジュール:** 会話管理モジュール
* **処理フロー:**
    1. ユーザーの発話を音声認識API（Azure Cognitive Services for Speech または OpenAI Whisper）でテキストに変換する。
    2. 過去の会話履歴をデータベースから取得する。
    3. 現在のユーザーの発話と過去の会話履歴を組み合わせて、OpenAI API (GPTモデル) に送信するプロンプトを生成する。
    4. OpenAI APIから応答を生成する。
    5. 生成された応答をテキストから音声に変換し、ユーザーに提示する。
    6. 現在の会話内容をデータベースに保存する。
* **データ:** 会話履歴データ
* **詳細設計:**
    * **会話履歴の管理:**
        * ユーザーIDごとに会話履歴を保存する。
        * 会話履歴の最大長を設定し、古い履歴を削除する。
    * **プロンプトの設計:**
        * システムプロンプト: AIフレンドのキャラクター設定や応答の指示を記述する。
        * ユーザーの発話: 音声認識APIから変換されたテキスト。
        * 会話履歴: 最新の数件の会話履歴を含める。
    * **応答の生成:**
        * OpenAI APIの`gpt-3.5-turbo`またはそれ以上のモデルを使用する。
        * 応答の多様性を制御するために、temperatureパラメータを調整する。
    * **会話状態の管理:**
        * 特に管理しない。必要に応じて、会話の目的やコンテキストを保持する仕組みを検討する。

### 2.2 感情認識機能

* **モジュール:** 感情認識モジュール
* **処理フロー:**
    1. ユーザーの発話音声を分析し、声質、トーン、内容から感情を抽出する。
    2. 抽出された感情に基づいて、会話管理モジュールに応答の調整を指示する。
* **データ:** 感情分析モデル

### 2.3 共感機能

* **モジュール:** 共感応答モジュール
* **処理フロー:**
    1. 会話内容と感情認識結果を基に、共感的な応答を生成する。
    2. 適切な相槌や質問を生成し、会話の流れを円滑にする。
* **データ:** 共感表現パターン

### 2.4 愛嬌とユーモア

* **モジュール:** キャラクター表現モジュール
* **処理フロー:**
    1. 会話の流れや文脈に応じて、冗談やユーモアのある応答を生成する。
    2. 個性的な口癖や話し方を適用する。
* **データ:** キャラクター設定データ、ユーモア表現パターン

### 2.5 AR表示機能

* **モジュール:** AR表示モジュール
* **処理フロー:**
    1. Unityで作成されたAIフレンドの3Dモデルをロードする。
    2. ARCore/ARKitを用いて、現実空間を認識し、AIフレンドを配置する。
    3. ユーザーの操作に応じて、AIフレンドの移動やアニメーションを制御する。
* **データ:** AIフレンド3Dモデル、アニメーションデータ

### 2.7 ユーザー管理機能

#### 2.7.1 ユーザー登録

* **モジュール:** ユーザー管理モジュール
* **処理フロー:**
    1. ユーザーからメールアドレスとパスワードを受け取る。
    2. メールアドレスの形式とパスワードの強度を検証する。
    3. パスワードをハッシュ化してデータベースに保存する。
    4. ユーザーIDを生成し、クライアントに返す。
* **データ:** ユーザー情報 (user_id, email, password_hash)
* **API設計:**
    * **エンドポイント:** `/register`
    * **メソッド:** POST
    * **リクエスト:** `{ "email": "string", "password": "string" }`
    * **レスポンス:** `{ "user_id": "string" }`

#### 2.7.2 ログイン

* **モジュール:** 認証モジュール
* **処理フロー:**
    1. ユーザーからメールアドレスとパスワードを受け取る。
    2. データベースからユーザー情報を取得する。
    3. 入力されたパスワードとハッシュ化されたパスワードを比較する。
    4. 認証に成功した場合、アクセストークン（JWT）を生成してクライアントに返す。
* **データ:** ユーザー情報 (user_id, email, password_hash)
* **API設計:**
    * **エンドポイント:** `/login`
    * **メソッド:** POST
    * **リクエスト:** `{ "email": "string", "password": "string" }`
    * **レスポンス:** `{ "access_token": "string" }`

#### 2.7.3 ログアウト

* **モジュール:** 認証モジュール
* **処理フロー:**
    1. クライアントからアクセストークンを受け取る。
    2. アクセストークンを無効化する（オプション）。
* **データ:** アクセストークン
* **API設計:**
    * **エンドポイント:** `/logout`
    * **メソッド:** POST
    * **リクエスト:** `Authorization: Bearer <access_token>`
    * **レスポンス:** (204 No Content)

#### 2.7.4 ユーザー情報取得

* **モジュール:** ユーザー管理モジュール
* **処理フロー:**
    1. クライアントからユーザーIDを受け取る。
    2. データベースからユーザー情報を取得する。
    3. ユーザー情報をクライアントに返す。
* **データ:** ユーザー情報 (user_id, email)
* **API設計:**
    * **エンドポイント:** `/users/{user_id}`
    * **メソッド:** GET
    * **リクエスト:** `Authorization: Bearer <access_token>`
    * **レスポンス:** `{ "user_id": "string", "email": "string" }`

### 2.8 データベース設計

* **テーブル:**
    * `users`: ユーザー情報 (user_id, email, password, google_id)
    * `conversations`: 会話履歴 (conversation_id, user_id, timestamp)
    * `messages`: メッセージ内容 (message_id, conversation_id, sender, content, timestamp)

## 3. API設計

### 3.1 会話API

* **エンドポイント:** `/chat`
* **メソッド:** POST
* **リクエスト:** `{ "user_id": "string", "message": "string" }`
* **レスポンス:** `{ "response": "string" }`

### 3.2 感情認識API

* **エンドポイント:** `/analyze_emotion`
* **メソッド:** POST
* **リクエスト:** `Content-Type: audio/wav` またはその他の音声フォーマット
* **レスポンス:** `{ "emotion": "string" }`

## 4. モジュール構成

* **会話管理モジュール:**
    * 会話機能
* **感情認識モジュール:**
    * 感情認識機能
* **共感応答モジュール:**
    * 共感機能
* **キャラクター表現モジュール:**
    * 愛嬌とユーモア
* **AR表示モジュール:**
    * AR表示機能
* **ユーザー管理モジュール:**
    * ユーザー登録
    * ユーザー情報取得
* **認証モジュール:**
    * ログイン
    * ログアウト

## 5. データフロー

### 5.1 会話機能

```mermaid
graph LR
    actor User(ユーザー)
    actor Client(クライアントアプリ)
    actor SpeechAPI(音声認識API)
    actor Emotion(感情認識モジュール)
    actor Server(Pythonサーバー)
    actor OpenAI(OpenAI_API)
    actor TTS(音声合成API)
    database DB(データベース)

    User --> Client : 発話(音声)
    Client --> SpeechAPI : 音声データ
    SpeechAPI --> Client : テキストデータ
    Client --> Emotion : 音声データ
    Emotion --> Client : 感情データ
    Client --> Server : テキストデータ、感情データ
    Server --> OpenAI : テキストデータ、感情データ
    OpenAI --> Server : 応答テキスト
    Server --> TTS : 応答テキスト
    TTS --> Server : 応答音声データ
    Server --> Client : 応答音声データ
    Client --> User : 応答(音声)
    Client --> DB : 会話履歴保存
    DB --> Client : 保存完了
```

1. ユーザーの発話は、クライアントアプリケーションで録音されます。
2. 録音された音声データは、音声認識API（Azure Cognitive Services for Speech または OpenAI Whisper）に送信されます。
3. 音声認識APIは、音声データをテキストデータに変換し、サーバーに送信します。
4. サーバーは、テキストデータをOpenAI API (GPTモデル) に送信し、応答を生成します。
5. 生成された応答は、テキストデータとしてサーバーからクライアントアプリケーションに送信されます。
6. クライアントアプリケーションは、テキストデータを音声合成APIで音声データに変換し、ユーザーに提示します。
7. 会話の内容（ユーザーの発話とAIの応答）は、会話履歴としてデータベースに保存されます。

### 5.2 感情認識機能

```mermaid
graph LR
    actor User(ユーザー)
    actor Client(クライアントアプリ)
    actor EmotionModule(感情認識モジュール)
    actor ConversationModule(会話管理モジュール)

    User --> Client : 発話(音声)
    Client --> EmotionModule : 音声データ
    EmotionModule --> Client : 感情データ
    Client --> ConversationModule : 感情データ
    ConversationModule --> Client : 応答調整指示
```

1. ユーザーの発話は、クライアントアプリケーションで録音されます。
2. 録音された音声データは、感情認識モジュールに送信されます。
3. 感情認識モジュールは、音声データを分析し、ユーザーの感情を抽出します。
4. 抽出された感情データは、会話管理モジュールに送信されます。
5. 会話管理モジュールは、感情データに基づいて、OpenAI APIへのリクエスト内容や応答を調整します。

### 5.3 ユーザー管理機能

```mermaid
graph LR
    actor Client(クライアントアプリ)
    actor Server(Pythonサーバー)
    database Supabase

    subgraph ユーザー登録
        Client --> Server : メールアドレス、パスワード
        Server --> Supabase : ユーザー情報保存
        Supabase --> Server : ユーザーID
        Server --> Client : ユーザーID
    end

    subgraph ログイン
        Client --> Server : メールアドレス、パスワード
        Server --> Supabase : ユーザー情報取得
        Supabase --> Server : ユーザー情報
        Server --> Client : アクセストークン
    end

    subgraph ログアウト
        Client --> Server : アクセストークン
        Server --> Server : アクセストークン無効化 (オプション)
    end

    subgraph ユーザー情報取得
        Client --> Server : アクセストークン
        Client --> Server : ユーザーID
        Server --> Supabase : ユーザー情報取得
        Supabase --> Server : ユーザー情報
        Server --> Client : ユーザー情報
    end
```

1. ユーザー登録時、クライアントアプリケーションからメールアドレスとパスワードがサーバーに送信されます。
2. サーバーは、メールアドレスの形式とパスワードの強度を検証し、パスワードをハッシュ化してデータベースに保存します。
