---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

MCP 服务器可以为 Copilot CLI 提供内置功能之外的工具和服务。本课将了解模型上下文协议 (MCP)，配置 Playwright MCP（如果尚未配置），并让智能体在浏览器中体验筛选功能。

在本课中，你将：

- 了解 MCP 及 Copilot CLI 对它的使用方式。
- 添加 Playwright MCP 服务器（如果当前不可用）。
- 请智能体操作浏览器并检查筛选功能。

## 场景

单元测试和端到端测试很重要，但验证界面更新还需要像用户一样实际操作界面。通过 MCP，Copilot 可以使用你正在开发的网站，进一步自动验证更改是否符合预期。

## 什么是模型上下文协议？

[模型上下文协议 (MCP)][mcp-blog-post] 为 AI 智能体提供与外部工具和服务通信的方式，使智能体能够访问最新信息并代表你执行操作。MCP 服务器充当智能体与外部工具和服务之间的桥梁，每个服务器提供一组可访问的工具和资源。

常见服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化能力。

GitHub 提供了 [MCP 注册表][mcp-registry]，帮助发现并贡献 MCP 服务器。

> [!CAUTION]
> 将 MCP 服务器视为项目的其他依赖项。使用前请检查源代码、验证发布者并评估安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 确认服务器类型为 **STDIO**，按 <kbd>Enter</kbd>，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 使用 Playwright 体验功能

之前你已手动确认功能行为正常。现在使用 Copilot 中的 Playwright MCP 重复验证。

1. 使用以下提示，请 Copilot 使用 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告并确认行为符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器并提供报告。

## 总结和后续步骤

你通过 Copilot CLI 使用 Playwright MCP 在真实浏览器中体验了功能，并了解了 MCP、在需要时添加服务器以及让智能体操作浏览器的流程。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能与浏览器工具结合到专门的质量流程中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
