---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリ更新でも、複数のファイルを変更し、テストを実行する必要がある場合があります。開発者は通常、必要なファイルを探して変更し、テストを実行し、問題をデバッグして、変更漏れのファイルを見つけ、さらに修正します。作業は際限なく続くこともあります。

このようなときに役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、統合開発環境 (IDE) 内でより自律的に作業するよう設計されています。開発者と同じように、既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる種類の作業向けです。agent mode は IDE 内で動作するため、すばやくフィードバックを得て対話できます。cloud agent はチームの同僚のように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を確認します。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを見つけて、複数ファイルにまたがる変更を行う。
- GitHub Copilot Agent Mode が UI とデータ レイヤーにまたがる新機能を実装する。
- Copilot Agent Mode が生成した変更とテストを確認してから、コードベースにマージする。

## シナリオ

ゲーム一覧が大きくなるにつれ、カテゴリや発行元で絞り込みたいというニーズがあります。前の演習で publishers helper を追加しました。ここでは Copilot Agent Mode を使い、残りのデータ レイヤー、ユーザー インターフェイス (UI)、テストを仕上げます。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、現在の機能を理解するため Tailspin Toys の Web サイトを確認します。

この Web サイトは、ボードゲームを扱う開発者向けテーマのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ページはビルド時に静的な HyperText Markup Language (HTML) として生成されます。ページは frontmatter 内で Drizzle のオブジェクト関係マッピング (ORM) を使ってローカル SQLite データベースに直接問い合わせます。別のバックエンド アプリケーション プログラミング インターフェイス (API) やクライアント側 UI framework はありません。再利用可能なデータアクセス helper は `src/lib/` にあり、インタラクティブな処理には標準のドキュメント オブジェクト モデル (DOM) のアプリケーション プログラミング インターフェイス (API) を使った小さな範囲限定の Astro `<script>` を使います。

### Web サイトを起動する

用意されている `npm run dev` task で、単一の Astro 開発サーバーを起動できます。

1. 前の演習の後、`main` に戻っていることを確認して codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、codespace で新しいターミナルを開きます。
3. フィルタリング作業用の branch を作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの起動後、次のような Uniform Resource Locator (URL) を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro server のアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> codespace でターミナルに表示された localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web サーバーを実行している codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **Home Page**: タイトル、画像、説明付きでボードゲームの一覧を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細を表示します。

## Copilot で backlog を確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の手順を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルは演習ごとに異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、機能を追加します。テンプレートからプロジェクトをセットアップしたときに issue backlog が自動作成されています。GitHub Copilot に backlog を表示してもらいましょう。

1. Chat ビューの agent picker で **Agent** を選びます。**Agent** は複数ファイルの変更を計画・実装し、ターミナル コマンドや tool を実行できます。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。作成時に Claude Sonnet 4.5 を使ったため、手順にも記載しています。比較的一貫した体験を得られることを期待していますが、大規模言語モデル (LLM) は確率的に動作するため、提案が手順と異なる場合があります。これは正常な動作です。

> [!NOTE]
> 大規模言語モデル (LLM) の確率的な動作により、Copilot は別の MCP コマンドを使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、issue backlog を表示してもらいます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーにとって役立つと思われる順に項目を並べていることを確認してください。

## instruction file を確認する

コードを生成するよう agent に依頼する前に、作業のコンテキストとして使う instruction file を確認します。Web サイトへの機能追加方法に関する情報が記載された [ユーザー インターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) file を使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。たとえば次の内容があります。
   - アーキテクチャの概要。
   - component 設計、テスト可能性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの instruction file へのリンク。

> [!TIP]
> instruction file から、ほかの instruction file やプロジェクト ファイルを参照できます。パスはその instruction file からの相対パスです。instruction を再利用しやすい小さな単位に分け、例を示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) のフィルタリング ロジックを追加または調整する。
- フィルタリングの動作に関するテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

さらに、コードベースにマージする前にテストを実行して合格させる必要があります。これらを Copilot Agent Mode に依頼します。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI instruction file を追加します。

   ![UI instruction file を追加する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent picker で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. backlog の該当 issue に基づいて機能を実装するよう、次のプロンプトを送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを特定して、変更とテストを始める様子を確認します。

   ![Copilot がプロジェクトを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクとコードベースの複雑さによっては数分かかることがあります。作業中、エディターに **Keep** と **Undo** が表示される場合があります。Copilot が作業を終えると、すべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot から指示されたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に処理が止まったり、一時的にテストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、完了してエラーがないと判断するまで修正します。

   ![Copilot Agent Mode の完了したチャット セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI tool が生成したコードは、必ずレビューしてください。

10. Web サイトを起動しているブラウザーに戻り、新機能を試します。
11. 動作を確認しコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加しました。次の方法を確認しました。

- GitHub Copilot Agent Mode が UI とデータ レイヤーにまたがる新機能を実装する。
- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを見つけて、複数ファイルにまたがる変更を行う。
- Copilot Agent Mode が生成した変更とテストを確認してから、コードベースにマージする。

次は [Playwright MCP server で機能をテストし][next-lesson]、プルリクエスト (PR) を作成します。

### ボーナス演習: ページネーションを実装する

ゲーム一覧の拡大に合わせて、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの実装を依頼してみましょう。プロンプトの作成では、次の点を考慮します。

- 既存の instruction file を使うなど、現在のベスト プラクティスに従う。
- ページサイズをユーザーが選べるようにするか、固定するかなど、ページネーションの設計を決める。
- 希望する実装に必要なガイダンスをプロンプトに含める。
- GitHub Copilot に変更やコンテキストを伝えながら、繰り返し調整する。Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: 用途と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
