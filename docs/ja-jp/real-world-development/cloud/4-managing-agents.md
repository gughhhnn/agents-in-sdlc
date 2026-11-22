---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、ユーザー体験の改善と機能追加に関する3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法は多数あります。たとえば、GitHub.com の[agents ページ][agents-page]を使えます。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べ、進行中のセッションに指示を追加して作業の方向を導くこともできます。

このレッスンでは、次の操作を行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を送り、追加機能を依頼します。

## シナリオ

ハイコントラストモードを作成するようエージェントに依頼した後、チームはライトモードも追加するとよいと気付きました。サイトのスタイルを更新し、切り替え機能を追加する作業をすでに行っているため、ライトモードも含めるのが合理的です。エージェントの作業に指示を送り、ハイコントラストモードとライトモードの両方を実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。Copilot に割り当てたタスクには、次のようなものが表示されます。
   - コードのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択します。Copilot が実行した次のような処理を確認します。
   - リポジトリのコードをチェックアウトする。
   - Copilot の環境を作成する。
   - MCP サーバーをセットアップする。
   - タスクを完了するためにさまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実施方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) のペインも確認します。PR と変更されたファイルを確認し、作業を監視できます。

## Cloud agent に指示する

現在実行中のタスクを確認できました。次は、Copilot がハイコントラストモードに取り組んでいる間にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードを追加しているセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control に表示されるアクセシビリティのセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    ハイコントラストモードに加えて、ライトモードも追加してください。このモードにも切り替えを用意し、ユーザーが希望する表示モードを選べるようにしてください。
    ```

    ![agents ページの cloud agent タスクで、作業中のセッションに指示を送るダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に反映する様子を確認します。

## Copilot の作業を待つ

更新したタスクに取り組むため、Copilot は引き続き作業します。メッセージを送信した時点で進行中だった処理が終わると、新しい要望が作業に反映されます。

前と同じように数分かかります。これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して案内する中心的な場所である Copilot agents ページを確認しました。mission control では、実行中と完了済みのタスクを追跡し、実行中の作業をレビューし、進行中のタスクに指示を送って範囲を調整できます。

次の操作を行いました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションに指示を送り、追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
