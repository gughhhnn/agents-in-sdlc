---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两个练习中，你让 Copilot 云智能体处理了三项不同任务，用于改善用户体验并添加功能。云智能体虽然以异步、自主方式运行，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，你可以查看所有拥有未关闭拉取请求 (PR) 的智能体任务、查看它们执行的操作，甚至引导进行中的会话。

本课将带你：

- 探索智能体页面，监控云智能体任务。
- 引导进行中的会话，请求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到现在也是添加浅色模式的好时机。网站样式和切换功能都在修改中，因此一并加入这项功能很合理。你将引导智能体的工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作。这些操作可能包括：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成所分配任务所需的各项步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和执行方案而变化。

4. 同时留意右侧出现的 PR 窗格。你可以在此查看 PR 和变更文件，以便进一步监控。

## 引导云智能体

了解当前活动任务后，现在让 Copilot 在实现高对比度模式时一并添加浅色模式切换开关。

1. 选择标题中提到添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission Control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示引导活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！完成收到消息时正在执行的步骤后，它会将新请求加入工作流程。

和之前一样，这需要几分钟。你可以趁此暂停片刻，回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心控制台。你可以跟踪所有活动和已完成的任务、审查正在执行的工作，并重定向进行中的任务以调整范围或补充指导。

本课介绍了以下概念：

- 使用 Mission Control 和智能体页面监控云智能体任务。
- 重定向进行中的会话以请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
