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
    * `password` (VARCHAR, NOT NULL)
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

### 2.3 messages

メッセージ内容を格納するテーブル。

* **テーブル名:** `messages`
* **カラム:**
    * `id` (UUID, PRIMARY KEY)
    * `conversation_id` (UUID, NOT NULL, FOREIGN KEY references `conversations`)
    * `sender` (ENUM ['user', 'ai'], NOT NULL)
    * `content` (TEXT, NOT NULL)
    * `timestamp` (TIMESTAMP WITH TIME ZONE, DEFAULT now())

**説明:**

* `id`: メッセージの一意な識別子。
* `conversation_id`: メッセージが属する会話のID。
* `sender`: メッセージの送信者（ユーザーまたはAI）。
* `content`: メッセージの内容。
* `timestamp`: メッセージ送信日時。

**制約:**

* `conversation_id` は `conversations` テーブルの `id` を参照する外部キーです。

## 3. リレーションシップ

* `conversations` テーブルは `users` テーブルと 1対多 の関係を持ちます (1人のユーザーが複数の会話を持つことができます)。
* `messages` テーブルは `conversations` テーブルと 1対多 の関係を持ちます (1つの会話に複数のメッセージが含まれます)。

## 4. 認証

Supabaseの認証機能を利用し、メールアドレスとパスワードによる認証、およびGoogleアカウントによる認証をサポートします。

### 4.1 メールアドレスとパスワード認証

Supabase Authの標準機能を使用します。`users` テーブルの `email` と `password` カラムを利用します。

### 4.2 Googleログイン

Supabase Authのソーシャルログイン機能を使用します。`users` テーブルの `google_id` カラムを利用します。
