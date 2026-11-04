---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

刚才我们为项目添加了一项小功能。较大的更改需要更完善的流程。本课将从 GitHub 议题出发，规划并实现功能，确保代码有效且行为符合预期。

> [!NOTE]
> 后续功能练习将继续使用同一对话和分支。实际工作中，通常会为不同文件类型使用不同分支或 PR；本研讨会简化了流程，以便聚焦核心概念。

在本课中，你将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

后续还会更新存储库指令、自定义 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，用户需要按类别和发行商筛选游戏。议题描述了需求，但多个类别如何组合等细节需要在编码前确定。你将使用 Plan 模式明确这些决定，再授权 Autopilot 执行范围明确的实现。

## 背景

引入 AI 编码智能体不会改变软件开发的基本流程。常见步骤包括：从已创建的议题开始、制定实现计划、编写并审查代码、运行测试、手动验证功能、创建 PR，并在代码审查和持续集成成功后合并。

## 对话模式

**对话模式**决定智能体拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 在模式间切换：

- **Interactive**：与智能体协作；智能体提出更改并等待输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并迭代，直至任务完成。

先使用 Plan 模式审查计划，再使用 Autopilot 实现。

## 从议题开始

1. 返回 Codespace；如果已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main` 并创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中 **Allow users to filter games by category and publisher**。
9. 按 <kbd>C</kbd> 将议题加入提示并返回 **Session** 选项卡。

提示现在会包含类似 `#7` 的引用。使用 `#` 可将 GitHub 议题或 PR 加入上下文。

## 规划筛选功能

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式，并确认提示框下方显示 **Plan**。
2. 在已添加的议题引用后输入：

   ```plaintext
   Create a plan for implementing this feature.
   ```

3. 根据自己的偏好回答有关筛选行为的问题。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 浏览并审查计划。
6. 要求 Copilot 修改任何不符合你决定的部分。

## 批准 Autopilot

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示框下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 对照议题和规划时做出的决定检查结果。
4. 审查完成后按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决失败项。

## 体验新功能

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商筛选，以及计划中约定的组合。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同对话模式构建并审查功能：从 GitHub 议题开始，在 Plan 模式明确需求，在 Autopilot 模式实现，在 Interactive 模式审查，并通过转发的浏览器手动验证。

接下来，[使用自定义指令][next-lesson]深入了解如何让代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
