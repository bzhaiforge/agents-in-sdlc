---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "查看现有 quality-checks 技能、自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码只是开发工作的一部分。你已手动验证代码并使用指令文件落实标准；本课将使用**智能体技能**指导 Copilot 执行测试、lint 和其他持续集成 (CI) 任务。

在本课中，你将：

- 查看现有的 `quality-checks` 技能。
- 自定义结果的报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易读。

## 指令、脚本和资源

智能体技能将可复用的任务指令、可执行脚本和支持资源打包起来，供智能体按需加载。技能文件夹包含 `SKILL.md`，其中的 frontmatter 使用名称和描述定义技能，正文说明技能用途以及何时调用。文件夹也可以包含脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 查看技能

1. 返回 Codespace，在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。描述帮助 Copilot 判断何时调用技能。
3. 阅读指令，了解其如何引导 Copilot 执行测试和 lint。
4. 确认当前技能尚未包含 **Results output formatting** 部分。

## 修改前先运行技能

技能可以通过 Copilot CLI 命令或自然语言直接调用。先请 Copilot 运行这三项检查。

1. 返回处于 Interactive 模式的筛选对话。
2. 输入：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告。

## 自定义报告

改进报告，使其说明运行了什么、结果如何，以及工具实际报告了哪些信息。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

Copilot CLI 可以重新加载已编辑的技能，无需重启对话。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次输入相同提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看并比较这次和上次的报告。

## 总结和后续步骤

你查看了用于单元测试、lint 和类型检查的 `quality-checks` 技能，自定义了结果报告格式，然后重新加载并运行该技能。此更改会与筛选功能一同包含在功能 PR 中。

接下来，让 Copilot 直接操作网站，[配置 Playwright MCP 服务器][next-lesson]。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及其导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
