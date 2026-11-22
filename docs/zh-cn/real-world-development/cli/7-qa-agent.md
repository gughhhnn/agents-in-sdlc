---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将 issue 需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在，你将通过自定义智能体整合这些能力，并定义清晰的 QA 流程。

本课将带你：

- 了解自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选 issue 审查其发现。

## 场景

在创建拉取请求 (PR) 前，Tailspin Toys 希望统一审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调这一 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专业版本。配置文件说明智能体的用途、指令和可用工具。在本研讨会中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义项各有用途：存储库指令描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何利用这些能力评估需求并报告发现。它不会取代这些能力，也不需要另开对话。

## 创建 QA 配置文件

在创建功能 PR 前，你将让 Copilot 创建一个可复用的 QA 配置文件。配置文件会定义 QA 执行的检查以及必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 向默认智能体发送请求，创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，请检查其配置文件，确认 Copilot 捕捉到了预期的 QA 工作流和边界。这样可以避免配置不完整或范围过宽的智能体在你只想验证功能时修改功能本身。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选项，但添加后会显示清晰的名称。
3. 阅读配置文件中的指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据，在修改实现代码前先征求同意，并且不会提交变更或创建拉取请求。
5. 如果生成的配置文件遗漏了上述职责或边界，请先让默认智能体修改，再继续。

## 根据 issue 执行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，请恢复同一个筛选对话，再选择 QA，以便它使用已有的 issue 和规划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚刚创建了智能体，它可能不会显示在可用智能体列表中。它仍然存在，上面的命令会启用它。

2. 使用以下提示让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后，阅读它提供的报告。

## 总结与后续步骤

你为工作流添加了可复用的专业角色，并审查了它的工作。本课中，你：

- 了解了自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选 issue 审查其发现。

现在，实现、技能更新、QA 配置文件、测试和验证报告都已准备好接受审查。下一步，[将这些内容整合到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
