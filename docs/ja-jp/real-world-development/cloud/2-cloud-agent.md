---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタムインストラクション][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間がレビューできるドラフト PR を作成します。面倒な作業を任せて開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent では、次の内容を確認します。

- コード生成の環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの最初のバージョンを作った請負業者が、ドキュメントを適切に整備しませんでした。実際には、ドキュメントがまったくありません。まず、アプリケーションのエクスポート関数すべてに TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示し、ユーザーが閲覧を続けられるようにします。完成された実装はまだ必要ありません。UX を受け入れテストできる程度のものが必要です。これは現在のブロッカーですが、優先度の高い他の issue があります。

こうしたタスクは優先順位が下がりやすい一方、Copilot cloud agent に任せるのに適しています。Copilot cloud agent は非同期に作業するため、開発者は別のタスクに取り組み、後から戻って作業結果を確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者のようにバックグラウンドで作業できます。[GitHub issue を Copilot に割り当てる][start-sessions]方法など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境をセットアップしてタスクに取り組み始めます。作業中も完了後も Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確に定めたインストラクションの重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作でも、詳しく確認すると複雑な部分が見つかります。[Copilot cloud agent のタスク範囲を決める際は注意してください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタムインストラクション

これまでの演習では、カスタムインストラクションと Copilot への指示方法を学びました。カスタムインストラクションの実習を終えている場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、この機会にざっと読みましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意されたインストラクションファイルを読み取り専用で確認すると、後でその影響を確認できます。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この設定により、インストラクションは `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、作成されたプルリクエストで TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーを確認できます。後の演習で PR をレビューするときに、もう一度この点を確認します。

## Copilot cloud agent の開発環境をセットアップする

コードを作成する際は、通常、適切な状態で作業できるように特定の環境を用意し、セットアップスクリプトを実行します。Copilot にタスクを割り当てる場合も同じです。Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。このワークショップでは学習しやすいよう、事前に設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、マイグレーションと初期データ投入済みの SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順だけに権限を絞ってください。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # セットアップ手順の一部としてリポジトリをクローンする (依存関係のインストールなど) 場合は、contents: read が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 単一の Astro アプリ用にローカル SQLite データベースをセットアップ (マイグレーションと初期データの投入) します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じですが、いくつか重要な点があります。

- `copilot-setup-steps` というジョブがひとつあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するまで待たずに、ワークフローの動作を確認できます。

## ドキュメントを追加する

ドキュメントが重要なのは誰もが理解していますが、情報が古かったり、そもそもなかったりするプロジェクトがほとんどです。こうした技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の受け入れを困難にします。Copilot はドキュメント作成が得意なので、この作業を Copilot cloud agent に任せるのに適しています。Copilot はバックグラウンドで必要なドキュメントを作成します。後の演習で作業内容を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選んで issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    組織では、関数やメソッドに必要に応じて TSDoc のドキュメントコメントを付けるよう定めています。しかし、最近の変更ではこの標準に従っていません。動作を明確にするドキュメントコメントが含まれるよう、既存のコードを更新する必要があります。
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

  ![issue を Copilot に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は前のセクションで説明したワークフローを使って環境を構築し、プロジェクトを分析して、作業方法を検討しています。

13. todo リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ブラウザーを再読み込みしてください。

16. ライブセッションをスクロールして、Copilot がどのように作業しているかを確認します。コードを調べて状態を理解し、適切な計画を考えるために一時停止し、コードを作成する様子が分かります。

完了まで数分かかることがあります。Copilot cloud agent の主な目的は、非同期でタスクを実行し、その間に別の作業を進められるようにすることです。別のタスクも Copilot cloud agent に割り当て、その間にアプリケーションの機能を実装するコードを書くことで、この機能を活用します。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点のひとつは、作業を分けられることです。デザインチームが求める関連ゲームの表示は、それほど時間がかからないかもしれませんが、その時間を別の作業に使えます。Copilot cloud agent に依頼してみましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選んで空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    ゲーム詳細ページに関連ゲームを表示し、ユーザーが他のゲームを見つけられるようにしたいと考えています。デザインチームは UX を確認し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` に、指定したゲームを除き、同じカテゴリに属する他のゲームを返すデータアクセスヘルパーを追加する
   - ヘルパーを使って、ゲーム詳細ページに「関連ゲーム」セクションを表示する
   - 関連ゲームがない場合に対応する
   - 新しいヘルパーのユニットテストを作成する
   - PR を作成する前に、すべてのテストが合格することを確認する
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

  ![issue を Copilot に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を始めたことを示す 👀 が issue の最初のコメントに表示されます。

![issue で Copilot の作業中を示す目の絵文字が表示されている画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組み始めました。Copilot cloud agent はソフトウェアエンジニアのように作業します。常に監視する必要はなく、完了してから結果を確認できます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当て、非同期で作業を任せられます。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などを Copilot に依頼できます。

次の内容を確認しました。

- コード生成の環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

Copilot cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタムインストラクションにも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
[start-sessions]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
