---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode を使ってフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルタリング機能を手動テストするため、Copilot に Playwright MCP を使ってもらいます。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部に接続できると、さらに便利になります。Copilot がそのために使う仕組みが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![agent mode の内部構造と、コンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための方法を提供します。MCP により、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。最新情報 (リソース) にアクセスしたり、ユーザーに代わって操作 (ツール) を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールの橋渡しをする MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や NPM パッケージなどのローカルツールと AI エージェントとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールとリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントで、新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントで、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも多くの MCP サーバーが、さまざまなツールやリソースへのアクセスを提供しています。GitHub は、エコシステムでの発見や貢献を促進するために[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトの他の依存関係と同様に扱ってください。利用前にソースコードを注意深く確認し、発行元を確かめ、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを利用し、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` の設定はプロジェクトテンプレートに含まれているため、前の演習で Copilot はバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、GitHub ツールを初めて使うときに認証を求めます。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できます。機能をテストするために利用します。

1. `.vscode/mcp.json` に `github` と並べて `playwright` の設定を追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは、表示ウィンドウなしでブラウザーを実行するよう Playwright に指示します。画面を表示するデスクトップ環境がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンドテストにも Playwright を使っているため、必要なブラウザーはすでにインストールされていることがほとんどです。Copilot からブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行して再度試すよう依頼してください。

## サーバーを起動して信頼する

VS Code は、Copilot がサーバーのツールを初めて使うときに MCP サーバーを**オンデマンドで**起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` の設定の上に **Start** が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定を確認して信頼することを選択し、サーバーを起動します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを使うときに、VS Code がサーバーを起動して信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot は実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントにページを開き、移動し、フィルターを適用し、結果を読み上げてもらい、確認した内容をまとめてもらえます。会話を離れずに機能を確認するための素早い方法です。

内部では、Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントは、支援技術と同じようにボタン、リンク、リスト項目などの構造化されたラベル付き要素を判断材料にします。そのため、機能を手早く確認しながら、アクセシビリティも簡単に確認できます。

サーバーを接続し、アプリを起動した状態で、次のプロンプトを Copilot に送り、フィルタリング機能を確認してもらいます。

```text
Playwright MCP サーバーを使って、実行中のアプリ (http://localhost:4321) をブラウザーで開き、新しいゲームのフィルタリング機能を確認してください。

1. ゲームページに移動し、一覧に表示されているゲームの数を確認します。
2. カテゴリフィルターを適用し、そのカテゴリのゲームだけが表示されることを確認します。
3. フィルターを解除し、パブリッシャーフィルターを適用して、そのパブリッシャーのゲームだけが表示されることを確認します。
4. カテゴリとパブリッシャーを組み合わせ、両方の条件が結果に反映されることを確認します。

各手順で確認したことを報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件に照らして報告を確認してください。問題がある場合は、プルリクエストを作成する前に、追加の質問をするかコードの修正を依頼します。

> [!NOTE]
> テストを実行するには、`http://localhost:4321` でアプリが動作している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードすることがあります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再度試すよう依頼してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームでレビューするためにプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![変更されたファイルを示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストを作成する方法には、github.com や GitHub コマンドライン インターフェイス (CLI) を使う方法などがあります。ここでは GitHub Copilot に依頼してみましょう。Copilot は関連 issue を探し、PR と関連付けて作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認し、Copilot が GitHub ツールを利用できるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   リポジトリでカテゴリとパブリッシャーによる絞り込みに関する issue を探してください。現在のブランチから新しいプルリクエストを作成し、正しい issue と関連付けてください。
   ```

4. Copilot が必要な情報を取得したり操作を実行したりする際に求められたら、**Continue** を選択します。GitHub ツールを初めて使うときに認証を求められることがあります。指示に従ってください。
5. Copilot が issue を探して適切なものを見つけ、PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択して PR を確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次の操作を行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP サーバーを使ってブラウザーで機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

機能のテストに MCP サーバーを使いましたが、MCP は Copilot を拡張する方法のひとつです。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなど、目的を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
