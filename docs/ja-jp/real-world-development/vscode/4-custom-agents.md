---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発ワークフロー内の特定のタスクや分野に合わせた専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown file で agent を定義し、対象の作業を効率的に進めるための専用 instruction、ベスト プラクティス、コーディング パターン、専門知識を Copilot に与えます。チームの知識を再利用可能な agent にまとめることもできます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] への準拠を確認するアクセシビリティ agent、セキュアなコーディング プラクティスに従うセキュリティ agent、テスト パターンを維持する testing agent などです。

custom agent は、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には、少なくとも `name` と `description` を含む YAML frontmatter と、agent の振る舞い、専門知識、instruction を定義する Markdown prompt が含まれます。

### custom agent と agent skill の比較

custom agent と [agent skill][agent-skills-concept] には概念上の重なりがあります。どちらも主に Markdown file で定義され、AI に作業方法を伝えます。簡潔に言うと、**custom agent** は作業者で、**skill** はツールです。

custom agent は独自の context window を持ち、作業中に skill や別の agent を調整するよう設計されています。このラボでは、アクセシビリティ custom agent がガイドラインに沿ってサイトをレビュー、更新します。その作業の一環として、pull request のワークフロー用 skill や、テストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> custom agent の作成に唯一の「正しい」方法はありません。AI のほかの使い方と同様に、環境やシナリオに合う方法が見つかるまでテストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにしたいと考えています。ユーザーから、現在のダーク テーマはテキストと背景のコントラストが不十分で読みにくいという意見が寄せられました。この問題に対応するため、デザイン チームはユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。custom agent を使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ custom agent を確認する。
- Copilot Chat でアクセシビリティ agent を使って高コントラスト モードを実装する。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。Copilot をどのように導くかを理解するために内容を確認します。

1. codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しい branch を作成します。演習 3 のフィルタリング プルリクエスト (PR) とは分けて作業します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、custom agent に必須です。

4. 続くセクションを確認します。主な内容は次のとおりです。
   - アクセシブルな Web サイトのコードを生成する際の中核的な責務。
   - アクセシビリティのベスト プラクティス。
   - HyperText Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例。
   - よくある問題や誤り。

## Copilot Chat で custom agent を使う

VS Code では、`.github/agents` にある各 custom agent が Copilot Chat ビュー下部の agent picker に表示されます。custom agent を選択すると、その agent 専用の instruction と tool を使う会話になります。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の手順を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルは演習ごとに異なります。必要な場合は各手順で説明します。

1. Chat ビューの agent picker で **Agent** を選択します。すでに選択されている場合はそのまま進みます。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部の agent picker を選択します。現在の agent が表示されます。既定では **default** です。
3. 使用可能な agent の一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ agent に送ります。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。各編集はファイル パスと diff とともにチャット内に表示されます。
6. 編集が反映されると、**Files changed** の表示に、このセッションで変更されたファイルが反映されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro component、CSS、関連テストなどの実際のファイルを変更します。

次の演習では、作業の進行を確認し、必要に応じて指示を追加します。

## まとめと次のステップ

この演習では、GitHub Copilot の [custom agent][custom-agents] を扱いました。custom agent は、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの知識や標準を再利用可能な agent にまとめ、特定の作業をより効果的に進められるよう Copilot を導くことができます。

次の内容を確認しました。

- custom agent がどのように定義されるか。
- Copilot Chat の agent mode で custom agent を使う方法。

次は、[agent の作業を監視して指示を追加します][next-lesson]。変更を確認しながら、同じセッションでライト モードの切り替えも追加します。

## リソース

- [custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [custom agent の構成][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前の演習: Playwright MCP server で機能をテストする][previous-lesson] | [次の演習: agent を監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
