---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-10-21
---

[← 前のレッスン: カスタム instruction][previous-lesson]

技術的負債に悩む組織は少なくありません。未解決のセキュリティ上の問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要求などがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などの作業を自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフトのプルリクエストを作成します。時間のかかる作業を委任することで、開発者はより大きな課題に集中できます。

GitHub Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境をカスタマイズする。
- 安全に操作する方法を確認する。
- Issue の範囲を明確にする重要性を知る。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、対処したい技術的負債があります。最初のサイトを作成した請負業者が、ドキュメントをほとんど残していません。まず、アプリケーションのすべての export 関数に TypeScript Documentation（TSDoc）の doc comment を追加します。

デザイン チームはゲームを見つけやすくする改善にも取り組んでいます。各ゲームの詳細ページに、同じカテゴリーに属する関連ゲームを表示したいと考えています。まずは受け入れテストに使える簡単な実装を求めています。優先度の高い Issue がほかにあるため、この作業は後回しになっています。

どちらも優先順位が下がりやすい作業ですが、GitHub Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期で作業している間、開発者は別の作業に取り組み、完了後に変更をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]などの方法で作業を依頼できます。割り当て後、Copilot は進捗を追跡するドラフトのプルリクエストを作成し、環境を準備してタスクに取り組みます。実行中や完了後にセッションを確認でき、提案された解決策をレビューできる状態になると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot を使っても、考える作業を省略できるわけではありません。一見単純な操作でも、詳細を確認すると複雑な場合があります。[cloud agent に依頼するタスクの範囲を明確にしてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に作業し、学びながら試して調整します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム instruction

これまでの演習では、カスタム instruction とその役割を確認しました。カスタム instruction の演習をすでに完了している場合は、`.github/copilot-instructions.md` がどのように働くかを見てきました。まだの場合は、ここで簡単に確認してください。cloud agent に作業を割り当てる前に、リポジトリに含まれる instruction file を読み取り専用で確認し、後で変更への影響を見つけられるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で、次のファイルを開いてください。

- `.github/copilot-instructions.md` - **Code standards** セクションを確認し、TypeScript の規約と TSDoc doc comment の要件に注目します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` front matter を確認します。この instruction は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、生成されたプルリクエストに TSDoc doc comment、TypeScript の規約、コメント ヘッダーが反映されます。これらの instruction file の効果は、後の演習でプルリクエストをレビューするときに確認します。

## GitHub Copilot cloud agent の開発環境を設定する

コードを作成する際は、開発ツールや依存関係を準備するセットアップ スクリプトが必要な場合があります。タスクを割り当てられた Copilot も同様です。cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップ ワークフロー][setup-workflow]を実行し、開発ツールや依存関係を準備できます。

このラボでは、作業の流れと学習を支えるために、セットアップ ワークフローがあらかじめ構成されています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、単一の Astro アプリ用に移行・シード済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。ここでクローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（アプリ全体が Astro + Drizzle/libSQL です）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカルの SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同様に見えますが、次の点に注目してください。

- `copilot-setup-steps` という単一のジョブがあり、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の作業を待たずに、ワークフローが正常に動作するかテストできます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いか、そもそも不足しています。こうした技術的負債は対応されないままになりやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成を支援できるため、この作業を cloud agent に依頼します。後の演習で生成結果をレビューします。

Issue を作成して、cloud agent にドキュメントを追加する作業を割り当てます。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue の画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て画面を開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエストを開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。プルリクエストが一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が作成した TODO リストを確認します。

> [!NOTE]
> Copilot が TODO リストをプルリクエストに表示するまで、数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を構築し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. TODO リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ブラウザーを更新する必要がある場合があります。

16. セッションをスクロールし、Copilot がどのように問題を解決しているかを確認します。コードの調査、状態の把握、計画の検討、コードの作成などの作業が表示されます。

完了まで数分かかる場合があります。cloud agent の主な利点は、非同期で作業を任せ、開発者がほかの作業に取り組めることです。この機能を活用し、別のタスクも cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent には、作業を分担し、開発者が別のタスクに集中できる利点があります。デザイン チームが求める関連ゲーム セクションの実装に長い時間がかからない場合でも、その時間をほかの作業に使えます。次の Issue も cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue の画面を開きます。
4. 空のテンプレートを使うために **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て画面を開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが示されます。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めました。GitHub Copilot cloud agent は開発者と同じように作業するため、常に監視する必要はありません。完了した後でレビューできます。ここからはカスタム エージェントを作成して使う方法を確認します。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期で作業を任せられます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに活用できます。

このレッスンでは、次の内容を確認しました。

- コード生成環境をカスタマイズする。
- 安全に操作する方法を確認する。
- Issue の範囲を明確にする重要性を知る。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、次は[カスタム エージェントの作成と使用][next-lesson]に進みます。cloud agent は Model Context Protocol（MCP）server も利用できます。また、前のモジュールで確認したカスタム instruction も利用できます。

## リソース

- [GitHub Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
