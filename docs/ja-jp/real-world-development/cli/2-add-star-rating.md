---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲームカードを更新し、転送ブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトに初めて変更を加えます。変更は小さくします。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示させ、変更を確認して、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- feature branch で Copilot の会話を開始する。
- エージェントに小さな変更を依頼する。
- `/diff` で変更を確認する。
- 転送ブラウザーでアプリを起動して変更を確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードにはタイトル、カテゴリ、発行元、説明しか表示されていません。最初の作業として、各カードに既存の評価を表示するようエージェントに依頼します。小さく独立した変更なので、初めてのセッションに適しています。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot アプリとは異なり、通常の CLI 会話では、ターミナルで現在チェックアウトしているリポジトリと Git branch を使用します。保存した会話は後から再開できますが、ファイルと branch はディスク上の通常の Git 状態として保持されます。

会話には、プロンプトとエージェントの応答、エージェントがファイルを調査・編集する際のツール操作、`/diff` で確認できる変更の 3 つが表示されます。

## 会話を開始して変更を依頼する

新しい会話を開始して、機能の実装に取りかかります。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. `/new` スラッシュ コマンドを実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使われているファイルを特定して、必要なコードを作成します。これで Copilot CLI を使って新機能を追加できました。

## diff を確認する

小さな変更でも、マージする前に人工知能 (AI) が生成した変更を確認してください。ここでは Copilot CLI 上で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲームカードに評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストが両方の状態を対象にしていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. 確認が終わったら、<kbd>Esc</kbd> を押して diff 画面を閉じます。

> [!NOTE]
> Copilot はほかの生成 AI ツールと同様に確率的に動作するため、コードは毎回異なる場合があります。特定の実装ではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると決めつけず、Copilot に Web サイトを起動してもらい、Codespaces の転送ブラウザーで更新されたユーザー インターフェイス (UI) を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。コードをプロジェクトにマージするプルリクエスト (PR) を作成します。

1. 既定のエージェントに、変更の commit を依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。PR の作成時に Copilot CLI が既存の commit を push し、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックを確認します。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って初めての変更をリリースしました。具体的には、次のことを行いました。

- feature branch で Copilot の会話を開始した。
- エージェントにゲームカードの小さな変更を依頼した。
- `/diff` で変更を確認した。
- 転送ブラウザーでアプリを起動し、星評価を確認した。
- 最初の PR を作成してマージした。

次は、[フィルタリングの issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
