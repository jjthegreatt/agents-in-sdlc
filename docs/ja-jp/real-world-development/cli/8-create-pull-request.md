---
title: "レッスン 8 - 機能のプルリクエスト (PR) を作成してマージする"
description: "フィルタリング、instruction、skill の更新、品質保証 (QA) プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、品質保証 (QA) プロファイル、テストは、すべて 1 つの branch に保存されています。まとめて確認し、プルリクエスト (PR) を作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常なら、機能、instruction の更新、skill の更新、QA agent を複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルタリングと品質確認の作業を 1 つの会話と branch にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルの自動化方法を理解する。
- 機能全体の変更と検証の根拠を確認する。
- フィルタリングの PR を作成する。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルタリングの開発を通して、Copilot で機能を計画、実装、検証しました。Tailspin Toys は、マージの承認を開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能にするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題（失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じた rebase など）に取り組み、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその作業を任せることもできますが、有効にする前に内容をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

すべてのコードがそろったので、まとめて確認し、PR を作成して、残りを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力して agent ダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、branch protection、merge queue、リポジトリ設定、権限不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、そしてプルリクエストのプロセスなど、開発作業の複数の手順を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルの自動化方法を学んだ。
- 機能全体の変更と検証の根拠を確認した。
- フィルタリングの PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える[Copilot CLI のスラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
