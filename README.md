# マッチメイキングキュー Discord Bot

Discord.jsを使用した、サーバーごとに独立したマッチメイキングキューを管理するBotです。

## 機能

### 基本機能
- `/join [rank]` - マッチメイキングキューに参加（ランクを指定）
- `/leave` - マッチメイキングキューから離脱
- `/status` - 現在のキュー状況を確認
- 10人集まると自動的にランクを考慮して2チームに分割
- サーバー（ギルド）ごとに独立したキュー管理

### プレミアム機能
- `/premiummatch` - プレミアムユーザー専用のマッチメイキング
  - `join` - プレミアムキューに参加
  - `leave` - プレミアムキューから離脱
  - `status` - プレミアムキューの状況確認
  - `history` - 自分のマッチ履歴とEloレート確認
- Eloレート計算と履歴保存
- 勝敗結果の記録とEloレートの自動更新
- カスタムボタンでの試合結果入力

## セットアップ方法

1. リポジトリをクローン
```
git clone <リポジトリURL>
```

2. 依存パッケージをインストール
```
npm install
```

3. 環境設定
- `.env-example` ファイルを `.env` にコピー
- Discord開発者ポータルでBotを作成し、トークンを取得
- `.env` ファイルに `BOT_TOKEN` を設定

4. Botを起動
```
npm start
```

## 開発環境での実行

変更を監視して自動再起動するには:
```
npm run dev
```

## Discord Botの作成方法

1. [Discord Developer Portal](https://discord.com/developers/applications)にアクセス
2. 「New Application」をクリック
3. Bot設定で「Add Bot」をクリック
4. Privileged Gateway Intentsセクションで以下を有効化:
   - SERVER MEMBERS INTENT
   - MESSAGE CONTENT INTENT
5. トークンをコピーして`.env`ファイルに貼り付け
6. OAuth2 > URL Generatorで以下を選択:
   - Scopes: bot, applications.commands
   - Bot Permissions: Send Messages, Embed Links, Use Slash Commands
7. 生成されたURLを使ってBotをサーバーに招待

## プレミアム機能の設定

プレミアム機能を使用するには:

1. サーバーで「Premium」という名前のロールを作成
2. プレミアム機能を使用させたいユーザーにこのロールを付与
3. 上記ユーザーは`/premiummatch`コマンドでプレミアム機能を使用可能
4. マッチ終了後にボタンで勝利チームを選択すると、自動的にEloレートが更新・保存されます

## チーム分けシステム

このBotは10人のプレイヤーが集まると、以下のようにチームを自動的に分けます：

### 通常マッチ
1. プレイヤーはランクに基づいてソート（高ランクから低ランクへ）
2. スネークドラフト方式で公平にチーム分け
3. 各チームの平均ランク値を計算
4. チーム分け結果をサーバーのテキストチャンネルに通知

### プレミアムマッチ
1. プレイヤーはEloレートに基づいてソート（高Eloから低Eloへ）
2. スネークドラフト方式で公平にチーム分け
3. 各チームの平均Eloを計算
4. 試合結果入力ボタン付きでチーム分け結果を通知
5. 勝敗結果に基づいてEloレートを更新

## 注意事項

- 通常キューのデータはメモリに保存されるため、Botを再起動するとリセットされます
- プレミアムユーザーのEloデータはJSONファイルに保存され、再起動後も維持されます

将来的な機能拡張として、データベース連携を検討しています #   V A L O - A I  
 