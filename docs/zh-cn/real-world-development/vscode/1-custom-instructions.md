---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须按特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些信息。提供上下文的方式有很多，[指令文件][instruction-files]是指导代码生成的重要方式。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及路径级指令文件传递给 Copilot。
- 在当前指令下生成筛选功能的第一部分数据层代码（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加一项存储库级标准。
- 再次发送相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能基础更改提交到 `main`，供 Copilot 在下一练习中使用。

> [!CAUTION]
> 生成的代码可能不完全遵循设置的标准。Copilot 的结果具有概率性。本练习旨在观察更新指令前后的行为趋势，而非逐字匹配示例输出。

## 指令文件

### 场景

Tailspin Toys 有一套开发指南，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代风格。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应有一段注释，说明文件用途。

使用指令文件可为 Copilot 提供这些要求，帮助它按团队实践完成任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。可以为整个存储库或特定文件类型、任务提供指引。

有两类指令文件：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此文件，适合记录项目级信息，如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对特定语言（如 TypeScript 或 Astro）、文件类型或任务提供指南。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用存储库级指令，以及包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

关于创建指令文件的完整讨论超出本研讨会范围。示例项目展示了一种常见做法：

- 将 `copilot-instructions.md` 聚焦于项目级指南，如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件、数据层）或任务提供指南。
- 使用自然语言，确保指南清晰，并提供代码应有和不应有的示例。

创建指令文件没有唯一正确的方法。通过试验找出适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护完善的指令文件。浏览项目文件时，可能会发现针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本工作路径会在相关处说明。
>
> 如需模板或起点，可查看 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

先阅读存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及多个用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网站中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。它们适用于与 Copilot 的各种交互。另请留意 **Code standards** 中的 **GitHub Actions Workflows** 指南。
3. 打开 `.github/instructions` 文件夹浏览内容，其中包括 Astro、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它使用相对于存储库根目录的 glob 指定适用文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看该文件中针对项目单元测试的具体指引。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。查看指向其他指令文件（如 `unit-tests.instructions.md`）和项目文件的链接。此处路径相对于指令文件，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来将添加 TSDoc 注释和文件注释头的规则。

## 为更改创建分支

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

先在当前指令下发送提示，观察其效果。稍后更新指令后，再发送相同提示进行比较。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意生成的辅助函数是类型化函数，以 `db` 客户端为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 留意生成代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使未明确要求也可能添加文档注释。若发生这种情况也没关系；本练习关注的是更新指令后行为是否更加一致。

## 添加存储库级标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来添加标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。留意其中记录了项目编码标准，但尚无代码内文档要求。
3. 在现有标准下方添加以下内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所述，指令既可以放在存储库级 `.github/copilot-instructions.md` 中，为所有任务提供指南，也可以放在 `*.instructions.md` 文件中，针对特定语言、文件类型或任务。刚添加的文档注释规则属于项目标准，因此放在存储库级文件中。

## 重新发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成文件现在会以类似下面的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于真实代码，供下一练习继续使用。

## 提交指令并推送分支

指令文件与存储库中的其他文件一样，使用版本控制管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办事项中的文档标准议题。Copilot 可使用已连接到项目的 GitHub 工具查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按提示授权。后续练习会介绍此连接的工作方式（模型上下文协议）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选择 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 提示允许它代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并将本地分支切回 `main`：

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 提示允许执行 GitHub 操作和 shell 命令时，检查命令并酌情选择 **Allow**。

你已在 GitHub Copilot 的帮助下创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取项目指令，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察现有指令下 Copilot 的输出。
- 在 `.github/copilot-instructions.md` 中添加存储库级标准。
- 再次发送相同提示，观察生成代码如何采用新标准。
- 将指令更新和辅助函数基础更改提交到 `main`，供下一练习继续使用。

接下来使用[代理模式][next-lesson]，让 Copilot 在整个代码库中实现新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
