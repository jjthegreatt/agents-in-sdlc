---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你要求 Copilot 云智能体执行了三项独立任务，重点是改善用户体验和增加功能。云智能体可以异步自主工作，但仍需监控任务进展。

你可以通过多种工具管理分配给云智能体的任务，例如 GitHub.com 上的[智能体页面][agents-page]。在此任务控制中心，可以查看所有拥有未关闭拉取请求 (PR) 的智能体任务、了解其执行的操作，还可以引导正在进行的会话。

在本练习中，你将：

- 使用智能体页面监控云智能体任务。
- 引导正在进行的会话，请求添加更多功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队认为也适合添加浅色模式。网站样式和开关功能正在更新，因此希望引导智能体同时实现高对比度模式和浅色模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到已分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成用于修改游戏的应用程序编程接口 (API)。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，审查 Copilot 执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置模型上下文协议 (MCP) 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 工作状态和采用的方法而变化。

4. 查看右侧的拉取请求 (PR) 面板，其中显示 PR 和已更改文件，可用于进一步监控。

## 引导云智能体

查看当前任务后，请 Copilot 在处理高对比度模式时也加入浅色模式开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已完成设置并开始工作；这时会看到类似示例中的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中突出显示 steer active session while copilot is working 对话框的云智能体任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务。完成发送提示时正在执行的步骤后，它会将新请求纳入后续流程。

与之前一样，处理可能需要几分钟。等待期间，可以回顾此前学到的内容。

## 总结和后续步骤

本练习探索了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以通过任务控制中心跟踪进行中和已完成的任务、审查执行情况，以及重定向进行中的任务以调整范围或补充指导。

本练习中，你：

- 探索任务控制中心和智能体页面，以监控云智能体任务。
- 重定向正在进行的会话，请求添加更多功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
