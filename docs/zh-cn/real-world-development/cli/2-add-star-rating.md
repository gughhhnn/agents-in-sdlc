---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查变更，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过一次对话后，现在可以开始修改项目了。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。你将让智能体显示评分、审查变更，并将其作为第一个拉取请求合并。

本课将带你：

- 在功能分支上启动专注于单项任务的 Copilot 对话。
- 让智能体对项目进行小幅修改。
- 使用 `/diff` 审查变更。
- 运行应用，并在转发到浏览器的页面中确认变更。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已显示该评分。但主页上的游戏卡片只显示标题、类别、发行商和描述。作为热身，你将让智能体在每张卡片上显示现有评分。这项变更小巧独立，非常适合首次使用 Copilot CLI。

## 对话的组成

**对话**是你使用 Copilot CLI 执行任务的地方。与 Copilot 应用不同，常规 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用 worktree。保存的对话让你可以稍后回到同一讨论，而文件和分支仍是磁盘上的常规 Git 状态。

在对话中，你会看到三类内容：你发送的提示和智能体的回复、智能体探索和编辑文件时使用的工具，以及可通过 `/diff` 检查的变更。

## 启动对话并请求变更

现在启动新对话，开始实现功能。

1. 返回 Codespace。
2. 如果之前打开的终端现已关闭，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot CLI 尚未启动，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求变更：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并创建所需代码。你已通过 Copilot CLI 添加了新功能。

## 审查差异

所有 AI 生成的变更在合并前都应审查，即使变更很小也不例外。现在直接在 Copilot CLI 中查看变更。

1. 输入 `/diff` 并检查每个变更文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖了两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> Copilot 和其他生成式 AI 工具一样，输出具有概率性而非确定性，因此你的代码可能与示例不同。请检查行为，不要期待某种特定实现。

## 检查变更

当然，不能只阅读代码就假定一切正常。现在让 Copilot 启动网站，以便在 Codespaces 转发到浏览器的页面中检查更新后的用户界面 (UI)。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

你已完成新功能！现在创建拉取请求 (PR)，将新代码合并到项目中。

1. 让默认智能体提交变更：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Copilot CLI 显示的 URL，以打开 PR。
4. 检查变更文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace 并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜你！你已使用 GitHub Copilot CLI 交付了第一个变更。本课中，你：

- 在功能分支上启动了专注于单项任务的 Copilot 对话。
- 引导智能体对游戏卡片进行小幅修改。
- 使用 `/diff` 审查变更。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选 issue 开始，并使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
