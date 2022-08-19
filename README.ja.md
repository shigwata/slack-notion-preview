# slack-notion-preview

[English](/README.md) | 日本語

## Description

private な Notion リンクが Slack に投稿された際に、それを展開してくれる Slack App です。

![Usage](docs/usage.png)

## Features

- Notion の記事タイトル展開（プロパティは未対応）
- Notion の記事コンテンツ展開
  - Heading
  - Paragraph
  - 箇条書きリスト
  - 番号リスト
  - TODO リスト

## Installation

1. Notion API の Integrations を Internal integrations で作成し、token を取得
2. Slack App 作成
3. slack-notion-preview のデプロイ
4. 2 で作った app に、3 の URL を登録する
5. Slack App の bot ユーザーをチャンネルに招待する
6. 展開したいページで Integration を許可する

### 1. Notion API の Integrations を Internal integrations で作成し、token を取得

[Getting Started](https://developers.notion.com/docs/getting-started) を参考にしながらアクセストークンを取得してください。

### 2. Slack App の作成

1. https://api.slack.com/apps の Create New App からアプリ作成
2. 左メニュー OAuth & Permissions を開き、Scopes で link:write を追加
3. 左メニュー Event Subscriptions を開く
   - App unfurl domains を展開し、 Add Domain で、 `www.notion.so` を入力し、Save Changes
4. 左メニュー Install App を開き、 Install App to Workspace -> Allow
5. OAuth Access Token が表示されるのでメモ (`SLACK_TOKEN`)
6. Basic Information を開き App Credentials の Signing Secret をメモ (`SLACK_SIGNING_SECRET`)

※後で戻ってくるので、Slack App の管理画面は開いたままにしておく。

### 3. slack-notion-preview のデプロイ

Node.js で書かれた Web アプリケーションなので、任意の場所で簡単に動かせますが、Heroku や Google App Engine を利用するのがより簡単でしょう。動作のためには以下の環境変数が必要です。

- `NOTION_TOKEN`: 手順 1 で取得した Notion のアクセストークン
- `SLACK_TOKEN`: 手順 2-5 で取得した Slack App のトークン
- `SLACK_SIGNING_SECRET`: 手順 2-6 で取得したリクエスト署名検証 secret

#### Cloud Run で動かす場合

以下のボタンからデプロイできます。

[![Run on Google Cloud](https://deploy.cloud.run/button.svg)](https://deploy.cloud.run)

※ デプロイしたアプリの URL ルートにアクセスしてもページは表示されませんが仕様なので気にしないでください。

### 4. 2 で作った app に、3 の URL を登録する

- 左メニュー Event Subscriptions を開く
- Request URL に `3でデプロイしたアプリのURL/slack/events` を入力（e.g. https://your-app-uc.a.run.app/slack/events)
- Verified と表示されたら Enable Events を On にして Save Changes

### 5. Slack App の bot をチャンネルに招待する

Bot 名は、左メニューの App Home から確認してください。

### 6. 展開したいページで Integration を許可する

API 経由でのアクセスをするためには、そのページで Integration を許可する必要があります。  
![Grant Integrations](docs/grant-integration.png)

現状ワークスペースレベルで全てのページを許可することはできないようです。  
とはいえ親ページで許可をすれば子孫のページでも適用されるため、サイドバーの各ページで許可をすれば面倒ですが解決は可能です。

これで準備完了です。

## See Also

slack-notion-preview は[MH4GF さんのリポジトリ](https://github.com/MH4GF/notion-deglacer)を参考に作られています。  
特に README の大半をそのまま利用させていただいています。この場をお借りして御礼申し上げます。
