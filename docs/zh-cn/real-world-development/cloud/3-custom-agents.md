---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，可为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成工作。团队可将专业经验整理为可复用智能体，例如执行 [WCAG][wcag] 合规检查的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可全局存放在 `~/.copilot/agents`。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面是定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是可调用的工具。**

自定义智能体拥有自己的上下文窗口，可在工作中协调技能，甚至协调其他智能体。本实验中的无障碍自定义智能体会根据无障碍指南审查和更新网站，并可调用拉取请求工作流或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

在本练习中，你将了解如何定义自定义智能体，以及如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，当前深色主题中的文字与背景对比度不足，导致部分用户难以阅读。设计团队因此要求添加可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。本练习将使用自定义智能体生成相关功能。

## 查看无障碍自定义智能体

项目已为无障碍任务创建自定义智能体。先查看它的内容，了解其如何引导 Copilot。

返回 Codespace，查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览后续内容，了解其中列出的：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。你可以将任务分配给 Copilot cloud agent、监控任务并引导会话。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![cloud agent 分配界面截图，其中高亮显示自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

你探索了 GitHub Copilot 中的[自定义智能体][custom-agents]，了解了如何将团队专业知识和标准整理为可复用智能体，并将高对比度模式议题分配给无障碍智能体。

Copilot 正在实现高对比度模式。接下来从任务控制中心[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
