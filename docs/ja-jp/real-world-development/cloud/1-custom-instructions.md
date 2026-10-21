---
title: "演習 1 - カスタム instruction（cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-10-21
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。特定の方法で進める必要があるタスクや、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[instruction file][instruction-files] を使うと、Copilot に何をしてほしいかだけでなく、どのように進めてほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム instruction と、パス単位で適用する instruction file によって、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加する。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後のプロンプトを実行しません。GitHub Copilot cloud agent は GitHub Issue に対して非同期に作業するため、効果をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、instruction file の影響を確認できます。

## Instruction file

### シナリオ

優れた開発チームである Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のようなものです。

- データ レイヤーには常に単体テストが必要です。
- ユーザー インターフェイス（UI）はダーク モードで、モダンな印象にする必要があります。
- コード内のドキュメントには TypeScript Documentation（TSDoc）の doc comment を使います。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

instruction file を使って、これらのプラクティスに沿ってタスクを実行するために必要な情報を Copilot に渡します。

### カスタム instruction

カスタム instruction を使うと、コンテキストや設定を Copilot に伝え、コーディング スタイルや要件を理解させられます。希望するコーディング規約、ライブラリ、コメントの種類などを指定すると、より適切な提案やコード スニペットを得る助けになります。instruction はリポジトリ全体に対して設定することも、特定のファイル種別を対象に設定することもできます。

instruction file には、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト全体のコンテキストを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別を対象に作成できます。TypeScript や Astro など特定の言語向けのガイドラインや、UI component の作成、単体テストの追加などのタスクに関する指示を記載できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat でのコード生成に使われます。コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と、`applyTo` front matter が付いた `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は、[ほかの標準に基づく instruction file][custom-instructions-support]（AGENTS.md や CLAUDE.md など）にも対応しています。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成について詳しく扱うことは、このワークショップの範囲外です。サンプル プロジェクトには、代表的な方法が示されています。概要は次のとおりです。

- `copilot-instructions.md` には、作成中のものの説明、プロジェクトの構成、全体的なコーディング標準など、プロジェクト レベルの指針を記載します。
- `*.instructions.md` は、単体テスト、Astro component、データ レイヤーなどのファイル種別や、特定のタスクに関する詳しい指示に使います。
- 自然言語で明確に説明します。コードの望ましい例と望ましくない例の両方を示します。

instruction file の書き方にも、AI の使い方にも唯一の正解はありません。試行錯誤しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した instruction file のセットが役立ちます。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction file を生成してもらうこともできます。方法は各 surface によって異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。必要な場合は、各 surface のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、instruction file、カスタム エージェントなどのリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム instruction file を確認する

リポジトリに含まれる instruction file を確認します。中核となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction file があることを確認してください。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、instruction の適用対象を決める glob パターンをリポジトリ ルートからの相対パスで指定します。この例では、`**/*.test.ts` などに一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトの単体テスト作成に特化した instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の既存ファイルへのリンクがあります。大きな instruction セットを小さく再利用できる単位に分割し、コード生成時に Copilot が参考にする例を示せます。ここにあるパスはリポジトリ ルートではなく、instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、プロジェクト全体の指針を示す `.github/copilot-instructions.md` です。コード提案の質を高められるよう、リポジトリのコーディング標準を明記します。

編集を始める前に、作業用ブランチを作成します。cloud agent は Issue の対象ブランチから instruction を読み取りますが、変更を整理して commit するためにブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` をもう一度開きます。
3. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準は記載されていますが、コード内ドキュメントのルールはまだありません。生成した helper に doc comment が含まれていなかったのはそのためです。
4. 既存の標準の直後に、次の Markdown 行を追加して、ファイル コメント ヘッダーと TSDoc の doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction file は、全体的な指針を示すリポジトリ レベルのファイル（`.github/copilot-instructions.md`）にも、特定の言語、ファイル種別、タスクに使う `*.instructions.md` にも作成できます。今回追加した doc comment のようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## instruction の変更を commit、push、マージする

cloud agent は、Issue が対象とするブランチの instruction file を読み取ります。次の演習で Issue を割り当てると、cloud agent は `main` からブランチを作成します。変更を読み取らせるには instruction を `main` にマージしておく必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。これにより、次の演習で Issue を割り当てたときに cloud agent がこの instruction を読み取れるようになります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略し、変更を `main` に直接 commit してもかまいません。ここでは実際のプロジェクトに近い流れを体験するため、ブランチを使います。

## まとめと次のステップ

この演習では、次のことを行いました。

- リポジトリのカスタム instruction と、パス単位で適用する instruction file から Copilot にコンテキストが伝わる仕組みを確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。

ハーネスの後半で cloud agent が作成するプルリクエストを確認すると、この標準が適用されていることがわかります。

次は、[最初の Issue を GitHub Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [カスタム instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [instruction file などのリソースを集めた Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
