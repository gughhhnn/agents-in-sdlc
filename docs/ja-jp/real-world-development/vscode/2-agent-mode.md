---
title: "演習 2 - Copilot agent mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、必要なファイルを探して変更し、テストを実行し、問題をデバッグし、更新し忘れたファイルを探して、さらに変更を加えることになります。作業は次々と続きます。

そこで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業できるように設計されています。開発者のように既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。Agent mode は IDE 内で作業し、素早くフィードバックを得ながら対話できます。Cloud agent はチームの一員のように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する方法。
- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる新機能を実装する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前に確認する方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリやパブリッシャーで絞り込めるようにします。前の演習でパブリッシャー用ヘルパーを追加しました。次は Copilot Agent Mode を使い、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を把握するため Tailspin Toys の Web サイトを確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。サイトは Astro アプリケーションひとつで構成され、ビルド時にページを静的 HTML として生成します。ページは frontmatter から Drizzle ORM を介してローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行できます。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace で新しいターミナルを開くには、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択するときは、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。Codespace へのプライベートトンネルを通じて、Web サーバーにアクセスできます。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順で説明します。

初期状態でも Web サイトは動作しますが、さらに機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、issue のバックログが自動的に作成されます。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** エージェントは、自律的に計画を立て、複数ファイルに変更を加え、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。ワークショップを作成する際に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードに一貫性を持たせ、使いやすくするためです。ただし、LLM は確率に基づいて結果を生成するため、ワークショップに記載したものと異なる提案が出る場合があります。これは通常の動作です。

> [!NOTE]
> 確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できます。

3. 次のプロンプトを Copilot に送り、issue のバックログを確認します。

   ```plaintext
   GitHub リポジトリのバックログを表示してください。ユーザーに最も役立つものを優先して、優先順位を付けてください。
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot はユーザーにとって役立つと思われる項目を優先して表示します。

## インストラクションファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを与えるインストラクションファイルを確認しましょう。Web サイトへの機能追加方法が記載された [user interface (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の指針を確認します。内容には次の項目が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル種別に対応するインストラクションへのリンク。

> [!TIP]
> インストラクションファイルから、他のインストラクションファイルやプロジェクト内のファイルを参照できます。参照パスはインストラクションファイルからの相対パスです。インストラクションを再利用し、大きなセットを小さく管理しやすい単位に分け、コード生成の参考例を Copilot に示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) にフィルタリングのロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行し、合格している必要があります。Copilot Agent Mode にこれらの作業を任せます。

1. 現在の Copilot 会話を続けるか、**New Chat** を選んで新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** を選択し、インストラクションファイルを追加します。

   ![UI インストラクションファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択で **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. 次のプロンプトで、バックログの関連 issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   バックログにある関連 GitHub issue の要件に基づいて、パブリッシャーとカテゴリで絞り込めるようサイトを更新してください。前の演習で publishers ヘルパーを作成済みです。必要に応じてその作業を維持または調整しながら、残りのデータレイヤー、UI、テストを完成させてください。完了する前に、すべてのテストが合格することを確認してください。サーバーはすでに実行中なので、起動する必要はありません。
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを見つけ、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べているスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行など、複数の作業を実行します。タスクやコードベースの複雑さによっては数分かかります。その間、エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業中に選択する必要はありません。完了すると、すべての変更に **Keep** または **Undo** が表示されます。

7. Copilot に確認されたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーが見つからなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode の Chat セッションが完了した状態を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動しているブラウザーに戻り、新機能を試します。
11. 動作を確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使い、Tailspin Toys の Web サイトに機能を追加しました。次の方法を学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる新機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前に確認する方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成しましょう。

### 追加演習 — ページネーションを実装する

ゲーム一覧が増えるにつれて、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存のインストラクションファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定値にするかを検討します。
- 希望する実装に必要な指示を Copilot に伝えます。
- Copilot に変更やコンテキストを伝えながら、何度かやり取りする必要があるかもしれません。Copilot を使う際によくある進め方です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
