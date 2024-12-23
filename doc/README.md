# AR Character AI - パーソナルアシスタント

概要
このプロジェクトでは、OpenAI API (GPT-4o-mini/GPT-o3-mini) と AR (拡張現実) 技術を組み合わせた、パーソナルアシスタント AI の開発 を目指しています。ユーザーは、AR 空間上のキャラクターと自然な会話を通じて、情報検索やタスク管理など、様々なアシスタンス機能を利用できます。

このリポジトリは オープンソース版 として公開しています。有料版 では、開発者管理の API キーを用いることで、ユーザーはより手軽にサービスを利用できます。

目標
自然な対話: ユーザーの音声・テキスト入力を理解し、自然な会話形式で応答する AR キャラクター AI を開発。
高度な言語処理: OpenAI API を活用し、高度な言語理解と応答生成を実現。
パーソナライズ: ユーザーのコンテキスト (会話履歴、個人情報など) を考慮し、パーソナライズされたアシスタンスを提供。
オープンソース版と有料版: ２つのバージョンを提供し、幅広いユーザーニーズに対応。
将来的な拡張: キャラクターのカスタマイズや、様々な AR デバイスへの対応を検討。

技術スタック
開発言語: Python 3.x, JavaScript (TypeScript)
AI / 言語処理:
OpenAI API (GPT-4o-mini/GPT-o3-mini): 自然言語処理、応答生成
(必要に応じて) Whisper (音声認識) 等のAPIも追加予定
AR 開発:
Unity (AR Foundation) (バージョン: 2022.3 LTS 以上を推奨)
(必要に応じて) ARKit, ARCore, Vuforia 等
バックエンド: Supabase
PostgreSQL Database: 会話履歴、ユーザープロファイル等のデータ管理
Auth: ユーザー認証
Storage: 音声、画像データ等の保存
Edge Functions: サーバーレス処理 (オープンソース版、有料版)
その他:
Requests (Python): HTTP リクエスト用
Supabase JS SDK: Supabase クライアント用

アーキテクチャ
本システムは、クライアント (AR アプリ)、Supabase Edge Functions、Supabase Database、OpenAI API を組み合わせた構成です。

バージョンによる違い:

オープンソース版: ユーザー自身が OpenAI API キーを入力。
有料版: 開発者管理の API キーを使用 (ユーザー入力不要)。

データの流れ:

オープンソース版:

入力: ユーザーが AR アプリで音声またはテキスト入力。
リクエスト: クライアントが Supabase Edge Functions に、ユーザー API キーを含めてリクエスト送信。
認証: Edge Functions が Supabase Database を参照し、API キーを検証。
応答生成: 有効な場合、Edge Functions が OpenAI API を呼び出し、会話履歴等を基に応答生成。
応答: Edge Functions が応答をクライアントに返却。
出力: クライアントが AR キャラクターを通じて応答を提示。
保存: 会話履歴を Supabase Database に保存。

有料版:

入力: ユーザーが AR アプリで音声またはテキスト入力。
リクエスト: クライアントが Supabase Edge Functions にリクエスト送信。
認証: Edge Functionがユーザー認証情報を検証(認証情報は別途管理)。
応答生成: Edge Functions が開発者側 API キーで OpenAI API を呼び出し、会話履歴等を基に応答生成。
応答: Edge Functions が応答をクライアントに返却。
出力: クライアントが AR キャラクターを通じて応答を提示。
保存: 会話履歴を Supabase Database に保存。

コンポーネント:

AR アプリ (クライアント): ユーザーインターフェース (Unity で開発予定)。
Supabase Edge Functions (Deno):
OpenAI API 連携、応答生成。
DB とのデータやり取り。
API キー認証 (オープンソース版)。
Supabase Database (PostgreSQL): 会話履歴、ユーザープロファイル、API キーを保存。
Supabase Storage: 音声、画像データ等の保存。
OpenAI API: 自然言語処理、応答生成。

開発計画
プロトタイプ開発 (1-2 ヶ月):
OpenAI API 連携検証 (Supabase Edge Functions 使用)。
基本会話機能実装 (テキストベース)。
会話履歴保存機能実装 (Supabase Database)。
オープンソース版の基本機能実装。
AR 機能実装 (2-3 ヶ月):
Unity で AR キャラクター表示、基本アニメーション実装。
音声認識・音声合成統合。
Supabase 基盤構築 (3-4 ヶ月):
Supabase の Database, Edge Functions, Storage, Auth 設定。
認証機能、API キー管理実装。
オープンソース版・有料版それぞれの Edge Functions 実装。
有料版開発:
ユーザー管理、認証、課金システム開発。
開発者側 OpenAI API キー設定。
機能拡張とテスト (4 ヶ月以降):
パーソナライズ機能強化 (ユーザープロファイル活用)。
タスク管理などアシスタンス機能追加。
ユーザーテストでフィードバック収集、改善。

貢献方法
CONTRIBUTING.md を参照してください (今後、ガイドラインを記述予定)。

ライセンス
MIT ライセンス のもとで公開しています。
