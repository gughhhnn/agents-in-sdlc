---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、インストラクション、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、インストラクションの更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべてひとつのブランチに保存されています。まとめて確認して、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、インストラクション、スキルの更新、QA エージェントを複数の PR に分割します。ワークショップを簡略化するため、フィルタリングと品質ワークフロー全体をひとつの会話とブランチで進め、この PR にまとめています。

このレッスンでは、次の操作を行います。

- Agent Merge とは何か、マージのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリング PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングの作業では、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの許可を開発者が管理しながら、PR の残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストを取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェック、レビューコメントへの対応、必要な場合の rebase など) を解決します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。有効にする前に、作業を確認して準備ができていることを判断してください。

## Agent Merge で PR を管理する

コードの作成が終わったので、まとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を実行してエージェントのダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションの処理を確認します。すべて成功すると、マージが実行されます。
7. Copilot が「PR #14 was squash-merged successfully.」などのメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テスト、検証、プルリクエストのプロセスなど、開発プロセスの一部を自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge とは何か、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリング PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は[Copilot CLI の便利なスラッシュコマンドを確認し][next-lesson]、コンテキスト、モデル、共有、必要に応じたクラウドへの委任を試します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
