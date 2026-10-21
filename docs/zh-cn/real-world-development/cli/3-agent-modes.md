---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助团队以正确的方式构建正确的内容。本课是后续一系列课程的起点：从议题出发生成新功能，验证代码、检查功能行为，最终成功合并到项目中。

> [!NOTE]
> 后续功能工作将继续使用同一个对话和分支。实际工作中，不同文件类型通常会使用不同分支或 PR；本工作坊简化了流程，以便聚焦核心概念。

本课将帮助你：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成该功能时，你还会更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发布商筛选游戏。待办事项中的议题描述了这项功能，但组合类别等细节需要在编码前达成一致。你将使用 Plan 模式明确这些决定，再授权 Autopilot 在明确范围内实现。

## 背景

将 AI 编码智能体引入开发流程，并不会改变软件开发的基本原则；这些原则反而更加重要。大多数开发者的流程大致如下：

1. 从描述待办工作的议题开始。
2. 规划要构建的内容。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多与上述流程类似。

遵循标准流程，可以确保 AI 生成的代码满足需求，并像手写代码一样经过审核。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换模式：

- **Interactive**：你与智能体协作；智能体提出更改并等待你的输入后再继续。
- **Plan**：智能体先制定计划，在此模式下不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并迭代，直到任务完成。

先从 Plan 模式开始并审查计划，然后再用 Autopilot 实现。

## 从议题开始

开始筛选功能之前，先返回 Codespace 并确认存储库和终端已就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中并返回 **Session** 选项卡。

现在提示开头会显示 `#7` 或类似编号。`#` 可将 GitHub 上的议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划让你有机会在交由 Copilot 执行之前，确定功能实现或任务处理方案。复杂任务尤其适合先花时间规划。现在切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实现方案。过程中，它可能会询问筛选功能应如何工作；请根据自己的偏好回答，没有唯一正确的答案。

3. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
4. 上下滚动审查计划。
5. 如果计划中的任何内容不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划制定并审查完成后，就可以开始实现。Autopilot 会让 Copilot 反复处理问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中含义相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按照计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再判断检查是否完成。

## 审查并验证实现

代码生成后，必须像其他代码一样在合并前审查。接下来审查代码并运行网站，确认效果正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将实现与议题以及规划时确定的要求进行对照。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目检查的结果，并要求 Copilot 解决失败项。

## 体验新功能

代码看起来没问题后，还要确认它确实能运行。像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发布商筛选，以及按计划确定的组合条件筛选。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。在本课中，你：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，[使用自定义指令][next-lesson]，进一步了解如何确保代码遵循已记录的开发规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
