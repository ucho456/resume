# 職務経歴書

## 基本情報

|key|value|
|---|---|
|氏名|荒木 裕登|
|ふりがな|あらき ゆうと|
|生年月日|1992年1月1日|
|居住地|東京|
|最終学歴|明治大学食料環境政策学科卒|
|GitHub|<a src="https://github.com/ucho456" target="_blank">https://github.com/ucho456</a>|


## 保有スキル

- JavaScript / TypeScript + Vue.js のフロントエンド開発・設計
- Node.js を使用したバックエンド開発・設計
- Firebase / GCP のサーバーレスアプリケーション開発

## 技術スタック

### 言語

JavaScript, TypeScript, Golang, Ruby

### フレームワーク・その他

Vue.js, Nuxt.js, Node.js, Ruby on Rails, MySQL, NoSQL, Docker, GitHub Actions, Firebase, GCP

<div style="page-break-before:always"></div>

## 職務経歴詳細

## ピクオス株式会社（2019/11〜現在）

### プロジェクト名：動物病院向けクラウド型レセプトサービスの開発

### 技術スタック
JavaScript, TypeScript, Vue.js, Node.js, MySQL, CSS, HTML, GCP

### 概要
従業員の勤怠管理、スケジュール管理、予約、電子カルテ、会計、保険請求、データ分析など動物病院が業務に必要な機能がシームレスに使用できるクラウド型サービスのリプレイスに参画。
キックオフ直後から参画した為、フロント・バックを中心に様々な新機能開発を担当した。

### チーム構成
シニアエンジニア1名と他エンジニア３名（自分含む）の４名体制

### 担当した役割
#### フロントエンド開発
Vue.jsを使用して外注したデザインを元にマークアップからスタイリング、機能作成まで全ての作業を担当。
Googleカレンダーのようなカレンダー機能、canvasを使用した画像の編集機能、TipTapを使用したリッチテキストエディタ機能、Billboardを使用したグラフ機能など多彩な機能を開発。

#### バックエンド開発
Node.jsを使用してREST APIの開発を担当。医院データのCRUD、保険会社やクレジット決済会社とのAPIエンドポイント作成、エクセルからデータのインポート、データ分析、後述するSocket.ioによるリアルタイム通信の実装などを担当。
またTypeScript導入による開発体験の向上を推進。

### 取り組んだ課題
#### 取り組み①
ソケット通信導入によるリアルタイム通信の実現
#### 課題と背景
１つの医院アカウントを複数の医師が使用するというシステムの都合上、データをリアルタイムで共有する必要があった。当時その方法としてフロント側で定期実行処理を行い1分毎にリクエストを発行し、最新データを取得するという仕組みを採用していた為、以下のような問題が発生していた。
1. タイムラグが存在する為、その際にデータの整合性を保つ為にエラーを出す必要があった。
2. 無駄なリクエストが発生しており、通信費用が嵩む。

#### 対応
Web Socketの仕組みを使用できるSocket.ioを導入し、非同期双方向通信を実現した。これによりサーバー側のデータ変更をトリガーにフロントへ更新情報を伝える事が可能になった。導入にあたり工夫した点としてはSocket通信専用のサーバーを立てたこと。Web Socketはステートフル通信であり、Webアプリと接続を維持し続ける必要があるが、セッションの生存時間が長い為、その間はリソースを専有してしまう事になる。それによる負荷がかからないように別サーバーでの実装とした。

#### 成果
1. リアルタイム同期により不必要なエラーを削減
2. 必要最低限のリクエストで済むようになり、通信費削減

#### 取り組み②
非正規化によるDB設計の改善

#### 課題と背景
電子カルテのデータは変更履歴を持つ必要があった。当時のDB設計ではあるカルテを変更する際に、同じテーブル内に新規のレコードを作成し、編集前のレコードのIDをoriginal_idとして持ち、編集前のレコードには削除Flgを立てるというものだった。この設計だと以下のような課題が発生していた。
1. 最新のデータと変更されたデータが同じテーブル内に保存されていた為、都度フィルタリングする手間があった。
2. 同一の削除フラグを削除された場合と編集された場合に使用していた為、複数の意味を持ってしまい、正しく把握するのが難しく不具合の温床となっていた。
3. 正規化されている為、カルテ周辺のデータにも履歴管理の仕組みが必要となり、データ仕様が複雑化していった。

#### 対応
当時個人開発でNoSQLを学習する機会があり、そこで非正規化することで履歴データを設計する手法を学び、業務プロジェクトにも応用できると考え、提案・実装に至った。正規化されたデータベースに重複データを存在させることになるので最初は懐疑的に思われたが、カルテ以外のデータに履歴管理の仕組みが不要になる事、SQL JOINなどが不要になることで通信速度が上がる事、その他データの切り分けが不要になることなどメリットを説明した事で実装することができた。
結果としてシンプルになった事で管理し易くなった。

#### 成果
1. 最新データと履歴データのフィルタリングが不要になった。
2. 編集の際に削除Flgを使用する必要がなくなり、役割が一つのカラムに戻すことができた。
3. 周辺データに履歴管理の仕組みが必要なくなりデータの管理が楽になった。

<div style="page-break-before:always"></div>

## 業務外活動

## 【個人開発】サッカーの選手採点共有サービス

### サービス概要
サッカーの選手採点の記事を作成しTwitterなどのSNSでシェアできるというシンプルなサービスです。SNSらしい基本的な機能（ユーザー認証・記事投稿・いいね・フォローなど）が備わっています。会員登録をしないでも使えるようにしてあるので興味がございましたら触ってみて下さい。

サービスURL：<a src="https://foot-repo.com/" target="_blank">https://foot-repo.com/</a>

リポジトリ：<a src="https://github.com/ucho456/foot-repo" target="_blank">https://github.com/ucho456/foot-repo</a>

### アーキテクチャ
![foot-repo.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2568126/800b1c5a-1eb9-e931-54c2-1060cb1495ea.png)

<div style="page-break-before:always"></div>

### 使用技術
Nuxt.js(SSR, Composition-api), TypeScript, Vuetify, Firebase, Authentication, Firestore, Cloud Storage, Cloud Functions, Cloud Run, Docker, Github Actions

### こだわりポイント
#### 開発のみに集中できる開発環境の構築
WSL2でWindowsにLinux環境を作成し、DockerでNode.jsのコンテナ環境を作成しました。さらにVSCodeのRemote Containerを使用することでデスクトップにあるショートカットワンクリックで仮想コンテナが立ち上がりコードが書けるようになります。
またGithub actionsでCICDパイプラインを作成し、mainブランチにpushするだけで自動デプロイされ、本番環境に反映されるようになっています。
これによりNuxt.js * Firebaseの開発に集中できるようにしました。

#### ページスピード改善
SEOも意識したサービスを作りたかったのでページスピードの改善にも着手しました。ある程度作り終えた時点での、スマートフォンページのスコアは37点ほどでしたが、画像やJS、CSSの不要コード改善・遅延読込などを駆使して90点以上をマークする事ができました。
費用の関係でコールドスタート対策は妥協したのでそこだけアクセスに時間がかかってしまいますが、それ以外は満足いく結果になりました。
![2022-08-16 (2).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2568126/ba5159af-e486-7ffc-75a9-e8dba50b91e4.png)

<div style="page-break-before:always"></div>

## 【個人学習】Golang（2022年8月～）

### 背景
フロントエンド開発・バックエンド開発両方で使用できるという事でJavaScriptを使用した開発を経験してきました。一通りアプリケーションを開発できるようになったと感じているので新しい言語を学びたいと思い、Golangを選択しました。

### 学習過程
1. <a src="https://www.amazon.co.jp/%E5%9F%BA%E7%A4%8E%E3%81%8B%E3%82%89%E3%82%8F%E3%81%8B%E3%82%8B-Go%E8%A8%80%E8%AA%9E-%E5%8F%A4%E5%B7%9D-%E6%98%87/dp/4863541171" target="_blank">基礎からわかる Go言語</a>を使用し、基礎を固めました。A Tour of Go も試しましたが、とっつきずらい部分もあったのでコチラの書籍を購入して勉強しました。

2. Golang * Cloud Functions * LINE Messaging API を使用した天気予報を取得するLINE BOTを作成しました。基礎を学んだ後、簡単でも良いのでアプリを作成して公開したいと思い作成しました。

    QRコード：<a src="https://qr-official.line.me/sid/L/662bwhhi.png" target="_blank">https://qr-official.line.me/sid/L/662bwhhi.png</a>

    リポジトリ：<a src="https://github.com/ucho456/go_weather" target="_blank">https://github.com/ucho456/go_weather</a>

3. CLIでTODOアプリを作成し、公開しました。クロスコンパイル可能で各プラットフォーム向けにバイナリを作成できるという機能を体験する為に作成しました。

    ダウンロードページ：<a src="https://github.com/ucho456/go_todo_cli/releases/tag/v1.0.0" target="_blank">https://github.com/ucho456/go_todo_cli/releases/tag/v1.0.0</a>

    リポジトリ：<a src="https://github.com/ucho456/go_todo_cli" target="_blank">https://github.com/ucho456/go_todo_cli</a>

4. <a src="https://www.amazon.co.jp/%E5%9F%BA%E7%A4%8E%E3%81%8B%E3%82%89%E3%82%8F%E3%81%8B%E3%82%8B-Go%E8%A8%80%E8%AA%9E-%E5%8F%A4%E5%B7%9D-%E6%98%87/dp/4863541171" target="_blank">詳解Go言語Webアプリケーション開発</a>を使用し、GolangでWebサーバーの構築を学習しました。

    リポジトリ：<a src="https://github.com/ucho456/go_todo_cli" target="_blank">https://github.com/ucho456/go_todo_app</a>
