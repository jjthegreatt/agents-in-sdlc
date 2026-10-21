---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都需要处理技术债务，例如尚未解决的安全问题、需要更新的旧代码，或一直积压、无暇实现的功能请求。GitHub Copilot 云智能体可以自主执行代码更新和功能开发等任务。完成后，它会创建草稿拉取请求 (PR)，供开发者审查。这样可以将繁琐工作委派出去，让开发者专注于更重要的事项并加快开发流程。

本练习将探索如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇创建网站的承包商没有妥善编写文档，代码几乎完全缺少文档。团队希望首先为应用中所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验：在每款游戏的详情页显示同一类别中的其他游戏，让用户继续浏览。团队目前只需要可供验收测试的初步实现。此项工作暂时受阻，而且当前优先级低于其他议题。

这两项工作都可能被不断延后，适合分配给 Copilot 云智能体异步处理。开发者可以先处理其他任务，之后再回来审查 Copilot 的工作是否符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。可以通过多种方式分配任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿 PR 来跟踪进度、设置工作环境并开始处理任务。你可以在会话进行期间或完成后查看内容。解决方案准备好供你审查时，Copilot 会在 PR 中标记你。

## 编写范围明确的指令

GitHub Copilot 没有魔法，不能替代思考。看似简单的操作，深入分析后也会显现复杂性，因此应谨慎界定云智能体任务范围，并遵循[云智能体最佳实践][cloud-agent-best-practices]。将它视为人工智能 (AI) 结对编程伙伴：分阶段开展工作、学习、试验并调整。生成式人工智能不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其对 Copilot 的引导作用。如果已经完成自定义指令练习，就已看到 `.github/copilot-instructions.md` 的效果；否则可以借此机会快速查看。分配任务前，先只读浏览存储库已有的指令文件，以便之后观察它们的影响。

在 GitHub Web 界面中打开以下文件；如果已有运行中的 Codespace，也可在其中查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定到 `**/*.test.ts` 文件。

下一部分将把 `Code lacks documentation` 议题分配给云智能体。审查结果 PR 时，留意来自这些指令文件的 TSDoc 注释、TypeScript 规范和文件注释标题；后续练习也会再次检查。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，以确保开发依赖项状态正常。分配任务给 Copilot 时同样如此，因为它会像软件工程师一样工作。

云智能体在 GitHub Actions 中创建工作环境。可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]，在它开始工作前运行脚本并提供所需工具和依赖项。项目已预先配置该工作流，以简化实验并支持本练习。它会安装 Node.js、项目依赖项和端到端测试所需的浏览器，并为单个 Astro 应用迁移和填充 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的“Actions”选项卡手动测试设置步骤。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为这些步骤所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置：Node.js（整个应用现在使用 Astro 和 Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试使用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流文件类似，但有几个重要特点：

- 它只包含名为 `copilot-setup-steps` 的任务。Copilot 开始处理 PR 前，GitHub Actions 会运行此任务。
- `workflow_dispatch` 触发器允许你从存储库的 **Actions** 选项卡手动运行工作流。这样可以测试工作流是否成功，而无需等待 Copilot 启动它。

## 添加文档

虽然大家都了解文档的重要性，但许多项目的资料要么过时，要么完全缺失。这类技术债务经常被搁置，会降低工作效率，也增加维护代码库或新开发者加入团队的难度。Copilot 擅长创建文档，因此这是分配给云智能体的合适议题。它会在后台生成所需文档，之后可在后续练习中审查结果。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
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
11. 打开新生成的 PR，标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办任务列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的方案。

13. 审查任务列表及其计划执行的事项。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口，才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定适当方案，以及编写代码。

处理过程可能需要几分钟。云智能体的主要优势之一是异步执行任务，让开发者可以专注于其他工作。接下来利用这一点，再分配一项任务给云智能体，同时开始为应用编写功能代码。

## 为游戏详情页添加相关游戏区域

Copilot 云智能体的一大优势是可以并行分配任务，让你在处理一组任务时由它处理另一组。即使为设计团队添加相关游戏区域不需要很长时间，这段时间也可以用于其他工作。现在将此任务分配给 Copilot 云智能体。

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

不久后，议题第一条评论中会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在处理第二项请求。云智能体的工作方式类似软件工程师，因此无需持续监控；任务完成后再审查即可。现在可以开始创建并使用自定义智能体。

## 总结和后续步骤

本练习探索了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、添加新功能或协助将代码迁移到其他框架。

本练习介绍了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并能读取前面模块中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

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
