---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流中使用了 GitHub Copilot CLI。你已完成以下工作：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到预先创建的筛选议题。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题出发，在 Plan 模式中确定方案，在 Autopilot 模式中构建，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并通过真实浏览器检查筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、覆盖范围、技能运行结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 交付的内容

本工作坊包含两个 PR 阶段，每个阶段都基于更新后的 `main` 创建独立分支：

1. **星级评分**：在游戏卡片上显示现有 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建 PR，你始终使用同一个对话和分支。为简化工作坊流程，这些更改合并在同一个 PR 中。

## 不同的验证方式

你通过多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前汇总这些结果、需求审查和测试覆盖情况。

新增测试应弥补真实缺口；如果 QA 检查后认为无需新增测试，也可能是合理结果。授权合并前，先审查代码和证据。代码更改后，应重新获取受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。指令定义规范，技能描述可重复任务，自定义智能体定义专家角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只依赖智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前理清方案，使用 **Interactive** 持续参与范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。常规编辑可选择更快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清楚说明要构建**什么**、**为什么**以及**如何**构建，会显著影响输出。

## 更多探索

核心工作流已经完成。还可以进一步了解以下 CLI 功能：

- `/review`：请代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获取另一个视角。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

持续使用工具，是提升熟练度的最佳方式。可以用它处理生产代码、业余项目，或一直想构建却未着手的小应用。与团队分享经验、向团队成员学习，并继续探索文档。

如果要从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。也可以探索其他环境：[VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
