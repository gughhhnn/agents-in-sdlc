---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したとおり、コードを書く以外にも必要な作業があります。データや外部サービスを扱い、Copilot が利用できる自動化機能を追加することもあります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を超えるツールやサービスを Copilot が利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI でどのように使うかを学びます。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように作業中の Web サイトを使えるようにすると、変更の自動化が進み、更新が期待どおりに動作する確信を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための方法を提供します。MCP により、AI エージェントは外部のツールやサービスとリアルタイムに通信し、最新の情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部サービスの橋渡しをする MCP サーバーを介してアクセスします。各 MCP サーバーは、AI エージェントが利用できるツールとリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステムでの発見や貢献を促進するために[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトの他の依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を確かめ、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

まだ Playwright MCP サーバーを利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が設定されている場合は、このセットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を試すよう Copilot に依頼する

これまでは機能が期待どおりに動くことを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   アプリを起動し、Playwright MCP を使って issue と計画に照らしてフィルタリングを確認してください。動作する点としない点を教えてください。変更は加えないでください。確認が終わったら、起動したサーバーを停止してください。
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを使うため、指定する必要はありません。ただし、使用するツールが分かっている場合は、明示するとよいでしょう。適切なツールを使うよう促すことで、結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで機能を確認するための手順を表示する様子を見ます。
3. 結果のレポートを読み、すべて期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してサーバーを停止し、レポートを作成します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を試しました。振り返ると、次の操作を行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI でどのように使うかを学びました。
- まだ利用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を試しました。

次はスキルとブラウザーツールを専門的な役割にまとめた[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
