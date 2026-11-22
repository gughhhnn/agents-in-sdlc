---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "了解存储库指令，添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，你需要让 Copilot 获得相应指导。[指令文件][instruction-files]不仅描述你*想要什么*代码，还描述代码*应该如何组织*。现在你已实现筛选功能，接下来将探索 Copilot 使用的指令、添加文档标准，并将其应用到代码中。

本课将带你：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 和其他优秀的软件开发团队一样，制定了一系列开发规范和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值（包括存在时可注入的 `db` 参数）。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码变更时，注释也应保持更新。
- 保留现有格式和 lint 指南。

借助指令文件，你可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。这是一种强大的方式，可以引导 Copilot 提供更符合需求的建议和代码片段。你可以为整个存储库创建指令，也可以针对特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖某种格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

入门项目已包含一组指令文件。先探索现有文件，再进行修改，以了解修改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解其中的项目简介和编码指导。这些指令适用于你在此存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` 字段；其中的 glob 模式决定指令适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和现有项目文件的链接。你可以将较大的指令集拆分成较小、可复用的文件，并链接到可参考的示例。

## 更新指令文件以符合团队规范

现有文件是不错的起点，但仍有一项遗漏。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含文档注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约在第 35 行。
2. 将以下内容添加为该部分的最后一条：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存更新后的指令后，请退出并恢复筛选对话；这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看变更过的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结与后续步骤

你探索了 Copilot CLI 如何获取上下文，并将新的规范应用到功能中。本课中，你：

- 了解了存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码标准。
- 查看了指令文件对代码的影响。

接下来，你将[定制并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持情况][custom-instructions-support]
- [创建有效自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件及其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
