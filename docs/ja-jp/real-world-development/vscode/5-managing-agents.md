---
title: "演習 5 - agent を監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前の演習: Custom agent][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースの調査、変更の提案、ファイルの編集、コマンドの実行を自律的に行います。作業はローカルのマシンでリアルタイムに実行されるため、VS Code の Copilot Chat には、各 tool の呼び出しとファイル編集が表示されます。diff をその場で確認し、変更を個別に採用または取り消し、エディターを離れずに追加のプロンプトで作業を調整できます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムで監視する。
- 提案されたファイル編集を確認して採用する。
- セッションの途中で agent に指示し、高コントラストに加えてライト モードを追加する。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ custom agent は、高コントラスト モードを実装しています。作業中にデザイン チームから追加の要望がありました。既定のテーマが暗すぎると感じるユーザー向けに、ライト モードの切り替えも追加してほしいとのことです。新しい会話を始めず、既存のセッションを使って作業を広げます。

## Copilot の進捗を監視する

Copilot Chat ビューには、agent が調査したファイル、実行したコマンド、提案した編集など、作業の各段階が表示されます。ファイルの編集は diff と **Keep** / **Undo** の操作とともにチャット内に表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. agent の作業中にチャットをスクロールし、各 tool 呼び出しを確認します。ファイルの読み取りや編集、実行を求められるターミナル コマンドなどが表示されます。
3. agent がファイル編集を提案すると、diff がチャット内に表示されます。
   - **Keep** を選択すると変更を採用します。
   - **Undo** を選択すると変更を取り消し、agent にもう一度作業させます。
4. チャット上部の **Files changed** を開くと、このセッションで agent が変更したファイル全体を確認できます。

> [!NOTE]
> tool の実行 (たとえばターミナル コマンド) に許可が必要な場合、agent は一時停止して **Allow** を表示します。内容を確認し、実行してもよいコマンドだけを許可してください。

5. agent が高コントラスト モードの実装を終えるまで待ちます。変更の概要を示す最終メッセージが表示されます。

## ライト モードを追加するよう agent に指示する

高コントラスト モードが実装されたら、同じ会話でライト モードの切り替えを追加します。同じチャット セッションを使うため、agent は以前の作業、変更した component、スタイル、local storage の使い方を把握しています。

1. Copilot Chat の入力欄に、次の追加プロンプトを送ります。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. agent はすでに作成した変更を確認し、ライト モード用の変更を提案します。
3. 高コントラスト モードと同じように、新しい編集をそれぞれ確認し、diff ごとに **Keep** または **Undo** を選択します。
4. agent の方向性が希望と異なる場合 (たとえば、好まない方法で toggle のユーザー インターフェイス (UI) をリファクタリングしようとする場合) は、チャット入力欄の横にある **Stop** ボタンで停止し、説明を補足するメッセージを送ります。

> [!TIP]
> 進行中のセッションに指示を追加すると、最初からやり直すより効率的です。追加のプロンプトを使って、「Cascading Style Sheets (CSS) を簡潔にし、selector を重複させる代わりに CSS variable を使う」のように変更を依頼したり、「toggle UI をリファクタリングせず、既存の設定パネルにライト モードを追加する」のように方向を修正したり、「両方の toggle にキーボード ショートカットも追加する」のように範囲を拡張したりできます。

## 作業中の変更を確認する

変更を commit する前に、agent が編集したすべての内容を確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイルの一覧を確認します。Astro component、style、関連テストなどが表示されているはずです。
3. 変更されたファイルをいくつか開き、diff を確認します。Accessible Rich Internet Applications (ARIA) 属性、キーボード操作、セマンティックな HyperText Markup Language (HTML)、local storage による永続化などのアクセシビリティ パターンが反映されていることを確認します。
4. 問題がなければ、Source Control パネルで変更を stage して commit します。変更は[次の演習][next-lesson]で公開します。

## まとめと次のステップ

Copilot Chat の agent mode を使って、作業中のセッションを監視し、指示を追加しました。ファイル編集を確認しながら採用または取り消し、会話のコンテキストを失わずに追加のプロンプトで作業を拡張しました。

この演習では、次のことを行いました。

- agent の作業をリアルタイムで監視した。
- 個々のファイル編集を採用または取り消した。
- セッションの途中で指示を追加し、作業を調整した。

次は [アクセシビリティの変更をプルリクエストとして公開][next-lesson]し、このラボでローカルの custom agent が生成した変更を確認します。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [agent mode の使用][agent-mode]

---

| [← 前の演習: Custom agent][previous-lesson] | [次の演習: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
