---
title: "レッスン 4 - カスタム instruction で Copilot を導く"
description: "リポジトリ instruction を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成系人工知能 (AI) を使う際にはコンテキストが重要です。タスクを特定の方法で進めたい場合は、そのガイダンスを Copilot が参照できるようにします。[instruction file][instruction-files] は、望むコードだけでなく、コードの構成方法も伝えます。フィルタリング機能を作成したので、Copilot が使用した instruction を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリ instruction とパス単位の instruction file がエージェントに渡る仕組みを確認する。
- コーディング標準を守るために instruction file を更新する。
- instruction file がコードに与える影響を確認する。

## シナリオ

適切な開発チームとして、Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントではコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` の export 関数には、目的、パラメーター、戻り値を説明する TypeScript Documentation (TSDoc)/JavaScript Documentation (JSDoc) を記載します。該当する場合は、注入可能な `db` 引数も記載します。
- 再利用可能な Astro component では `Props` の契約を記載し、関連コードが変更されたらコメントも最新に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

instruction file を使って、ここで示したプラクティスに沿って作業するための情報を Copilot に渡します。

## Instruction file

custom instruction を使うと、コンテキストや設定を Copilot に提供できます。コーディング スタイル、要件、ライブラリ、コメントの形式を指定することで、より適切な提案やコード スニペットを得られます。リポジトリ全体または特定のファイル種別向けに instruction を作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべて**のリクエストで Copilot に送られます。ほとんどのリクエストに関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用できます。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction と `applyTo` front matter が付いた `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は [ほかの標準に基づく instruction file][custom-instructions-support] もサポートしています。たとえば AGENTS.md や CLAUDE.md を使用できます。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには、すでに複数の instruction file が用意されています。変更を加える前に、既存の内容を確認します。

1. Codespace に戻ります。
2. Codespaces のエディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの説明とコーディング ガイダンスを確認します。この instruction は、このリポジトリの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの instruction file を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドに注目してください。ここには適用対象を決める glob (リポジトリのルートからの相対パス) が設定されています。この例では、`**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。ほかの instruction file (たとえば `unit-tests.instructions.md`) やプロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな instruction セットを小さく再利用しやすい単位に分け、Copilot が参照する例を示せます。記載されたパスはリポジトリのルートではなく、この instruction file からの相対パスです。

## instruction を更新する

既存の instruction にはまだ不足があります。TypeScript に TSDoc コメントを追加するよう、`copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. セクションの最後に次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話開始時にリポジトリ instruction を読み込みます。新しいガイダンスを読み込ませつつ機能のコンテキストを維持するため、保存した後にフィルタリングの会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動保存します。
2. リポジトリの `game-filters-cli` branch で次のコマンドを実行し、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   確認を求められたら、フィルタリングの会話を選び、`game-filters-cli` branch がチェックアウトされていることを確認します。

3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file をどのように取得するかを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリ instruction とパス単位の instruction file がエージェントに渡る仕組みを確認した。
- コーディング標準を守るために instruction file を更新した。
- instruction file がコードに与える影響を確認した。

次は、再利用可能な `quality-checks` skill を[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム instruction を追加する][instruction-files]
- [custom instruction のサポート状況][custom-instructions-support]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
