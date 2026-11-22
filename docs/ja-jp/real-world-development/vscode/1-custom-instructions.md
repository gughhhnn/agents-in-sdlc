---
title: "演習 1 - カスタムインストラクション (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。特に重要なのが、コード生成の指針を伝える[インストラクションファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタムインストラクションとパスを指定したインストラクションファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在のインストラクションを使って、フィルタリングに必要なデータの最初の部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に沿う様子を確認します。
- インストラクションの更新とフィルタリングの最初の部分を `main` にコミットし、次の演習で新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードは、設定した標準と異なる場合があります。Copilot は確率に基づいて結果を生成します。この演習では、文字単位で同じ出力を得ることではなく、インストラクションの更新後に動作がどのように変わるかを確認します。

## インストラクションファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードで、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

インストラクションファイルを使って、これらの開発手法に沿って Copilot がタスクを実行できるようにします。

### カスタムインストラクション

カスタムインストラクションを使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用するインストラクションや、特定のファイル用のインストラクションを作成できます。

インストラクションファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一のインストラクションファイルです。大半の Copilot Chat または CLI リクエストに関連する、プロジェクトの技術スタック、概要、ベストプラクティスなどを記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用するインストラクションです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに関するガイダンスを記述できます。

> [!NOTE]
> IDE では、インストラクションファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ全体のインストラクションと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、別の標準に準拠するインストラクションファイルも[サポートしています][custom-instructions-support]。

### インストラクションファイルを管理するためのベストプラクティス

インストラクションファイルの詳細はこの演習の範囲外です。ただし、サンプルプロジェクトのファイルから一般的な方法を確認できます。

- `copilot-instructions.md` には、作成中のアプリの説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト全体のガイダンスを記述します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスク向けのガイダンスを記述します。
- 自然な言葉を使います。ガイダンスは明確にし、コードのよい例と悪い例を示します。

AI の使い方に唯一の正解がないように、インストラクションファイルにも決まった方法はありません。試しながら、プロジェクトに最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分なインストラクションファイルを用意しましょう。このプロジェクトでは、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに対応したファイルを確認できます。
>
> Copilot にインストラクションファイルを作成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、利用する環境によって方法が異なります。必要な場合は、各レッスンで説明します。
>
> テンプレートや出発点を探す場合は、インストラクションファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタムインストラクションファイルを確認する

このリポジトリに含まれるインストラクションファイルを読みましょう。中核となる `copilot-instructions.md` と、さまざまなタスクに対応する `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の項目も確認します。これらの指示は Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションがあります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、インストラクションを適用するファイルをリポジトリのルートからの glob で指定します。ここでは、`**/*.test.ts` に一致する TypeScript のテストファイルなどが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。他のインストラクションファイル (`unit-tests.instructions.md` など) やプロジェクト内の既存ファイルへのリンクがあります。大きなインストラクションを小さく再利用可能なファイルに分け、コード生成の参考を Copilot に示せます。ここにあるパスはリポジトリのルートではなく、インストラクションファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントについてはまだ触れていません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここでは、ベストプラクティスに沿って変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 次のコマンドで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## インストラクションを更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順で説明します。

カスタムインストラクションの効果を確認するため、まずは現在のインストラクションファイルを使ってプロンプトを送ります。その後インストラクションを更新し、同じプロンプトを再送して違いを確認します。

1. 前の演習で使ったエディタータブを閉じ、Copilot に必要なコンテキストだけが伝わるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを追加する場所を Copilot に伝えます。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送ります。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、各パブリッシャーの名前と ID を含む一覧を返すデータアクセスヘルパーを実装してください。ファイルを変更してください。
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパーファイルとテストを変更することもあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイルコメントのヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率に基づいて結果を生成するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。インストラクションの更新後に、動作の一貫性が高まることがこの演習の目的です。

## リポジトリの新しい標準を追加する

先ほど確認したように、`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に伝えるためのファイルです。コード提案の質を高めるため、リポジトリのコーディング標準を記述します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。そのため、先ほど生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加し、ファイルコメントのヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルの `.github/copilot-instructions.md` には全体に適用する指針を記述し、`*.instructions.md` には特定の言語、ファイル種別、タスク向けの指針を記述できます。リポジトリ全体に適用する標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送ります。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、各パブリッシャーの名前と ID を含む一覧を返すデータアクセスヘルパーを実装してください。ファイルを変更してください。
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使うコードに適用できました。

## インストラクションとブランチをコミットする

インストラクションファイルも、他のアセットと同じようにソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルから、次のコマンドでインストラクションとヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 次のコマンドでインストラクションと、ヘルパーの基盤として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更 (テストなど) が表示される場合は、そのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue を紐付けたプルリクエストを作成します。手動で行う代わりに、Copilot に issue を探し、issue をクローズする PR を作成して、マージまで依頼できます。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートでは GitHub のツールがワークスペースに設定済みなので、追加のセットアップは不要です。Copilot が初めてツールを使うときに、VS Code から GitHub へのサインインを求められることがあります。プロンプトに従ってください。この接続がどのように機能するかは、後の演習で Model Context Protocol を使って学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. インストラクションファイルの更新と、フィルタリング作業に向けた新しい publishers ヘルパーの基盤の両方を説明する PR を現在のブランチから作成するよう、Copilot に依頼します。

   ```plaintext
   インストラクションファイルの更新に関する issue を探してください。現在のブランチから新しい PR を作成し、その issue をクローズすることと、今後のフィルタリング作業に向けて publishers ヘルパーの基盤も追加したことを説明してください。
   ```

4. Copilot が情報を取得したり操作を実行したりするとき、GitHub 上での操作を許可するよう求められたら、コマンドを確認して必要に応じて **Allow** を選択します。
5. PR が作成されたら、Copilot に次のプロンプトで PR のマージと、ブランチを `main` に戻すことを依頼します。

   ```plaintext
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードをプルして最新の状態にしてください。
   ```

6. Copilot が GitHub 操作やシェルコマンドの実行を許可するよう求めたら、コマンドを確認して必要に応じて **Allow** を選択します。

GitHub Copilot を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がプロジェクトのインストラクションを利用する方法を確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- 既存のインストラクションでコード生成のプロンプトを送り、生成結果を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に沿うことを確認しました。
- インストラクションの更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[agent mode][next-lesson]でインストラクションを活用し、Copilot にコードベース全体の新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズにインストラクションファイルを使う][instruction-files]
- [効果的なカスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタムインストラクションを改善するための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタムインストラクション][personal-instructions]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
