---
title: "演習 1 - カスタム instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: 前提条件][previous-lesson] · [次の演習: Agent mode →][next-lesson]

生成系人工知能 (AI) を使うときはコンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを利用できるようにします。Copilot に特定のコンテキストを与える方法はいくつかあります。その中心となる [instruction file][instruction-files] を使って、コード生成に関するガイダンスを提供できます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認する。
- *現在の* instruction を使って、フィルタリング用の最初のデータ スライス (publishers helper) を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が反映される様子を確認する。
- instruction の更新とフィルタリングのスライスを `main` に commit し、次の演習で更新済みのガイダンスを使えるようにする。

> [!CAUTION]
> 生成されたコードが、設定した標準と異なる場合があります。Copilot は非決定的です。この演習の目的は、instruction の更新後に振る舞いがどう変わるかを見ることであり、文字単位で同じ出力を得ることではありません。

## Instruction file

### シナリオ

適切な開発チームと同様に、Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには必ず unit test を用意する。
- ユーザー インターフェイス (UI) はダーク モードにして、現代的なデザインにする。
- TSDoc doc comment をコード内に追加する。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加する。

instruction file を使って、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

### Custom instruction

custom instruction を使うと、コンテキストと設定を Copilot に伝えられます。コーディング スタイル、ライブラリ、コメントの形式などを指定することで、より関連性の高い提案やコード スニペットを得られます。リポジトリ全体にも、特定のファイル種別にも instruction を作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべて**のリクエストで Copilot に送られる単一の instruction file です。ほとんどの chat や CLI リクエストに関連するプロジェクト レベルの情報を記述します。たとえば、技術スタック、開発中のプロジェクトの概要、ベスト プラクティス、全体に適用されるガイダンスなどです。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語、UI component や unit test の作成タスクに関するガイダンスを提供できます。

> [!NOTE]
> 統合開発環境 (IDE) では、instruction file は Copilot Chat でのコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction と `applyTo` front matter が付いた `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は [ほかの標準に基づく instruction file][custom-instructions-support] もサポートしています。たとえば AGENTS.md や CLAUDE.md を利用できます。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成方法について詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法が示されています。概要は次のとおりです。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` ファイルには、ファイル種別 (unit test、Astro component、データ レイヤー) または特定のタスク向けのガイダンスを記述します。
- 自然な言葉で、明確なガイダンスとコードの例を示します。

instruction file の作り方にも AI の使い方にも、唯一の正解はありません。試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file があると役立ちます。このプロジェクトでは、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク用の file を用意しています。
>
> Copilot を使って instruction file を生成することもできます。各環境で操作方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。必要な操作は、使用する環境のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、instruction file、custom agent、その他のリソースが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリに用意されている instruction file を確認します。中核となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とのやり取り全般に適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの instruction file を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、instruction の適用対象を決める glob (リポジトリのルートからの相対パス) です。ここでは、`**/*.test.ts` に一致するものなど、TypeScript のテスト ファイルが対象です。
5. このプロジェクトで unit test を作成するための instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの instruction file と既存プロジェクト ファイルへのリンクを確認します。instruction を小さく再利用可能な単位に分け、Copilot がコード生成時に参照する例を示せます。記載されたパスはリポジトリのルートではなく、この instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## 作業用 branch を作成する

変更を加えるため、作業用 branch を作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドで branch を作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する*前に* Copilot Chat を使う

custom instruction の効果を確認するため、現在の instruction を使って最初のプロンプトを送信します。その後、instruction を更新して、同じプロンプトを再送信します。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の手順を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルは演習ごとに異なります。必要な場合は各手順で説明します。

1. 前の演習から開いているエディター タブを閉じ、Copilot が必要なコンテキストだけを参照するようにします。
2. helper の配置先を知らせるため、`src/lib/publishers.ts` を開きます。
3. Chat ビューの agent picker で **Agent** を選び、Copilot にファイルを変更できるようにします。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、helper とテストなどのファイルを更新します。
6. 提案された helper が `db` client を最初の引数に取り、型付きの publishers 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードに TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。この演習で確認したいのは、instruction 更新後に出力の一貫性が高まることです。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コーディング標準を追加し、コード提案の質を向上させます。

1. `.github/copilot-instructions.md` を再び開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング標準は記載されていますが、コード内ドキュメントのルールはありません。そのため、生成された helper に doc comment が含まれませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。これにより、Copilot にファイル コメント ヘッダーと TSDoc doc comment を追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file はリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` のどちらにも作成できます。doc comment のルールのようにプロジェクト全体に適用する標準には、リポジトリ レベルの file が適しています。

## 同じプロンプトを再実行して変化を確認する

instruction に doc comment のルールを追加しました。Copilot Chat に戻り、同じ publishers file を更新して、変更の違いを確認します。

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot の作業対象を明確にします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案された file の先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys Crowd Funding プラットフォームの発行元データアクセス helper。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 関数に、次のような TSDoc doc comment が含まれることを確認します。

   ```typescript
   /**
    * すべての発行元の ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベース client。
    * @returns 発行元 object の配列に解決される promise。
    */
   ```

これで、新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使うコードに適用できました。

## instruction を commit して branch を push する

instruction file もほかのアセットと同様に、通常のソース管理で扱います。更新をリポジトリに commit して branch を push します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、instruction と helper の変更があることを確認します。

   ```bash
   git status
   ```

3. instruction の更新と、Copilot が helper の土台として変更したすべてのファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加の変更が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. branch をリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

branch を push したので、backlog のドキュメント標準に関する issue と関連付けたプルリクエスト (PR) を作成します。Copilot に issue を探してもらい、issue を閉じる PR を作って、マージも依頼します。

> [!NOTE]
> この演習では、Copilot が GitHub 上で操作します。プロジェクト テンプレートでは GitHub の tool がすでに workspace に設定されています。初回の使用時に VS Code から GitHub へのサインインを求められた場合は、画面の指示に従ってください。この接続の仕組み (Model Context Protocol) は後の演習で扱います。

1. codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. 次のプロンプトを使って、instruction file の更新に関する issue を探し、現在の branch から PR を作成するよう依頼します。instruction の更新と、新しい publishers helper の土台の両方を説明に含めます。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められた場合は、コマンドを確認し、問題がなければ **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使ってマージし、ローカルの branch を `main` に戻すよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作や shell command の実行を許可するよう求めたら、コマンドを確認し、問題がなければ **Allow** を選択します。

GitHub Copilot の助けを借りて PR を作成し、マージできました。

## まとめと次のステップ

このプロジェクトの instruction file を Copilot がどのように参照するかを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- *既存の* instruction を使ってコード生成のプロンプトを送り、生成結果を確認した。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加した。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認した。
- instruction の更新と helper の土台を `main` に commit し、次の演習で使えるようにした。

次は [agent mode][next-lesson]で instruction を使い、Copilot にコードベース全体の新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向けの custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
