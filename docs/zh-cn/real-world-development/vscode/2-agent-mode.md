---
title: "练习 2 - 使用 Copilot Agent Mode 添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者往往需要找到相关文件、修改代码、运行测试、调试、找出遗漏的文件，再继续修改。**Copilot Agent Mode** 可以在 IDE 中更自主地处理这些工作：探索项目结构、进行必要更新、运行测试等任务，并自动修复发现的问题。本练习将使用 Agent Mode 为网站添加功能。

> [!NOTE]
> Agent Mode 和 cloud agent 虽然名称相似，但适用于不同场景。Agent Mode 在 IDE 中执行任务，可快速反馈并随时交互；cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 让 Copilot Agent Mode 探索项目、找到相关文件并协调多处更改。
- 使用 Agent Mode 跨 UI 和数据层实现新功能。
- 在合并前审查 Copilot Agent Mode 生成的更改和测试。

## 场景

随着游戏列表不断增长，需要让用户按类别和发行商筛选。上一练习已添加发行商辅助函数；现在将使用 Copilot Agent Mode 完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

开始更改前，先探索 Tailspin Toys 网站，了解现有功能。

这是一个面向开发者的桌游众筹平台，可列出游戏并显示详情。网站是一个 Astro 应用，构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能由使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下的 URL 横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace（当前运行网站服务器）的私有隧道。

### 浏览网站

网站启动后，可以浏览主要功能：

- **主页**：显示桌游列表、标题、图片和简介。
- **游戏详情页**：选择游戏后可查看标题、简介、发行商和类别等详细信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

网站当前已可使用，但还需要增加功能。先查看从模板创建项目时自动生成的议题待办事项，请 GitHub Copilot 列出这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某一种模型。编写研讨会时使用了 Claude Sonnet 4.5，因此步骤中列出该模型，希望使代码建议较为一致。由于大型语言模型 (LLM) 具有概率性，你收到的建议可能与示例不同，这是正常现象。

> [!NOTE]
> Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，询问 Copilot 有哪些待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 也会根据它认为对用户最有用的程度为事项排序。

## 查看指令文件

让智能体生成代码前，先查看用于提供上下文的指令文件。本练习将使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的总体指南，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各文件类型专用指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用内容、拆分复杂指令，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项更改：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

合并前还必须运行并通过测试。Copilot Agent Mode 可以执行这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目、查找数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有相应的 **Keep** 或 **Undo** 选项，无需在工作进行时立即选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板提示确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这是正常的；Copilot 会在生成代码和运行测试之间迭代，直到任务完成且未发现错误。

   ![Copilot Agent Mode 完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> Copilot 和其他 AI 工具生成的代码始终需要审查。

10. 返回运行着网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

你使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加了功能，了解了如何跨 UI 和数据层实现更改、探索项目并协调修改，以及在合并前审查生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分练习：实现分页

随着游戏列表增长，网站需要分页。使用本练习学到的技能，请 Copilot 为网站添加分页。考虑以下事项：

- 遵循现有最佳实践，包括使用已有指令文件。
- 决定分页方式，例如允许用户选择每页大小，或将其设为固定值。
- 编写提示时提供足够信息，说明希望如何实现。
- 你可能需要通过提供上下文和要求更改，与 GitHub Copilot 多次迭代。这是使用 Copilot 的正常方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
