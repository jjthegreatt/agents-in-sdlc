---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-10-21
---

前の演習では、ユーザー体験の改善や機能追加につながる 3 つのタスクを GitHub Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクの進捗を監視することも大切です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するためのツールがあります。この画面では、未完了のプルリクエストがあるエージェントのタスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を与えたりすることもできます。

このレッスンでは、次の内容を確認します。

- エージェント ページを開き、cloud agent のタスクを監視する。
- 実行中のセッションに追加の機能を依頼する。

## シナリオ

チームは、ハイ コントラスト モードに加えてライト モードも追加することにしました。サイトのスタイルと切り替え機能を更新しているところなので、ハイ コントラストだけでなくライト モードも実装するよう、エージェントに指示します。

## GitHub Copilot cloud agent のタスクを確認する

GitHub Copilot cloud agent に割り当てたタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の領域にあるタスクの一覧を確認します。次のような、割り当てたタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更するアプリケーション プログラミング インターフェイス（API）を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような処理が含まれる場合があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP server をセットアップする。
   - 割り当てられたタスクを完了するための各種作業を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実装方法によって異なります。

4. 右側に表示されるプルリクエストの領域を確認します。プルリクエストや変更されたファイルを確認できます。

## cloud agent に追加の指示を与える

実行中のタスクを確認したので、ハイ コントラスト モードに取り組んでいるエージェントに、ライト モードの切り替えも依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

   ![Mission control に表示されたアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。作業開始後は、メッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェント ページで Steer active session while Copilot is working ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り込んだことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送った時点で作業中のステップが終わると、新しい依頼が処理に加わります。

完了まで数分かかる場合があります。ここまでに学んだことを振り返りながら待ちます。

## まとめと次のステップ

このレッスンでは、Copilot agents ページを確認しました。このページは、GitHub Copilot cloud agent のタスクを監視して指示するための中心的な場所です。実行中や完了済みのタスクを追跡し、作業内容をレビューし、進行中のタスクに追加の指示を出せます。

このレッスンでは、次の内容を確認しました。

- mission control とエージェント ページを使って cloud agent のタスクを監視する。
- 実行中のセッションに指示を追加し、機能を拡張する。

Copilot がアクセシビリティ機能に取り組んでいる間に、次は[Copilot が作成したプルリクエストを改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
