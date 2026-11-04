---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支上。现在一起审查这些内容并创建拉取请求。星级评分 PR 是你手动合并的；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 实际项目通常会将功能、指令、技能和 QA 智能体拆分到多个 PR 中。为简化研讨会流程，本练习将筛选和质量工作放在同一对话和分支中，并通过一个 PR 提交。

在本课中，你将：

- 了解 Agent Merge 及其如何自动化 PR 生命周期。
- 检查完整功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

你已使用 Copilot 规划、实现并验证筛选功能。Tailspin Toys 希望自动化剩余的 PR 工作，同时由开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并 PR 所需的后续工作。启用后，Copilot 会解决阻止 PR 合并的问题，例如修复持续集成 (CI) 检查失败、响应审查意见，以及在需要时变基；如果存储库允许，还会启用 GitHub 自动合并。

此前由你手动选择 **Merge pull request**。启用 Agent Merge 前，仍需先审查并确认更改已准备就绪。

## 使用 Agent Merge 管理 PR

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会跟进 PR 的持续集成流程。检查全部成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请查看报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你了解了 Agent Merge 如何自动化合并生命周期，检查了完整功能更改和验证证据，创建了筛选 PR，并在审查后启用 Agent Merge，确认 PR 已合并。

接下来，[探索更多有用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
