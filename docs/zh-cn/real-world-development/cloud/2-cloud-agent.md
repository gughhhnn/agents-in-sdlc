---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或长期搁置在待办列表中的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。智能体完成工作后，会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的工作。

本课将使用 Copilot 云智能体探索以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商留下的文档状况不佳——实际上完全缺失。首先，团队希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏（同一类别中的其他游戏），让用户可以继续浏览。现在不需要完善的实现，只需要能用于用户体验验收测试的版本。这项任务目前尚未完成，但还有优先级更高的 issue。

这两类任务都很容易被降级处理，因此适合分配给 Copilot 云智能体。它可以异步处理任务，让开发者继续处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。就像与开发者协作一样，有多种分配方式，包括[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或完成后查看 Copilot 的工作。解决方案准备好供你审查时，它会在拉取请求中通知你。

## 明确界定任务范围的重要性

GitHub Copilot 并无魔法——你仍然需要思考问题。看似简单的操作，深入分析后也会变得复杂，因此请[谨慎界定 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、试验并逐步调整。生成式 AI 不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果你已完成自定义指令练习，就已经见过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。将工作分配给 Copilot 云智能体前，先只读查看存储库中的指令文件，稍后便能发现它们的作用。

在 GitHub 网页中打开以下文件；如果已启动 Codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定为适用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` issue 分配给云智能体时，留意生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 规范和文件头注释。这些内容来自上述指令文件。后续练习审查 PR 时还会再次提到。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保一切就绪。将任务分配给 Copilot 时也一样，因为它的工作方式类似软件工程师。

云智能体使用 [GitHub Actions][github-actions] 环境执行工作。你可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]来自定义环境，让智能体获得所需开发工具和依赖项。为简化实验流程并提供学习机会，本项目已预先配置此工作流。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用所需的迁移并初始化数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的“Actions”选项卡手动运行这些设置步骤，以便进行测试
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在这些步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

      # 迁移并初始化本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流相同，但有几个要点：

- 其中仅包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会先运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。在不等待 Copilot 启动工作流的情况下，可以先用它确认工作流是否成功。

## 添加文档

虽然人人都知道文档的重要性，但大多数项目的文档要么过时，要么完全缺失。这类技术债务经常无人处理，会拖慢工作效率、增加代码库维护难度，并让新开发者难以加入团队。Copilot 擅长创建文档，因此这是分配给 Copilot 云智能体的理想 issue。它会在后台生成所需文档，后续练习中你会回来审查成果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，你应该会看到 Copilot 创建的待办清单。

> [!NOTE]
> Copilot 的待办清单可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看待办清单和 Copilot 即将执行的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的指示器。

16. 留意你可以滚动查看实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适方案，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们能专注于其他工作。现在就利用这一点：先为 Copilot 云智能体分配另一个任务，然后转而为应用编写代码，添加功能。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，它则处理另一组。为设计团队添加相关游戏区域可能不需要很久，但这段时间仍可用于其他工作。现在把任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在 issue 的第一条评论中看到一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！Copilot 云智能体的工作方式类似软件工程师，你无需持续监控，完成后再审查即可。现在来了解如何创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将 issue 分配给云智能体，让它异步处理。Copilot 可用于处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

云智能体正在后台处理任务，现在可以转而创建和使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并可使用之前课程中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
