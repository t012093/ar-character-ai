# データベース構造定義書

## プロジェクト名

AI Friend - 愛嬌とユーモアのあるパーソナルAI

## 1. 概要

本ドキュメントでは、AI Friendプロジェクトで使用するデータベースの構造を定義します。Supabaseを利用し、ユーザー認証機能とアプリケーションデータを格納します。

## 2. テーブル定義

### 2.1 users

ユーザー情報を格納するテーブル。

* **テーブル名:** `users`
* **カラム:**
    * `id` (UUID, PRIMARY KEY)
    * `email` (VARCHAR, UNIQUE, NOT NULL)
    * `password` (VARCHAR) -- Supabase Authの機能でハッシュ化して保存
    * `google_id` (VARCHAR, UNIQUE)
    * `created_at` (TIMESTAMP WITH TIME ZONE, DEFAULT now())
    * `updated_at` (TIMESTAMP WITH TIME ZONE, DEFAULT now())

**説明:**

* `id`: ユーザーの一意な識別子。
* `email`: ユーザーのメールアドレス。メールアドレスによる認証で使用。
* `password`: パスワード。メールアドレスによる認証で使用。
* `google_id`: GoogleアカウントのID。Googleログインで使用。
* `created_at`: レコード作成日時。
* `updated_at`: レコード更新日時。

**制約:**

  * `email` は一意である必要があります。
  * `google_id` は一意である必要があります。

**インデックス:**

* `email` カラムにインデックスを作成。
* `google_id` カラムにインデックスを作成。

### 2.2 conversations

会話履歴を格納するテーブル。

* **テーブル名:** `conversations`
* **カラム:**
    * `id` (UUID, PRIMARY KEY)
    * `user_id` (UUID, NOT NULL, FOREIGN KEY references `users`)
    * `created_at` (TIMESTAMP WITH TIME ZONE, DEFAULT now())

**説明:**

* `id`: 会話の一意な識別子。
* `user_id`: 会話を行ったユーザーのID。
* `created_at`: 会話開始日時。

**制約:**

* `user_id` は `users` テーブルの `id` を参照する外部キーです。

**インデックス:**

* `user_id` カラムにインデックスを作成。

### 2.3 messages

メッセージ内容を格納するテーブル。

* **テーブル名:** `messages`
* **カラム:**
    * `id` (UUID, PRIMARY KEY)
    * `conversation_id` (UUID, NOT NULL, FOREIGN KEY references `conversations`)
    * `sender` (VARCHAR, NOT NULL)
    * `content` (TEXT, NOT NULL)
    * `timestamp` (TIMESTAMP WITH TIME ZONE, DEFAULT now())
    * `emotion` (VARCHAR) -- AIが認識した感情 (喜び, 悲しみ, 怒り, 恐れ, 平常)

**説明:**

* `id`: メッセージの一意な識別子。
* `conversation_id`: メッセージが属する会話のID。
* `sender`: メッセージの送信者（'user' または 'ai'）。アプリケーション側でチェック。
* `content`: メッセージの内容。
* `timestamp`: メッセージ送信日時。
* `emotion`: AIが認識したユーザーの感情 (喜び, 悲しみ, 怒り, 恐れ, 平常)。

**制約:**

* `conversation_id` は `conversations` テーブルの `id` を参照する外部キーです。

**インデックス:**

* `conversation_id` カラムにインデックスを作成。
* `timestamp` カラムにインデックスを作成。

## 3. リレーションシップ

* `conversations` テーブルは `users` テーブルと 1対多 の関係を持ちます (1人のユーザーが複数の会話を持つことができます)。
* `messages` テーブルは `conversations` テーブルと 1対多 の関係を持ちます (1つの会話に複数のメッセージが含まれます)。

## 4. 認証

Supabaseの認証機能を利用し、メールアドレスとパスワードによる認証、およびGoogleアカウントによる認証をサポートします。

### 4.1 メールアドレスとパスワード認証

Supabase Authの標準機能を使用します。`users` テーブルの `email` と `password` カラムを利用します。

### 4.2 Googleログイン

Supabase Authのソーシャルログイン機能を使用します。`users` テーブルの `google_id` カラムを利用します。

## 5. 検討事項

* **users テーブルの拡張:** 将来的に、ユーザーのプロフィール情報（例：名前、アイコン、年齢、性別など）を追加する可能性がある場合は、あらかじめカラムを追加しておくか、別途 `profiles` テーブルを作成して `users` テーブルと1対1で関連付けることを検討してください。
* **messages テーブルの拡張:**
    * **感情情報の追加:** AIが認識したユーザーの感情を保存するカラム（例：`emotion` VARCHAR）を追加することを検討してみてください。これにより、感情に基づいた会話分析や、AIの応答の調整が可能になります。
    * **メッセージタイプの追加:** テキストメッセージだけでなく、画像、音声、スタンプなどのメッセージタイプを扱う可能性がある場合は、`type` (ENUM or VARCHAR) カラムを追加することを検討してください。
    * **メタデータの追加:** 必要に応じて、メッセージに関するメタデータ（例：言語コード、重要度フラグなど）を保存するカラムを追加することも検討できます。
* **会話の状態管理:** 現状では、会話の状態（例：開始、継続中、終了）を管理する仕組みがありません。必要に応じて、`conversations` テーブルに `status` カラムを追加することを検討してください。
* **論理削除の検討:** ユーザーや会話、メッセージを削除する際に、物理的に削除するのではなく、論理削除（例：`deleted_at` カラムを追加して削除日時を記録）する方式を採用することも検討できます。これにより、誤って削除した場合の復旧や、削除されたデータの分析が可能になります。
* **認証情報の扱い:** ドキュメント内で Supabase Auth を利用することに言及されていますが、認証情報の扱いについて、より詳細な記述があると良いでしょう。例えば、JWT (JSON Web Token) を使用するのか、セッション管理はどのように行うのかなど。
* 必要に応じて、論理削除を検討し、テーブルに`deleted_at`カラムの追加を検討してください。
