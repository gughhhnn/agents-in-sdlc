---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備をします。Tailspin Toys リポジトリを自分用にコピーし、作業用の [codespace][codespaces] を起動します。codespace は、インストラクションファイルの編集や cloud agent が作成した内容のレビューに使います。

## 演習用リポジトリをセットアップする

コードを作成するためのリポジトリのコピーを作ります。[テンプレート][template-repository]から新しいインスタンスを作成します。新しいインスタンスには演習に必要なファイルがすべて含まれます。演習中はこのリポジトリを使います。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. 演習用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

    ![テンプレートを使用するボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後で参照するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

## Codespace を作成する

次は、演習を行うために codespace を使います。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを書き、実行し、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、すべての機能を備えた IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりもはるかに早く準備できます。作成を待つ間に GitHub Copilot の機能を確認してみましょう。次のセクションで説明します。

> [!CAUTION]
> 今後の演習で codespace に戻ります。しばらくはブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの [dev container][dev-containers] で実行できます。どちらの環境にも、スムーズに進めるために必要なものがあらかじめインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

演習用リポジトリのコピーを作成できました。Copilot cloud agent と一緒に作業するときに使う codespace の作成も開始しました。

## 次のステップ

次は、cloud agent が使うカスタムインストラクションを追加します。[演習 1 - カスタムインストラクション][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
