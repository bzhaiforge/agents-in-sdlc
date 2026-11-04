---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

最近几次练习中，你已让 Copilot cloud agent 处理了三项旨在改善用户体验和添加功能的任务。Cloud agent 虽然异步自主工作，但监控任务仍然很重要。

可使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的 [agents 页面][agents-page]。在这个任务控制中心，你可以查看所有创建了开放 PR 的智能体任务，检查执行步骤，并引导进行中的会话。

在本课中，你将：

- 查看 agents 页面，监控 cloud agent 任务。
- 引导正在进行的会话，要求添加更多功能。

## 场景

将高对比度模式任务分配给智能体后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并实现切换功能，将浅色模式纳入同一任务更合理。你将引导智能体，使它同时实现高对比度和浅色模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 工作状态和采用的方案而变化。

4. 同时查看右侧显示的拉取请求 (PR) 面板，其中会列出 PR 和更改的文件，方便进一步监控。

## 引导 cloud agent

查看活动任务后，要求 Copilot 在实现高对比度模式的同时添加浅色模式开关。

1. 选择正在添加高对比度模式的会话。确切标题会根据 Copilot 使用的名称和当前工作状态而不同。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始实际工作。届时会看到类似的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents 页面中的 cloud agent 任务截图，突出显示 Steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 查看 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。它会先完成发送消息时正在执行的步骤，再将新请求加入工作流程。

这可能还需要几分钟。等待时可以回顾目前学到和探索的内容。

## 总结和后续步骤

你使用 Copilot agents 页面作为监控和引导 cloud agent 任务的中心：跟踪活动和已完成任务、查看正在执行的工作，并调整进行中任务的范围或提供更多指导。你还引导会话添加了额外功能。

Copilot 正在完成无障碍功能。接下来[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
