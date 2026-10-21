---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-10-21
---

[← 前のレッスン: エージェントの管理][previous-lesson]

## 作業内容をレビューする

このラボでは、ユーザー体験の改善や機能追加につながる複数のタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザイン チームが試せる関連ゲーム機能、ハイ コントラスト モードとライト モードを含むアクセシビリティ機能です。変更内容を確認し、必要に応じて Copilot に改善を依頼してから、チームでコードレビューを有効にします。

### シナリオ

生成 AI を使っても、ソフトウェア設計や DevOps の基本は変わりません。生成コードは必ずレビューし、通常の DevOps プロセスに沿って作業します。チームでレビューを有効にする前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

GitHub Copilot cloud agent は非同期に、監督なしでタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みができるのは作業用のブランチだけです。
- cloud agent は GitHub Actions 内で動作し、作業用の一時的な環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。Model Context Protocol（MCP）server も対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエストを確認します。GitHub.com の標準的なプルリクエスト画面を使います。

> [!NOTE]
> プルリクエストに、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は既定で外部リソース（外部 MCP server への呼び出しなど）へのアクセスを制限されているため、これは想定された動作です。必要に応じて、[Copilot cloud agent のファイアウォールを設定または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付きます。作業が完了するまで待ってください。数分かかることがあるため、その間にここまでに学んだことを振り返ってもよいでしょう。

4. 準備ができたら **Files changed** タブを選択し、変更内容をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc doc comment など、追加されたドキュメントを含むコードを確認します。変更内容は実行ごとに異なります。

   TSDoc doc comment、TypeScript の規約、コメント ヘッダーを確認します。これらは演習 2 の冒頭で確認したカスタム instruction file によるものです。

6. 変更をレビューしたら、**Conversation** タブに戻り、下へスクロールします。
7. ワークフローの実行承認を待っていることを示す表示があるか確認します。
8. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されたことを確認します。問題がなければ、単一の Astro アプリ用のプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストでは、GitHub Copilot にコメントを残して変更を依頼できます。チーム メンバーに依頼するときと同じように、プルリクエストのコメントやコードの行に付けたコメントで Copilot をメンションします。Copilot はコメントを確認し、対応するためのセッションを開始します。生成結果は確定的ではないため、依頼文を一つに決めることはできません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を説明するコメント ヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc doc comment を追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを参考に、コードベースに必要な変更を選び、**@copilot** をメンションしたコメントを追加します。
2. **View Session** を選択し、Copilot が更新する様子を確認します。Copilot は変更用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![Back to pull request ボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業完了後、プルリクエストに新しい commit が追加されたことを確認します。
5. **Files changed** タブを選択し、変更内容をレビューします。

必要な結果になるまで、Copilot に追加の変更を依頼できます。満足できたら、ドラフトのプルリクエストをレビュー可能な状態にして `main` ブランチへマージします。

![プルリクエストをレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して、Copilot が作成したプルリクエストをレビューします。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いたプルリクエストを選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 変更に問題がなければ、**Conversation** タブに戻って下へスクロールします。
6. ワークフローが承認待ちであることを示す表示があるか確認します。
7. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが追加され、単一の Astro アプリ用のプロジェクト チェックが成功することを確認します。数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名（例: `copilot/fix-8`）に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を実装しました。前と同じように、Copilot cloud agent に追加の変更を依頼して、反復的に改善できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc doc comment を追加したりできます。この Issue はカスタム instruction を更新する前に割り当てたため、後から追加した標準が適用されていない場合があります。**Conversation** タブで新しいコメントを追加すると、Copilot が次のセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタム エージェントが実装した機能をレビューします。このプルリクエストには、演習 3 で依頼したハイ コントラスト モードと、演習 4 で追加したライト モードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いたプルリクエストを選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付きます。作業が完了するまで待ってください。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認するときは、次の点に注目します。
   - モードを切り替えるユーザー インターフェイス（UI）component
   - ユーザー設定を保持する local storage の利用
   - ハイ コントラスト モードとライト モードのカスケーディング スタイル シート（CSS）またはスタイル変更
   - アクセシビリティ属性（Accessible Rich Internet Applications（ARIA）label、キーボード操作など）
   - モード切り替えを管理する JavaScript / TypeScript コード
6. 変更に問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. ワークフローが承認待ちであることを示す表示があるか確認します。
8. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加され、単一の Astro アプリ用のプロジェクト チェックが成功することを確認します。数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。ターミナルで、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードの切り替えを試します。ページを再読み込みしても選択が保持されることを確認します。

アクセシビリティ用カスタム エージェントが、アクセシビリティのベスト プラクティスに沿って Copilot の実装を導いたことを確認します。懸念や改善点があれば、前のプルリクエストと同様に、コメントで **@copilot** をメンションして変更を依頼できます。

## 任意の演習 — 作業を委任し続ける

実際のバックログ項目を cloud agent に渡し、別の作業に集中する方法を身につけるために、リポジトリに Issue を追加して Copilot に割り当ててみましょう。たとえば次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス helper に入力検証とエラー処理を追加する。

## まとめ

cloud agent ハーネスを完了しました。これらのレッスンを通して、次の内容を行いました。

- リポジトリに含まれるカスタム instruction file を確認し、cloud agent の生成結果への影響を確認できるようにしました。
- Issue を GitHub Copilot cloud agent に割り当て、環境の準備、計画、非同期の作業を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使用し、ハイ コントラスト モードとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティ セッションを監視して追加の指示を出しました。
- cloud agent のプルリクエストをレビューし、**@copilot** をメンションして変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

cloud agent ハーネスを完了しました。引き続き学ぶ場合は、ほかのハーネスで今回の内容を補完できます。

- **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を試します。
- **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、agent skill、カスタム品質保証（QA）エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 連携を使い、ターミナルから 2 つの変更を実装してレビューします。

自分のリポジトリでは、次のような作業を試せます。

- リファクタリングやテスト カバレッジに関する Issue を cloud agent に割り当てる。
- 別の領域に対応するカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を構成する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
