---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀 CLI 工具一样，GitHub Copilot CLI 提供许多斜杠命令，可用于访问高级功能、查看内部状态和调整配置。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr`。本课将探索其他实用命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索并行工作、工作树和委派给 cloud agent 的可选命令。

## 场景

核心 CLI 工作流已完成。现在了解上下文管理、切换模型、共享会话，以及可选地委派工作给 [Copilot cloud agent][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理大型或复杂任务时，可能会接近模型上下文窗口的上限。Copilot CLI 会在需要时自动压缩对话；你也可以通过斜杠命令查看或手动压缩。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及系统指令、工具、消息和剩余空间的上下文分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话原本很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。需要自行选择时机时使用 `/compact`。切换到无关任务并希望使用全新对话时，使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者也有不同偏好。Copilot CLI 可以列出并选择模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员共享经验有助于共同改进 AI 工具的使用方式。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前版本中的 `/share` 选项。
2. 如果要共享会话，请选择合适的目标，例如 `/share file` 将内容导出为本地 Markdown 文件。
3. 发送或发布前先检查导出内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展并委派工作

核心研讨会已完成。Copilot CLI 还提供适用于较大任务的命令：

- `/fleet` 可将独立子任务分配给子智能体并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给异步工作的 Copilot cloud agent，它可能会创建 PR。

这些命令是可选的，因为它们可能创建额外工作树或远程工作。尝试前先启动一个范围明确的新任务，并按常规流程审查结果。若要深入了解异步智能体工作，可继续学习 [cloud agent 研讨会][cloud-workshop]。

## 总结和后续步骤

你使用 `/context` 和 `/compact` 查看上下文管理，使用 `/model` 查看模型，了解了 `/share`，并探索了并行任务、工作树和 cloud agent 委派等可选命令。

接下来，[回顾所学内容][next-lesson]并了解后续学习方向。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
