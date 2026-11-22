---
title: "演習 3 - カスタムエージェント"
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

カスタムエージェントを使って、次の内容を確認します。

- カスタムエージェントの定義方法。
- カスタムエージェントにタスクを割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを使えるように取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不足しており、読みにくいというフィードバックがありました。このアクセシビリティの問題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに用意されています。内容を確認して、Copilot をどのように導くかを理解しましょう。

Codespace に戻り、アクセシビリティ用のカスタムエージェントファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイト向けのコードを生成する際の主な役割。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作するための中心的な場所です。Copilot cloud agent にタスクを割り当て、監視し、作業中のセッションに方向を示したり、追加のガイダンスを伝えたりできます。まず、ハイコントラストモードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選び、新しい issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    サイトにハイコントラストモードを追加してください。ユーザーがハイコントラストを切り替えられるようにし、ブラウザーの local storage に設定を保存してください。
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てのダイアログを開きます。
9. カスタムエージェント一覧から **Accessibility agent** を選択します。

    ![アクセシビリティのカスタムエージェントが選択された Copilot cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた GitHub Copilot の AI アシスタントである[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、特定の種類の作業を効果的に進めるよう Copilot を導けます。

次の内容を確認しました。

- カスタムエージェントの定義方法。
- カスタムエージェントにタスクを割り当てる方法。

Copilot はハイコントラストモードを実装しています。次は、mission control から[エージェントのセッションを監視して調整します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備をする][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
