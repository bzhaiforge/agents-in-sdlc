---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须按特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些信息。[指令文件][instruction-files]可告诉 Copilot 不仅要完成什么，也要如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及路径级指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一项存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作路径不同，本练习不会比较指令更改前后的提示效果。Copilot cloud agent 会异步处理 GitHub 议题，难以实时并排展示差异。后续审查 cloud agent 创建的 PR 时，可以看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代风格。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应有一段注释，说明文件用途。

使用指令文件可让 Copilot 按照这些实践完成任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。指令可针对整个存储库，也可针对特定文件类型或任务。

有两类指令文件：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此文件。它适合放置项目级信息，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对特定语言（如 TypeScript 或 Astro）、文件类型或任务提供指南。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用存储库级指令，以及包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

有关创建指令文件的完整讨论超出本研讨会范围。示例项目展示了一种常见做法：

- 将 `copilot-instructions.md` 聚焦于项目级指南，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件、数据层）或特定任务提供指南。
- 使用自然语言，确保指南清晰，并说明代码应有和不应有的形式。

创建指令文件没有唯一正确的方法；通过试验找出适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护完善的指令文件。浏览项目文件时，可能会发现针对 [UI][ui-instructions] 和 [Astro][astro-instructions] 等任务的指令。
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
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 指定适用文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看该文件中针对项目单元测试的具体指引。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。查看指向其他指令文件（如 `unit-tests.instructions.md`）和项目文件的链接。路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来将添加 TSDoc 注释和文件注释头的规则。

## 添加新的存储库标准

接下来要编辑 `.github/copilot-instructions.md`，添加一项全局规则，要求代码文档使用 TSDoc 注释和文件级注释头。后续将 cloud agent 分配给议题时，它会读取这项规则。

编辑前先创建分支。cloud agent 会从议题目标分支读取指令；在本练习中仍会提交更改，以保持工作清晰。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

存储库级 `.github/copilot-instructions.md` 适合记录项目指南。接下来添加代码文档标准。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应靠近第 27 行）。留意它记录了项目编码标准，但尚未规定代码内文档。
3. 在现有标准下方添加以下 Markdown：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所述，指令既可以放在存储库级 `.github/copilot-instructions.md` 中，为所有任务提供指南，也可以放在 `*.instructions.md` 文件中，针对特定语言、文件类型或任务。刚添加的文档注释规则属于项目标准，因此放在存储库级文件中。

## 提交、推送并合并指令更新

Cloud agent 会从议题目标分支读取指令。下一练习将从 `main` 分支创建任务，因此必须先将指令更新合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。之后 cloud agent 处理分配给它的议题时就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处采用分支，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取项目指令，并在 `.github/copilot-instructions.md` 中添加了存储库级标准。后续 cloud agent 创建 PR 时，可以看到这项标准的作用。

接下来，[将第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
