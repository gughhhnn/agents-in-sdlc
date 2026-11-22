---
title: "演習 1 - カスタムインストラクション (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[インストラクションファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタムインストラクションとパスを指定したインストラクションファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトを更新前後で比較しません。Copilot cloud agent は GitHub issue を非同期で処理するため、リアルタイムで影響を並べて示すのは困難です。このハーネスの後半で cloud agent が作成したプルリクエストをレビューし、インストラクションファイルの影響を確認します。

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

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一のインストラクションファイルです。大半のリクエストに関連する、技術スタック、作成中のアプリの概要、ベストプラクティス、全体に適用するガイダンスなどを記述します。
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

## リポジトリの新しい標準を追加する

ここで行う編集は、TSDoc のドキュメントコメントとファイル単位のコメントヘッダーをコードに含める、プロジェクト全体のルールを追加することです。以降の演習で issue を割り当てると、cloud agent はこのルールを適用します。

編集の前にブランチを作成します。cloud agent は issue の対象ブランチからインストラクションを読み取りますが、変更を整理してコミットします。

1. Codespace のターミナルで、次のコマンドを実行してブランチを作成します。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に伝えるためのファイルです。コード提案の質を高めるため、リポジトリのコーディング標準を記述します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の内容を追加し、ファイルコメントヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルの `.github/copilot-instructions.md` には全体に適用する指針を記述し、`*.instructions.md` には特定の言語、ファイル種別、タスク向けの指針を記述できます。リポジトリ全体に適用する標準には、リポジトリレベルのファイルが適しています。

## インストラクションの変更をコミット、プッシュ、マージする

Cloud agent は issue の対象ブランチにあるインストラクションファイルを読み取ります。次の演習で Copilot に issue を割り当てると、`main` からブランチを作成します。cloud agent が更新したインストラクションを使えるよう、変更を `main` にマージする必要があります。

1. 次のコマンドで変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを開いてマージします。次の演習で issue を割り当てると、cloud agent はこれらのインストラクションを読み取ります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成を省略して、変更を `main` に直接コミットできます。ここでは実際のプロジェクトで行う手順を体験するため、ブランチを作成しています。

## まとめと次のステップ

Copilot がこのプロジェクトのインストラクションを使う方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスで cloud agent が作成するプルリクエストを確認し、この標準が適用されているかを見ます。

次は[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズにインストラクションファイルを使う][instruction-files]
- [効果的なカスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタムインストラクションを改善するための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
