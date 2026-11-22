---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub 上で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を行い、プルリクエストを作成します。その間、別の作業を続けられます。

各演習では、まず cloud agent が使うカスタムインストラクションを追加し、その後 GitHub issue を割り当てて実装を任せます。カスタムエージェントを使って方針を定め、agents ダッシュボードでセッションを監視して調整し、最後に作成されたプルリクエストを確認して結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタムインストラクション][ex1] | コンテキスト | cloud agent が使うカスタムインストラクションを追加する |
| [2. Cloud Agent][ex2] | 非同期エージェント | Copilot cloud agent に issue を割り当てる |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して調整する |
| [5. 反復と改善][ex5] | レビュー | PR を確認し、Copilot の作業を改善して、次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していない場合でも、認定を受けた学生は [GitHub Education][callout-student-plan-education] を通じて無料で GitHub Copilot を利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どのハーネスも完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free では利用できません。Copilot Business または Enterprise の場合は、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## 開始する

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
