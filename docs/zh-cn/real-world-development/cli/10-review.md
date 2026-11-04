---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式确定方案，在 Autopilot 模式实现，再在 Interactive 模式审查。
- 使用自定义指令引导智能体，自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整筛选功能更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR：

1. **星级评分**：在游戏卡片上显示已有 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新指令并应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相关测试。

两个 PR 都从更新后的 `main` 分支创建。筛选功能从规划到创建 PR 使用同一对话和分支，以简化研讨会流程。

## 不同类型的验证

你通过自动化测试、浏览器手动检查和 Copilot 使用 MCP 进行的浏览器探索检查了代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 综合检查结果、需求和测试覆盖情况后再提交 PR。

新增测试应解决实际覆盖缺口；如果 QA 检查后不需要新增测试，也可能是合理结果。授权合并前请审查代码和证据，并在更改后更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义技能、创建 QA 配置文件并配置 MCP 服务器。根据团队需求，在不同对话中复用并调整这些自定义项。指令定义标准，技能描述可重复任务，自定义智能体定义专业角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只是智能体的总结。

根据任务选择**模式和模型**：使用 **Plan** 先制定方案，使用 **Interactive** 跟进聚焦的更改，使用 **Autopilot** 处理范围明确的任务。日常修改可选择速度较快的模型，复杂工作可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建什么、为什么构建以及如何实现，会显著影响结果。

## 继续探索

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获取另一种观点。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务交给 Copilot cloud agent。

## 后续步骤

持续使用工具是提升熟练度的最佳方式。将它用于生产代码、个人项目，或一直想构建的小应用。与团队分享经验，也向团队成员学习，并继续探索文档。

若要从终端扩展 Tailspin Toys，可继续学习可选的 [Foundry Backer Concierge 系列][foundry]。还可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot cloud agent 研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
