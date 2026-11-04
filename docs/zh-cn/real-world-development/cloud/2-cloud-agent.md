---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期搁置的功能请求。GitHub Copilot cloud agent 可自主执行代码更新和功能添加等任务。完成后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，让开发者专注于更重要的工作。

本练习将使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上几乎完全没有文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验：在每款游戏的详情页显示同类别的其他游戏，方便用户继续浏览。团队暂时不要求精致实现，只需要能用于用户体验探索和验收测试。这项工作目前受其他高优先级议题影响而受阻。

这类任务很容易被不断推迟，适合分配给 Copilot cloud agent。它可以异步处理任务，让开发者先做其他工作，之后再回来审查结果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台处理任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建用于跟踪进度的草稿拉取请求、设置环境并开始处理任务。会话进行中或完成后都可以查看；方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，不能替代思考。即使看似简单的操作，深入分析也可能很复杂，因此应留意[如何为 Copilot cloud agent 界定任务范围][cloud-agent-best-practices]。将它视为 AI 结对编程助手：分阶段工作、学习、试验并不断调整。生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已完成自定义指令练习，就已看到 `.github/copilot-instructions.md` 的效果；否则可在此快速查看。分配任务前，先只读查看存储库中已有的指令文件，以便稍后识别其影响。

在 GitHub 网站上打开以下文件，或者使用已启动的 Codespace：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解这些指令如何应用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给 cloud agent。查看它生成的 PR，留意 TSDoc 文档注释、TypeScript 约定和文件注释头；这些要求来自上述指令文件。后续练习会再次检查这些内容。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行设置脚本，确保环境就绪。向 Copilot 分配任务时也一样，它会像软件工程师一样完成这些工作。

Cloud agent 使用 [GitHub Actions][github-actions] 环境。你可以创建专用设置工作流 [setup workflow][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，让它在开始工作前运行，以便获得所需开发工具和依赖项。实验室已预先配置此工作流，确保 Copilot 可以访问 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用所需的迁移和播种后的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的 "Actions" 选项卡中测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并初始化本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于验证工作流，而无需等待 Copilot 启动它。

## 添加文档

许多项目的文档已过时或缺失，这类技术债务常被忽视，会降低效率，也让维护代码库或引入新开发者变得更困难。Copilot 擅长生成文档，因此很适合将此任务分配给 Copilot cloud agent。它会在后台生成所需文档，之后可再回来审查。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚生成的 PR，标题可能类似 `[WIP]: Code lacks documentation`。如果列表中暂时没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 生成待办列表可能需要几分钟。它正在创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 审查列表及其计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码和理解状态、暂停思考并制定计划，以及创建代码。

任务可能需要几分钟。Copilot cloud agent 的主要用途之一是异步执行任务，让你可以处理其他工作。现在利用这一点，再分配一项任务，然后转而编写应用功能。

## 在游戏详情页添加相关游戏

GitHub Copilot cloud agent 的优势之一是可以分配并行工作：你专注于一组任务时，它处理另一组。即使为设计团队添加相关游戏区域不需要很长时间，也可将这段时间用于其他工作。现在将任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在议题的首条评论中看到一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它的工作方式类似软件工程师，无需持续监控；任务完成后再审查即可。接下来将创建并使用自定义智能体。

## 总结和后续步骤

你探索了 [GitHub Copilot cloud agent][copilot-agents]，了解了如何将议题分配给 Copilot 异步处理。它可用于解决技术债务、创建功能，或协助将代码从一种框架迁移到另一种框架。

你自定义了代码生成环境、了解了如何确保操作安全、学习了明确界定议题范围的重要性，并将议题分配给 Copilot。

cloud agent 正在后台工作，现在可以转而创建和使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并支持前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
