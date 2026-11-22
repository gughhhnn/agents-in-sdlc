---
title: "レッスン 4 - カスタムインストラクションで Copilot を導く"
description: "リポジトリのインストラクションを確認し、ドキュメント標準を追加して、コードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合は、その指針を Copilot が利用できるようにします。[インストラクションファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルタリング機能を作成したので、Copilot が使ったインストラクションを確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリのインストラクションやパスを指定したインストラクションファイルがエージェントに適用される仕組みを確認します。
- コーディング標準に従うよう、インストラクションファイルを更新します。
- インストラクションファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントには、コードの言い換えではなく、意図や自明ではない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードの変更時にはコメントを最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

インストラクションファイルを使い、これらの開発手法に沿って Copilot がタスクを実行できるようにします。

## インストラクションファイル

カスタムインストラクションを使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用するインストラクションや、特定のファイル用のインストラクションを作成できます。

インストラクションファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一のインストラクションファイルです。大半のリクエストに関連するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイダンスを記述します。

> [!NOTE]
> その他のインストラクション形式やサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタムインストラクションのサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタムインストラクションファイルを確認する

スタータープロジェクトには、すでにインストラクションファイルがいくつか含まれています。変更前に内容を確認して、変更の影響を見てみましょう。

1. Codespace に戻ります。Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明とコーディングガイダンスを確認します。このインストラクションは、リポジトリ内で Copilot を使うすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションがあります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、インストラクションが適用されるファイルを指定する glob です。
5. `.github/instructions/drizzle.instructions.md` を開き、他のインストラクションファイルや既存プロジェクトファイルへのリンクを確認します。これにより、大きなインストラクションセットを再利用可能な小さなファイルに分割し、参考例を Copilot に示せます。

## インストラクションファイルを更新する

すでにあるファイルもよい出発点ですが、追加すべき内容があります。生成された TypeScript に TSDoc コメントを追加するよう、`copilot-instructions.md` ファイルを更新します。

1. `.github/copilot-instructions.md` で、35行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きに次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリのインストラクションを読み込みます。更新したインストラクションを適用するには、フィルタリングの会話を終了して再開し、機能のコンテキストを保ったまま Copilot に作業を依頼します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで、`game-filters-cli` ブランチに切り替えて保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   必要に応じてフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 次のプロンプトで、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   インストラクションとコードガイダンスを更新しました。生成したコードをそのガイダンスに合わせて更新してください。
   ```

4. `/diff` を実行して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がインストラクションを選ぶ仕組みを確認し、機能に新しい標準を適用しました。具体的には、次の操作を行いました。

- リポジトリのインストラクションとパスを指定したインストラクションファイルがエージェントに適用される仕組みを確認しました。
- コード生成に関するインストラクションを更新しました。
- コードに対するインストラクションの影響を確認しました。

次は[再利用可能な `quality-checks` スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタムインストラクションを追加する][instruction-files]
- [カスタムインストラクションのサポート][custom-instructions-support]
- [効果的なカスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
