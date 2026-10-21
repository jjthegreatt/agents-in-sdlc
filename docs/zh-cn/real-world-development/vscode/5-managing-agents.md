---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 代理模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些操作会在本机实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以在聊天中检查每项差异、接受或拒绝单项更改，也可以通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

在本练习中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话中引导智能体，在高对比度模式旁边添加浅色模式开关。

## 场景

[练习 4][previous-lesson]中的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出了一个需求：为觉得默认主题太暗的用户添加浅色模式。无需从头开始新对话，可以引导现有会话扩展功能。

## 监控 Copilot 进度

Copilot Chat 视图会显示智能体执行的每一步，包括探索的文件、运行的命令和建议的编辑。每项文件编辑都会以内联差异和 **Keep** / **Undo** 控件显示。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容查看每次工具调用，包括文件读取、文件编辑和请求执行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在此会话中修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式。最后应会看到总结更改内容的消息。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中添加浅色模式开关。由于继续使用同一会话，智能体会保留之前的上下文，包括修改过的组件、样式方法和本地存储模式。以下提示还要求在同一用户界面 (UI) 区域提供开关。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会继续之前的工作，探索已有更改并提出浅色模式的补充实现。
3. 每项新编辑出现时都要像之前一样审查，并使用 **Keep** 或 **Undo** 处理差异。
4. 如果智能体的方向不符合预期（例如尝试以不合适的方式重构开关 UI），可使用聊天输入框旁的 **Stop** 按钮停止，然后发送澄清提示。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以通过后续提示要求调整（“简化层叠样式表 (CSS)，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板中”），或扩展范围（“也为两个开关添加键盘快捷键”）。

## 审查工作集

提交更改前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有更改文件。应能看到 Astro 组件、样式和相关测试更新。
3. 打开几个更改文件并检查差异。确认实现遵循自定义智能体提出的无障碍模式，包括无障碍富互联网应用 (ARIA) 属性、键盘导航、语义化超文本标记语言 (HTML) 和本地存储持久化。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导正在进行的会话，审查每项文件编辑、接受或拒绝更改，并发送后续提示扩展工作，同时保留会话上下文。

本课中，你了解了：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 在会话进行过程中通过后续提示引导智能体。

接下来，在[下一课][next-lesson]中将无障碍工作作为拉取请求发布，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
