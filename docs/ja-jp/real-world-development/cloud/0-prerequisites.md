---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-10-21
---

GitHub Copilot cloud agent の演習を始める前に、作業環境を準備します。Tailspin Toys リポジトリのコピーを作成し、instruction file の編集や cloud agent が生成した作業のレビューに使う [Codespace][codespaces] を起動します。

## ラボ用リポジトリをセットアップする

コードを作成するためのリポジトリを用意します。[テンプレート リポジトリ][template-repository]から自分用のインスタンスを作成してください。作成したリポジトリにはラボに必要なファイルが含まれており、演習を通して使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys のテンプレート][tailspin-template]を開きます。
2. ラボのリポジトリで **Use this template** を選択し、続けて **Create a new repository** を選択して、自分用のコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（`organization-or-user-name/repository-name`）を控えてください。演習の後半で使います。

## Codespace を作成する

続いて、演習を進めるための Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザー上でコードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールを使える統合開発環境（IDE）が用意されています。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりも短時間で準備できます。作成を待つ間に、次に扱う GitHub Copilot の機能を確認してもよいでしょう。

> [!CAUTION]
> 後の演習で Codespace に戻ります。今はブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行するように構成されています。どちらの環境にも、演習をスムーズに進めるための前提条件が用意されています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、cloud agent と作業するときに使う Codespace の作成を開始しました。

## 次のステップ

次は、cloud agent が従うカスタム instruction を追加します。[演習 1 - カスタム instruction][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
