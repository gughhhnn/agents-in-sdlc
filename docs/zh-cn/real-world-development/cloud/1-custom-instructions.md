---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能访问这些上下文。[指令文件][instruction-files]用于提供此类指导，帮助 Copilot 理解你不仅希望它*做什么*，还希望它*如何完成*。

本练习将带你：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会运行*修改前/修改后*提示。Copilot 云智能体会异步处理 GitHub issue，因此难以实时并排展示指令的影响。稍后审查云智能体生成的拉取请求时，你会看到指令文件发挥的作用。

## 指令文件

### 场景

和其他优秀的软件开发团队一样，Tailspin Toys 制定了一系列开发规范和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

借助指令文件，你可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。这是一种强大的方式，可以引导 Copilot 提供更符合需求的建议和代码片段。你可以为整个存储库创建指令，也可以针对特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含项目级信息，例如适用于大多数 Copilot Chat 或 CLI 请求的技术栈、项目概述、最佳实践和全局指导。
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

花些时间阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 和一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` frontmatter，它使用相对于存储库根目录的 glob 模式，决定指令适用于哪些文件。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 留意针对项目单元测试的专用指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并为 Copilot 提供代码生成示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求为代码添加文档。接下来将添加 TSDoc 文档注释和文件头注释的规则。

## 添加新的存储库级标准

接下来需要编辑项目文件：添加项目级规则，要求在代码中通过 TSDoc 文档注释和文件级注释块添加文档。云智能体稍后处理本环境中分配给它的 issue 时会读取此规则。

编辑前，先创建工作分支（云智能体会读取 issue 目标分支中的指令，但仍建议单独提交这些更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在记录存储库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约在第 27 行。留意该部分介绍了项目编码标准，但还没有代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，指令文件可以在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以通过 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如刚刚添加的文档注释规则。

## 提交、推送并合并指令更新

云智能体会读取 issue 目标分支中的指令。下一练习中，你会将 issue 分配给 Copilot；它会从 `main` 分支创建分支，因此必须先将指令变更合并到 `main`，云智能体才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并完成合并。下一练习中，云智能体处理 issue 时就会读取这些指令。

> [!TIP]
> 如果本研讨会中希望直接在 `main` 上工作，可以跳过创建分支，直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你探索了 Copilot 如何获取此项目的指令文件上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续练习中，云智能体生成的拉取请求会体现该标准。

接下来，将[第一个 issue 分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建有效自定义指令的最佳实践][instructions-best-practices]
- [编写更有效 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
