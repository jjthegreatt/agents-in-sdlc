---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で issue に基づくフィルタリングを実装して、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能をプロジェクトに追加しましたが、大きな変更には、より堅実なプロセスが必要です。GitHub Copilot CLI は、組織で使われている既存の流れに沿って、適切なものを適切な方法で構築できるよう設計されています。このレッスンから、一般的なエージェント主導の開発プロセスをたどります。issue をもとに機能を作成し、コードと動作を検証したうえで、プロジェクトにマージします。

> [!NOTE]
> 機能の作業を続ける間は、同じ会話と branch を使います。通常なら、作業対象ごとに別の branch やプルリクエスト (PR) を作成しますが、ここでは中心的な概念に集中できるよう簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を開始する。
- Plan モードで要件を定める。
- Autopilot モードで新機能を実装する。
- コードを確認する。
- 転送ブラウザーで機能を手動検証する。

この機能の作業を続けながら、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズして、Model Context Protocol (MCP) による検証を追加し、品質保証 (QA) agent を作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが拡大し、訪問者はカテゴリと発行元でゲームを絞り込む必要があります。backlog の issue には機能が記載されていますが、カテゴリの組み合わせなど、実装前に合意すべき詳細があります。Plan モードで決定事項を整理してから、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ基本がさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 作業内容を記した issue から始める。
2. 実装計画を作る。
3. コードを作成してレビューする。
4. テストを実行して検証する。
5. 新機能を手動で検証する。
6. プルリクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合、上記の流れに沿っています。

この標準的な方法に従えば、人工知能 (AI) が生成したコードも、定められた要件に合っていることを確認し、手書きのコードと同様に審査できます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、作業を続ける前にユーザーの入力を待ちます。
- **Plan**: エージェントがまず計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から作業を始める

フィルタリング作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止中の場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング作業用の branch を作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot に作業を任せる前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替え、計画を作成するよう Copilot に依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、適切な実装方法を検討して、計画を作成します。

3. フィルタリング機能の動作について質問された場合は、希望に基づいて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 自分の決定と合わない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を書いて確認したら、Autopilot モードを使って実装します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様のラベルが付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が合意した計画に沿って作業し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。Copilot から依存関係の不足やポートの競合を報告された場合は、チェック完了と見なす前にセットアップの問題を解決します。

## 実装を確認して検証する

コードをマージする前に、ほかのコードと同様にレビューします。コードを確認し、サイトを起動して表示を確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を issue と計画で決めた内容に照らし合わせます。
4. コードの確認が終わったら、<kbd>Esc</kbd> を押して diff を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードが適切に見えても、実際に動くとは限りません。前と同様にアプリを起動し、Codespaces の転送ポート経由でサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリ、発行元、計画で合意した組み合わせで絞り込みを試します。
4. リセットと結果なしの場合の動作が、issue と合意した内容に沿っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを Copilot に停止するよう依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を作成しレビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の会話を開始した。
- Plan モードで要件を定めた。
- Autopilot モードで新機能を実装した。
- コードを確認した。
- 転送ブラウザーで機能を手動検証した。

次は、[カスタム instruction を使って][next-lesson]コード生成をさらに掘り下げ、文書化されたプラクティスに従う方法を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
