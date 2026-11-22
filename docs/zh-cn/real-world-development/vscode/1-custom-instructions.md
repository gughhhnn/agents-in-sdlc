---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种是[指令文件][instruction-files]，它可以为代码生成提供指导。

本练习将带你：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 使用当前指令生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选切片提交到 `main`，供 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设定的某些标准。Copilot 的输出具有概率性。本练习旨在观察更新指令后的*整体行为趋势*，而不是要求输出逐字相同。

## 指令文件

### 场景

和其他优秀的软件开发团队一样，Tailspin Toys 制定了一系列开发规范和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

借助指令文件，你可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过这种方式引导 Copilot 提供更符合需求的建议和代码片段。可为整个存储库创建指令，也可针对特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含与大多数 Copilot Chat 或 CLI 请求相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件。你可以为 TypeScript 或 Astro 等特定语言，或创建 UI 组件、单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用存储库级指令和带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了常见做法。总体而言：

- 在 `copilot-instructions.md` 中集中提供项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码应有和不应有的示例。

创建指令文件没有唯一方法，使用 AI 也是如此。通过实践，你会找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览此项目中的文件时，你会看到针对多种任务的指令，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。不同产品界面的操作各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；相关练习会在适当位置说明。
>
> 想要模板或起点？请查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 和一组面向不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` 字段；其中的 glob 模式相对于存储库根目录，决定指令适用于哪些文件。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 留意针对项目单元测试的专用指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并为 Copilot 提供代码生成示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求为代码添加文档。接下来将添加 TSDoc 文档注释和文件头注释的规则。

## 为变更创建分支

遵循最佳实践，为本次变更创建一个分支。

1. 返回上一练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；每一步会在需要时说明。

要查看自定义指令的影响，先在当前指令文件下发送提示。稍后更新指令文件后，再发送相同提示并比较差异。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个有类型的函数，它将 `db` 客户端作为第一个参数，并返回有类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释块。

> [!CAUTION]
> Copilot 的输出具有概率性，即使没有指示，它也可能添加文档注释。如果发生这种情况也没关系；更新指令后行为的一致性改进仍是本练习的重点。

## 添加新的存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在记录存储库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约在第 27 行。留意该部分介绍了项目编码标准，但尚无代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，指令文件可以在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以通过 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如刚刚添加的文档注释规则。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除缓冲区。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意现在建议的文件开头包含类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数现在包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将标准应用到实际代码中；下一练习将基于这些代码继续构建。

## 提交指令并推送分支

指令文件和存储库中的其他资源一样，也需要通过源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数变更都已保存：

   ```bash
   git status
   ```

3. 暂存并提交指令更新及 Copilot 为辅助函数基础工作修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应创建拉取请求，并关联待办列表中的文档标准 issue。可以手动操作，不过 Copilot 可以使用项目中已连接的 GitHub 工具代为处理。让 Copilot 查找 issue、创建可关闭该 issue 的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需额外配置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这种连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选择 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的 issue，并从当前分支创建 PR，说明指令更新和新增的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 当 Copilot 提示是否允许它代表你执行 GitHub 操作时，请检查命令并酌情选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR 并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 提示是否允许它执行 GitHub 操作和 shell 命令时，请检查命令并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结与后续步骤

你了解了 Copilot 如何获取此项目的指令文件上下文，并在 VS Code 中使用 Copilot Chat 完成以下工作：

- 发送代码生成提示，并观察现有指令下 Copilot 的输出。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次发送相同提示，观察建议的代码如何采用新标准。
- 将指令更新和辅助函数基础工作提交到 `main`，供下一练习继续使用。

接下来，在[智能体模式][next-lesson]中应用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建有效自定义指令的最佳实践][instructions-best-practices]
- [编写更有效 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
