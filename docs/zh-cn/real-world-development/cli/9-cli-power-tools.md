---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令用于访问高级功能、后台信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令；现在探索其他实用命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何通过 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。接下来了解一些其他功能：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令查看或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未运行，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌用量，以及系统指令、工具、消息和剩余空间在上下文中的占比。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近满载时，Copilot CLI 会自动压缩上下文。想自行选择压缩时机时，可使用 `/compact`。切换到无关任务并想开始全新对话时，可使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发者也有不同偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或超文本标记语言 (HTML) 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容前先审查。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心工作坊流程已完成。Copilot CLI 还提供了一些适合大型任务的命令：

- `/fleet` 可将独立子任务分配给子智能体并并行运行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot 云智能体；它会异步工作，并可能创建拉取请求。

这些命令是可选的，因为它们可能创建额外的工作树或触发远程工作。试用前，请先启动一个全新且范围明确的任务，并通过常规工作流审查结果。想进一步了解异步智能体工作，可继续学习[云智能体工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、共享会话并查看后台状态。在本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何通过 `/share` 导出或共享会话。
- 探索了用于并行工作、工作树和委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令可供探索。接下来，[回顾所学内容和后续步骤][next-lesson]。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
