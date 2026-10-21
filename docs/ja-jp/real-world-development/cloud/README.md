---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-10-21
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot にクラウド上で非同期に作業を任せられます。GitHub で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べ、変更を加えてプルリクエストを作成します。その間、開発者はほかの作業を続けられます。

演習では、cloud agent が従うカスタム instruction を追加し、GitHub Issue を割り当てて実装を任せます。カスタム エージェントを確認して活用し、エージェント ダッシュボードからセッションを監視して指示を追加します。最後に、作成されたプルリクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 内容 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム instruction][ex1] | コンテキスト | cloud agent が従うカスタム instruction を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | Issue を GitHub Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使用する |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視し、指示を追加する |
| [5. 反復的な改善][ex5] | レビュー | プルリクエストをレビューし、Copilot の作業を改善して次の手順を選ぶ |

## 前提条件

このワークショップを始める前に、次の条件を満たしていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランがある GitHub アカウント
- [ ] アカウントまたは組織で GitHub Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していない場合、在学確認済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは Model Context Protocol（MCP）を扱いません。Copilot で MCP server を使う方法は、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## 開始する

**[演習 0: 前提条件から始める][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
