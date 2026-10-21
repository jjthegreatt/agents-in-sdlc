---
title: "レッスン 12 - GitHub Pages でサイトを公開する"
description: "GitHub Copilot app で Pages ワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages で Space Quiz を公開し、誰でもアクセスできる場所を用意します。Copilot はワークフローの準備やデプロイの案内を行いますが、変更をレビューしてサイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次の内容を学びます。

- Copilot に GitHub Pages のデプロイ設定を依頼する。
- マージする前にワークフローとプルリクエストをレビューする。
- デプロイを追跡し、公開されたサイトを確認する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ簡単に試せる方法がありません。単一ファイルのサイトを GitHub Pages で公開すると、サーバーや実行時の依存関係を追加せずに、公開 Uniform Resource Locator（URL）からアクセスできるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![space-quiz のプルリクエストのマージ、Build website・Upload artifact・Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行結果、yourname.github.io/space-quiz/ で公開された Space Quiz の3段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、エージェントにプルリクエストの作成とマージ、リポジトリの Pages 設定変更を依頼します。実行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを確認する

変更内容を確認し、デプロイが実際に完了したことを確かめます。

1. README が、リポジトリにまだ存在しない場合にのみ追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエスト チェックが成功してからマージします。
4. GitHub Actions のデプロイ実行を追跡し、成功するまで確認します。
5. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、提示された手順に従ってください。デプロイが成功し、公開サイトが表示されるまでは完了とみなさないでください。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- Copilot に GitHub Pages のデプロイ設定を依頼しました。
- マージする前にワークフローとプルリクエストをレビューしました。
- デプロイを追跡し、公開されたサイトを確認しました。

Space Quiz を公開し、エージェントの報告だけに頼らず、デプロイを確認しました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
