---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の開発ワークフローに取り組みました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されているフィルタリング issue を見つけた。
- 星評価を追加し、転送ブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージした。
- フィルタリング issue から作業を始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードで確認した。
- custom instruction でエージェントを導き、既存の `quality-checks` skill をカスタマイズして、単体テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリングを試した。
- 品質保証 (QA) custom agent を作成して選択し、要件、テスト カバレッジ、skill の結果、ブラウザーでの根拠を確認した。
- フィルタリングの変更全体を確認し、PR に対して Agent Merge を許可した。
- コンテキスト、モデル、共有、cloud への任意の委任に使うスラッシュ コマンドを確認した。

## 作成したもの

このワークショップには、更新した `main` からそれぞれ作成した branch 上で、2 つの PR の節目があります。

1. **星評価**: 既存の `starRating` と、評価なしを示す状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、instruction を更新して機能に適用します。さらに、`quality-checks` の報告をカスタマイズし、QA プロファイルを作成し、関連するテストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話と branch を使いました。ワークショップを簡潔にするため、この作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分自身によるブラウザー確認、MCP を介した Copilot のブラウザー確認を行いました。`quality-checks` skill は単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、それらの結果と要件およびテスト カバレッジの確認をまとめました。

追加するテストは実際の不足を補う必要があります。QA の実行時に新しいテストが不要と判断されることもあります。マージを許可する前にコードと根拠を確認し、変更後は影響を受ける根拠を再確認してください。

## ベスト プラクティス

Copilot に渡すコンテキストとツールは、作業の進め方に影響します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA プロファイルを作成し、MCP server を構成しました。これらのカスタマイズは会話をまたいで再利用し、チームの必要に応じて調整できます。instruction は標準を定め、skill は繰り返し実行する作業を記述し、custom agent は専門的な役割を定義し、MCP server は外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに応じて**モードとモデル**を使い分けます。構築前に方針を検討するには **Plan**、範囲を絞った変更を確認しながら進めるには **Interactive**、明確に定義したタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

コンテキストは環境と同じくらい重要です。何を、なぜ、どのように作りたいかを明確に説明すると、出力が大きく変わります。

## さらに試す

中核的なワークフローは完了しました。Copilot CLI には、次のような機能もあります。

- `/review` で code review agent に変更を分析してもらう。
- `/rubber-duck` で問題について話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並列に調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

ツールを上達させるには、使い続けることが一番です。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリなど、さまざまな用途で試してください。自分の学びをチームと共有し、チームからも学びましょう。そして、いつものようにドキュメントを確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
