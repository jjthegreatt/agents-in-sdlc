---
title: "レッスン 9 - GitHub Copilot CLI の便利な機能"
description: "コンテキスト管理、モデル選択、セッション共有、クラウドへの任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には操作に使うスラッシュ コマンドが多数あります。高度な機能、内部状態の情報、追加の設定オプションを利用できます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認する。
- `/model` で利用可能なモデルを確認する。
- `/share` でセッションを export または共有する方法を学ぶ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の中核的なワークショップを終えました。ここでは、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクでは、モデルの最大コンテキスト ウィンドウに近づくことがあります。必要に応じて Copilot CLI が自動的に会話を compact します。また、スラッシュ コマンドを使って自分で状態を確認したり compact したりできます。

1. Codespace に戻り、まだ開いていない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在の token 使用量、システム instruction、tool、メッセージ、空き領域に分けられたコンテキストを確認します。
4. 会話を compact します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合、大きな変化がないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に compact します。タイミングを自分で決めたい場合は `/compact` を使います。無関係なタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに強みが異なり、開発者によって好みも違います。Copilot CLI では利用可能なモデルを一覧表示し、使うモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学びを共有すると、人工知能 (AI) ツールの活用も向上します。`/share` コマンドを使うと、セッションを Markdown または HyperText Markup Language (HTML) ファイルに export したり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストールされているバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown export に使う `/share file` など、目的に合う出力先を選びます。
3. export した内容を送信または公開する前に確認します。セッション export にはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有する意図のないリポジトリや会話の内容を公開しないでください。

## 任意: 並列化または委任する

中核的なワークショップはここまでです。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクを subagent に分け、並列に実行できます。
- `/worktree` は別タスク用の独立した Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送り、非同期で作業させます。PR が作成される場合があります。

これらのコマンドは worktree やリモート作業を追加する可能性があるため、任意です。試す場合は、新しく範囲を明確にしたタスクから始め、通常のワークフローで結果を確認してください。非同期 agent の作業を詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定、共有、内部状態の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認した。
- `/model` で利用可能なモデルを確認した。
- `/share` でセッションを export または共有する方法を学んだ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認した。

スラッシュ コマンドはほかにもあります。最後に[ここまでの内容を振り返り][next-lesson]、さらに活用するための次のステップを確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
