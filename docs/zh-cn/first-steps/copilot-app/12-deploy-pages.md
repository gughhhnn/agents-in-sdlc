---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot 应用配置 Pages 工作流、审查其拉取请求，并验证 Space Quiz 在线运行。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 为 Space Quiz 创建公开访问入口。Copilot 可以准备工作流并引导部署，但你仍需负责审查变更并确认网站已上线。

本课将带你：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证发布的网站。

## 场景

Space Quiz 已在 GitHub 存储库中，但用户仍需要简单的体验方式。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖，即可通过公开 URL 访问。

## 让 Copilot 准备部署

> [!WARNING]
> 创建拉取请求可能会提交并推送代码。发送此提示前，请检查批准设置和目标存储库。选择 **Approve all** 后，不要期待出现单独的确认提示。

在 `space-quiz` 的 Copilot 应用会话中发送以下提示：

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## 审查工作流和拉取请求

1. 确认仅在存储库尚无 README 时才添加。
2. 审查拉取请求变更，包括工作流的构建和部署步骤、触发条件及权限。
3. 确认工作流从默认分支部署，而不是从尚未合并的拉取请求部署。
4. 要求修正任何问题并审查更新后的差异。授权合并前，等待必需的拉取请求检查全部通过。

## 授权合并和部署

> [!WARNING]
> 以下提示授权智能体合并已审查的拉取请求、修改存储库 Pages 设置并发布网站。只有在完成审查并确认存储库和拉取请求后才能发送。批准设置可能允许这些操作直接执行，而不会再次询问。

将 `<PR_NUMBER>` 替换为已审查的拉取请求编号，然后单独发送以下提示：

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功的 GitHub Actions Deploy website 运行，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## 验证部署

1. 跟踪 GitHub Actions 部署运行，直到成功。
2. 打开报告的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按它提供的具体说明操作。只有在部署成功且网站在线可访问后，任务才算完成。

## 总结与后续步骤

Space Quiz 现已作为在线网站发布。你还亲自验证了部署，而不是只依赖智能体的报告。继续学习[第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
