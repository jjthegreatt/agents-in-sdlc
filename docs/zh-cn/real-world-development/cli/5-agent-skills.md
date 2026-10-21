---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码并非开发工作的全部。我们已手动验证代码，并使用指令文件确保代码遵循规范；接下来还要考虑测试、lint 和持续集成 (CI) 的其他部分。

**智能体技能**适合执行这类任务。技能能帮助 Copilot 正确运行这些操作。

在本课中，你将：

- 探索现有的 `quality-checks` 技能。
- 自定义结果格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 指令、脚本和资源

智能体技能会打包可复用的任务指令、可执行脚本和辅助资源，供智能体按需加载。技能通常是一个文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该文件的 frontmatter 包含技能名称和描述，并说明技能的作用及适用时机。技能文件夹也可以包含供技能使用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能目录 `~/.copilot/skills` 中。

## 探索技能

Tailspin Toys 团队创建了名为 `quality-checks` 的技能，用于运行单元测试、lint 和类型检查。先了解它的内容。

1. 返回 Codespace，在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。描述能帮助 Copilot 判断何时调用该技能。
3. 阅读指令，了解它如何引导 Copilot 执行测试和 lint。
4. 注意技能中尚无 **Results output formatting** 部分。

## 更改前先运行技能

请 Copilot 运行技能中的三项检查。

1. 返回 Interactive 模式下的筛选对话。
2. 输入以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告。

## 自定义报告

我们希望报告说明运行了什么、是否成功，以及工具实际报告了什么。现在更新技能以生成这类报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改完成后，检查新报告的效果。Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 输入与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告，并与第一次的报告对比。

## 总结和后续步骤

你自定义并使用了现有的智能体技能。在本课中，你：

- 探索了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果报告格式。
- 重新加载并运行了技能。

这项更改将与筛选功能一起包含在功能 PR 中。接下来，[通过 Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
