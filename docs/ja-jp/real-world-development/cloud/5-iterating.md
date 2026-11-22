---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加に関するいくつかのタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザインチームが改善を検討する関連ゲーム機能の作成、ハイコントラストとライトモードを切り替えるアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードを必ずレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督者がいない状態で作業するため、安全性を保つためのセキュリティ制約が設けられています。次のような制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコードの作業に使うブランチに限られます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人間の承認が必要です。
- MCP サーバーなど、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を確認します。ドキュメント作業のレビューには、GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認する際、GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。Copilot は MCP サーバーへの接続など、外部リソースへのアクセスが既定で制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が表示されます。その場合は作業の完了を待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら **Files changed** タブを選び、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメントコメントや、その他のドキュメントを確認します。実際の変更内容は異なる場合があります。

   変更を確認する際は、演習 2 の最初に確認したカスタムインストラクションが適用され、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーが反映されていることを確認します。

6. 更新を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認を待っていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが合格します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやり取りする方法は一方向ではありません。チームメンバーと同じように、プルリクエストのコメントやコードのインラインコメントで Copilot をメンションできます。Copilot はそのコメントを読み、対応するための新しいセッションを開始します。結果は確率に基づいて生成されるため、具体的な指示文は決められません。Copilot に依頼する内容の例を次に示します。

- 各コードファイルの先頭に、ファイルの役割を簡単に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例、またはコード内のドキュメントについて Copilot に依頼したい内容を使い、**@copilot** をメンションして変更を依頼するコメントを追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は新しいセッションを開始して変更を行います。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選び、変更を確認します。

納得できるまで、修正を繰り返せます。問題がなければ、ドラフト PR をレビュー可能な状態にして、`main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、Copilot が生成したコードをレビューします。
5. 更新を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認を待っていることを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが合格します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に変更を依頼して、繰り返し改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます (カスタムインストラクションを更新する**前**に割り当てたタスクのため、これらが含まれていない可能性があります)。前と同様に、**Conversation** タブに新しいコメントを追加すると、Copilot がコメントを読み、新しいセッションを始めます。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の mission control で追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が表示されます。その場合は作業の完了を待ちます。数分かかることがあります。

4. **Files changed** タブを選び、Copilot が生成したコードを確認します。
5. 実装をレビューします。特に次の点を確認します。
   - 各表示モードを切り替える UI コンポーネント。
   - ユーザー設定を保持する local storage の使用。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認を待っていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが合格します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用のカスタムエージェントが、Copilot のベストプラクティスに沿った機能実装をどのように支援したか確認できました。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションして変更を依頼できます。

## 任意の演習 — さらに作業を委任する

実際のバックログ項目を cloud agent に任せ、その間に別の作業を進めると、Cloud agent を効果的に使えます。この習慣を身に付けるため、リポジトリにいくつか issue を作成して Copilot に割り当ててみてください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタムインストラクションファイルを確認し**、後で cloud agent の出力に反映される様子を確認しました。
- **issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の作業を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成して使用し**、ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、進行中のアクセシビリティセッションを監視して調整しました。
- **cloud agent のプルリクエストを確認して改善し**、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。さらに試す場合は、ほかのハーネスで今回学んだ内容を発展させることができます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP の連携を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルからレビュー済みの変更を2つ作成します。

自分のリポジトリでは、次のような方法を試してください。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
