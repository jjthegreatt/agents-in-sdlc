---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が使える自動化機能を追加することもあります。MCP server を使うと、Copilot CLI に組み込まれていないツールやサービスも利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解する。
- Playwright MCP server がまだ利用できない場合は追加する。
- エージェントにブラウザーを操作させ、フィルタリング機能を試す。

## シナリオ

単体テストと end-to-end テストは重要ですが、ユーザー インターフェイス (UI) の更新を検証するには実際に操作する必要があります。ユーザーと同じように Web サイトを利用できるよう Copilot に権限を与えれば、変更が期待どおりに動作するかをより自動的に確認できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、人工知能 (AI) agent が外部ツールやサービスと通信する方法を提供します。MCP を使うと、AI agent は外部のツールやサービスとリアルタイムに通信し、最新情報にアクセスしたり、代わりに操作を実行したりできます。

ツールやリソースには、AI agent と外部サービスをつなぐ MCP server を介してアクセスします。それぞれの MCP server は、AI agent が利用できるツールやリソースを提供します。

よく使われる MCP server の例を示します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP server があります。GitHub の [MCP registry][mcp-registry] で、エコシステム内の MCP を見つけたり、コントリビュートしたりできます。

> [!CAUTION]
> MCP server は、プロジェクトのほかの依存関係と同様に扱ってください。使用前にソース コードを確認し、公開元を検証して、セキュリティ上の影響を検討します。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加して、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を設定済みの場合は、セットアップをスキップして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP server の追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. server の種類が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を試すよう Copilot に依頼する

これまでは機能が期待どおりに動作することを手動で確認しました。次は Copilot が利用できる Playwright MCP server を使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP server で機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は指示しなくても MCP server を判断できます。ただし、使ってほしい server が分かっている場合は、指定すると結果が安定し、トークンの節約にもなります。

2. Copilot がブラウザー上で確認する手順を示す様子を確認します。
3. 報告を読み、すべてが期待どおりに動作するか確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

おめでとうございます。Playwright MCP server を使って、Copilot CLI から実際のブラウザーで機能を確認しました。要点を振り返ると、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学んだ。
- Playwright MCP server が未設定の場合は追加した。
- エージェントにブラウザーを操作させ、フィルタリング機能を試した。

次は、skill とブラウザー ツールを組み合わせた[QA custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
