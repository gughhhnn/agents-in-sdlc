---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。具体的には、次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、初期状態のフィルタリング issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードで確認しました。
- カスタムインストラクションでエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの根拠を確認しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR に対して Agent Merge を許可しました。
- コンテキスト、モデル、共有、必要に応じたクラウドへの委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` からそれぞれブランチを作り、2つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価の状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、インストラクションを更新して機能に適用し、`quality-checks` レポートをカスタマイズして QA プロファイルを作り、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチで作業しました。ワークショップを効率よく進めるため、作業をひとつの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を介した Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果に要件とテストカバレッジの確認を加え、PR を作成する前にまとめました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと根拠を確認してください。変更があった場合は、影響を受ける根拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の質に影響します。このワークショップでは、インストラクションを更新し、スキルをカスタマイズし、QA プロファイルを作り、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。インストラクションは標準を設定し、スキルは繰り返し実行できるタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに応じて **モードとモデル** を選びます。実装前に方針を考えるには **Plan**、範囲の狭い変更でやり取りを続けるには **Interactive**、範囲が明確な作業には **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を作るのか、なぜ必要なのか、どう作るのかを明確に伝えると、出力が大きく変わります。

## さらに試す

主要なワークフローを体験しました。Copilot CLI には、次のような便利な機能もあります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールでも、上達するには使い続けるのが一番です。本番コード、趣味のコード、何年も作ろうと思いながら手を付けていなかった小さなアプリなどで試してください。学んだことをチームと共有し、チームからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
