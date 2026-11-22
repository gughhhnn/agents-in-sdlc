---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義すると、特定の種類の作業を効果的に進めるための、対象を絞ったインストラクション、ベストプラクティス、コーディングパターン、専門知識を Copilot に伝えられます。チームの専門知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを徹底するセキュリティエージェント、一貫したテストパターンを維持するテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー内の Markdown ファイルか、`~/.copilot/agents` に定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その後にエージェントの動作、専門知識、インストラクションを定義する Markdown プロンプトを記述します。

### カスタムエージェントと agent skills の違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義され、AI が作業を実行する方法を指定します。簡単に区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや他のエージェントを調整できます。この演習では、アクセシビリティのカスタムエージェントがガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロースキルや、テストを実行・管理するスキルなどを呼び出す場合があります。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使う他の作業と同様に、自分の環境やシナリオに合わせて試し、調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを使えるように取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不足しており、読みにくいというフィードバックがありました。このアクセシビリティの問題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

この演習では、次の操作を行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに用意されています。内容を確認して、Copilot をどのように導くかを理解しましょう。

Codespace に戻り、次に新しいブランチを作成してアクセシビリティ作業に切り替えます。演習 3 のフィルタリング PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイト向けのコードを生成する際の主な役割。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義されたすべてのカスタムエージェントが、Copilot Chat ビュー下部のエージェントのドロップダウンに表示されます。カスタムエージェントを選ぶと、そのエージェントのインストラクションとツールに合わせてチャットセッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順で説明します。

1. Chat ビューのエージェントのドロップダウンで、**Agent** を選択します。すでに選択されている場合はそのまま進みます。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェントのドロップダウンを選択します。現在のエージェントが表示されています。既定では **default** です。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

   ```plaintext
   サイトにハイコントラストモードを追加してください。ユーザーがハイコントラストを切り替えられるようにし、ブラウザーの local storage を使って設定をページ再読み込み後も保持してください。
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトファイルに適用します。変更はファイルパスと差分とともにチャット内に表示されます。
6. 作業が進むと、エージェントが変更したファイルの一覧を示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連するテストなどの実際のファイルを変更します。

次の演習では、この作業を確認し、方向を調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた GitHub Copilot の AI アシスタントである[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、特定の種類の作業を効果的に進めるよう Copilot を導けます。

次の操作を行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat の agent mode でカスタムエージェントを使いました。

次は[エージェントの作業を監視して調整し][next-lesson]、同じセッションにライトモードの切り替えも追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
