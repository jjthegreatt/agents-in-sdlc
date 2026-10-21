---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是面向开发工作流中特定任务或领域的专业人工智能 (AI) 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，可以为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地执行特定类型的工作。团队可以将专业经验封装成可复用智能体，例如确保符合 [Web 内容无障碍指南 (WCAG)][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML Ain't Markup Language (YAML) frontmatter（至少包含 `name` 和 `description`），后续 Markdown 提示会定义智能体的行为、专业知识和指令。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]有一些相似之处：两者主要通过 Markdown 文件定义，并会告诉人工智能 (AI) 如何执行操作。最简单的区分方式是：**自定义智能体是工作者，技能是工具。**

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；它也可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方式。与人工智能的其他用法一样，应通过测试和迭代找到适合环境和场景的做法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，部分用户觉得当前深色主题难以阅读，因为文本与背景的对比度不足。设计团队因此要求添加可开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义智能体生成实现。

在本练习中，你将：

- 审查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

项目已提供无障碍自定义智能体。先审查其内容，了解它如何引导 Copilot。

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览并审查后续部分，了解其中包括：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - 超文本标记语言 (HTML)、层叠样式表 (CSS) 和 JavaScript 代码示例。
   - 常见问题和错误清单。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，对话会使用其指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始干净的对话。不同练习使用的模式和模型有所不同；相关步骤会在适用时说明。

1. 如果 Chat 视图中尚未选中 **Agent**，请在智能体下拉列表中选择它。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表；默认显示当前智能体 **default**。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中内联显示文件路径和差异，供你审查。
6. 智能体修改文件时，**Files changed** 指示器会更新，显示它更改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际更改存储库，例如编辑 Astro 组件、CSS 和相关测试。

下一练习会审查并引导这项正在进行的工作。

## 总结和后续步骤

本课探索了 [GitHub Copilot 自定义智能体][custom-agents]，即面向特定任务和领域的专业 AI 助手。借助自定义智能体，可以将团队专业知识和标准封装成可复用配置，引导 Copilot 更有效地完成工作。

你了解了以下内容：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat 代理模式中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
