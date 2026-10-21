---
title: "レッスン 7 - 品質保証 (QA) agent を作成して使用する"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせた品質保証 (QA) custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright Model Context Protocol (MCP) でブラウザー上のフィルタリングを確認しました。次は、明確な品質保証 (QA) プロセスを定義した custom agent にこれらの機能をまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP tool と連携する仕組みを確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA agent を選択し、フィルタリング issue に照らして報告を確認する。

## シナリオ

Tailspin Toys はプルリクエスト (PR) を開く前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザー上の動作を一貫した方法で確認したいと考えています。custom agent を使えば、この QA プロセスを調整し、再利用可能な報告を作成できます。

## custom agent とは

custom agent は、Markdown プロファイルで定義する Copilot の特化版です。プロファイルには、エージェントの目的、instruction、利用できる tool を記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズには、それぞれ異なる役割があります。リポジトリ instruction はチームの標準を記述します。`quality-checks` skill は繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件の評価と報告にこれらの機能をどう使うかを Copilot に指示します。これらを置き換えるものではなく、別の会話を必要とするものでもありません。

## QA プロファイルを作成する

機能のプルリクエスト (PR) を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、実施するチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、custom agent の作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA の流れと境界が反映されていることを確認します。こうすることで、検証だけを求めているのに不完全なエージェントや範囲の広すぎるエージェントが機能を変更することを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須で、`name` は省略できますが、指定すると表示名が分かりやすくなります。
3. プロファイルの instruction を読み、QA が要件を起点にしてリポジトリ instruction に従い、`quality-checks` skill と Playwright MCP を使うことを確認します。
4. QA が根拠を示し、実装コードを変更する前に確認を求め、変更を commit したり PR を開いたりしないことを確認します。
5. 必要な責務や境界が不足している場合は、続行する前に既定のエージェントへ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクトの agent を読み込みます。プロファイルを作成した後、同じフィルタリングの会話を再開し、QA を選択すると、すでに共有されている issue と計画の決定事項を利用できます。

1. 次のコマンドで agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後の agent は、利用可能な agent の一覧に表示されないことがあります。上記のコマンドで有効化できます。

2. 次のプロンプトで、QA agent に機能の確認を依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent の作業が始まります。
4. 作業完了後、報告を読んで確認します。

## まとめと次のステップ

再利用可能な専門 agent を追加し、その作業を確認しました。このレッスンでは、次のことを行いました。

- custom agent が instruction、skill、MCP tool と連携する仕組みを確認した。
- 再利用可能な品質保証プロファイルを作成して確認した。
- QA agent を選択し、フィルタリング issue に照らして報告を確認した。

実装、skill の更新、QA プロファイル、テスト、検証報告がそろいました。次は、これらを機能 PR にまとめ、Agent Merge を使用します。

## リソース

- [Copilot CLI の custom agent を作成する][create-agents]
- [custom agent の構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
