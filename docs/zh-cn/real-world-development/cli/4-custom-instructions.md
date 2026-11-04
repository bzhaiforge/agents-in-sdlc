---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。指令文件不仅说明希望生成什么代码，也说明代码应如何编写。本课将探索 Copilot 使用的指令、添加文档标准，并将其应用于筛选功能。

在本课中，你将：

- 探索存储库指令和按路径应用的指令文件如何传递给智能体。
- 更新指令文件以落实编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发指南：注释应说明意图和不明显的决策，而不是复述代码；`db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（包括可注入的 `db` 参数）；可复用 Astro 组件应说明 `Props` 契约；代码变更时应同步更新注释；现有格式和 lint 指南必须保留。使用指令文件可为 Copilot 提供这些要求。

## 指令文件

自定义指令为 Copilot 提供上下文和偏好。存储库级指令文件 `.github/copilot-instructions.md` 会用于存储库的每个请求；`.github/instructions/*.instructions.md` 则为特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指南。此文件中的指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它通过 glob 指定文件适用范围。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目文件。这种方式可将较大的指令集拆分成可复用的小文件，并提供示例。

## 更新指令以符合团队指南

现有文件还缺少一项标准：要求新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指南

Copilot CLI 会在对话开始时加载存储库指令。保存后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选对话仍在运行，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。确认新增的 TSDoc 注释能准确说明代码。

## 总结和后续步骤

你探索了 Copilot 如何读取指令文件，并将新标准应用于功能代码：查看了存储库和路径级指令、更新了编码标准，并观察其对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保以一致方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
