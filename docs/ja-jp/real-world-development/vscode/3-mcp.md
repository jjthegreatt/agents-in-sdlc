---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: Agent mode][previous-lesson] · [次の演習: Custom agent →][next-lesson]

agent mode でフィルタリング機能を作成しました。プルリクエスト (PR) を開く前に、ブラウザーで正しく動作することを確認します。アプリを自分で操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、branch を公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP server が新しい tool で Copilot を拡張する仕組みを学ぶ。
- workspace に Playwright MCP server を追加する。
- Copilot にブラウザーでフィルタリング機能を手動テストさせる。
- branch を公開し、フィルタリング作業の PR を作成する。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にある tool を利用できると、さらに強力になります。Model Context Protocol (MCP) は Copilot が外部 tool やサービスと通信するための標準です。

![agent mode が context、大規模言語モデル (LLM)、tool (MCP server と VS Code 拡張機能が提供する tool を含む) とやり取りする仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、人工知能 (AI) agent が外部ツールやサービスと通信する方法を提供します。MCP を使うと、AI agent は外部のツールやサービスとリアルタイムでやり取りできます。これにより、最新情報にアクセスしたり (resources)、代わりに操作を実行したり (tools) できます。

これらの tool と resource には、AI agent と外部サービスをつなぐ MCP server を介してアクセスします。MCP server は、AI agent と外部 tool (既存の API や `npm` package などのローカル tool) の間の通信を管理します。各 MCP server は、AI agent が利用できる tool と resource を提供します。

広く利用されている MCP server の例を示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理するアプリケーション プログラミング インターフェイス (API) にアクセスできます。AI agent は、新しいリポジトリの作成、既存のリポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI agent は、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにもさまざまな tool や resource を利用できる MCP server があります。GitHub が公開する [MCP registry](https://github.com/mcp) では、エコシステム内の MCP を見つけたり、貢献したりできます。

> [!CAUTION]
> セキュリティの観点では、MCP server をプロジェクトのほかの依存関係と同様に扱ってください。使用前にソース コードを慎重に確認し、公開元を検証して、セキュリティ上の影響を検討します。信頼できる MCP server のみを使い、機密リソースや機密操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルでは、この VS Code workspace で利用できる MCP server を設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` server がすでに設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` の設定はプロジェクト テンプレートに含まれているため、前の演習で Copilot は backlog の issue を読み取れました。[リモート GitHub MCP server][remote-github-mcp-server] を使用するため、ローカルにインストールする必要はありません。VS Code は Hypertext Transfer Protocol (HTTP) 経由で接続し、Copilot が初めて tool を利用するときに GitHub で認証します。

## Playwright MCP server を追加する

次に、2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] は、Copilot が操作できるブラウザーを提供します。これを使って機能をテストします。

1. `.vscode/mcp.json` で `github` の隣に `playwright` を追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` server とは異なり、`playwright` は **local** server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、表示ウィンドウなしでブラウザーが動作します。表示用のデスクトップがない codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトでは end-to-end test に Playwright を使用しているため、必要なブラウザーは通常すでにインストールされています。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行します。

## server を起動して信頼する

VS Code は MCP server を必要に応じて起動します。Copilot が最初に server の tool を必要としたとき、VS Code は server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` 項目の上に **Start** 操作が表示されます。選択して server を起動し、設定が正しいことを確認します。
2. VS Code から server を信頼するよう求められた場合は、設定を確認して信頼する選択肢を選びます。

手動での起動は任意です。スキップしても、次の手順で Copilot が tool を必要としたときに VS Code が起動し、信頼の確認を表示します。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作して確認する代わりに、agent がページを開き、移動し、filter を適用して結果を読み取り、報告できます。会話を続けながら機能が期待どおりに動くかをすばやく確認できます。

内部では、Playwright MCP server はスクリーンショットではなく、ページの [accessibility tree][playwright-mcp-server] を使います。そのため agent は、支援技術と同じように、ボタン、リンク、リスト項目などの構造化されラベル付けされた要素を使って動作します。簡単な機能テストを、アクセシビリティの簡易確認にも活用できます。

server を接続し、アプリを起動したら、次のプロンプトでフィルタリング機能を試すよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server 経由でブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件に照らして報告を確認します。問題があれば、追跡質問をしたり、PR を作成する前にコードを修正するよう依頼したりしてください。

> [!NOTE]
> テスト時には、アプリが `http://localhost:4321` で動作している必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。初めて Copilot が Playwright MCP server を使うときは、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## branch を公開してプルリクエストを作成する

機能の動作を確認したので、チームがレビューできるよう PR を開きます。最初に `filtering-vscode` branch を公開します。

1. codespace の **Source Control** パネルを開き、Copilot の変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンで commit message を生成します。

   ![変更されたファイルを表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して branch をリポジトリに push します。

## プルリクエストを作成する

プルリクエストは github.com や GitHub command-line interface (CLI) などから作成できます。ここでは GitHub Copilot に依頼します。Copilot は関連する issue を見つけ、issue に関連付けてプルリクエスト (PR) を作成できます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを始めます。
2. agent picker で **Agent** を選択し、Copilot が GitHub tool を使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を取得して操作を実行できるようにします。GitHub tool を初めて使うときは、GitHub 認証を求められることがあります。画面の指示に従って許可します。
5. Copilot が issue を検索して適切なものを見つけ、PR を作成したことを確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP server が新しい tool で Copilot を拡張する仕組みを学んだ。
- workspace に Playwright MCP server を追加した。
- Copilot にブラウザーでフィルタリング機能を手動テストさせてから公開した。
- branch を公開し、フィルタリング作業の PR を作成した。

MCP server を使って機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティのレビューなどを効率化する [custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
