---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-10-21
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや領域に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、焦点を絞った指示、ベスト プラクティス、コーディング パターン、ドメイン固有の知識を Copilot に提供します。チームの専門知識を再利用可能なエージェントとして共有できます。たとえば、[Web Content Accessibility Guidelines（WCAG）][wcag] 準拠を促すアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` またはグローバルな `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML front matter と、その後にエージェントの動作、専門性、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントと agent skill の違い

カスタム エージェントと[agent skill][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に区別すると、**カスタム エージェントは作業者、skill はツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一環として skill や別のエージェントを連携させられます。このラボでは、アクセシビリティ用カスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業では、プルリクエストのワークフロー用 skill や、テストを実行・管理する skill を呼び出す場合があります。

> [!NOTE]
> カスタム エージェントを作成する方法に唯一の正解はありません。AI を使うほかの場面と同様に、環境やシナリオに合う方法を見つけるため、テストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次の内容を確認します。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、現在のダーク テーマはテキストと背景色のコントラストが不十分で読みにくいという指摘がありました。デザイン チームは、この問題に対応するため、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。機能の生成にはカスタム エージェントを使います。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、すでに用意されています。Copilot をどのように導くのか理解するため、内容を確認してください。

1. Codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを持つ YAML front matter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む front matter が必要です。

3. 続く各セクションで、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成するときの中核的な責務。
   - アクセシビリティのベスト プラクティス。
   - HyperText Markup Language（HTML）、Cascading Style Sheets（CSS）、JavaScript のコード例。
   - よくある落とし穴や間違い。

## カスタム エージェントに Issue を割り当てる

Mission control は、環境内のすべてのエージェントを扱う場所です。GitHub Copilot cloud agent にタスクを割り当て、進捗を確認し、追加の指示を与えられます。ここでは、ハイ コントラスト モードの実装を依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue の画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て画面を開きます。
9. 使用するカスタム エージェントの一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントと Accessibility が選択された、cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始したことを確認します。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントは、特定のタスクや領域に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントとして共有し、Copilot が特定の種類の作業をより効果的に進められるようにします。

このレッスンでは、次の内容を確認しました。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方。

Copilot がハイ コントラスト モードを実装している間に、次は[エージェント セッションの監視と指示の追加][next-lesson]に進みます。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使う準備][enterprise-custom-agents]

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
