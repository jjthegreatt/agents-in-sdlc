---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求 (PR) 前，需要确认它在浏览器中确实能正常工作。你将连接 **Playwright 模型上下文协议 (MCP) 服务器**，让 Copilot 操作真实浏览器来测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 提供新工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当代理模式可以访问编辑器以外的资源时，功能会更强大。Copilot 通过模型上下文协议 (MCP) 与外部工具和服务通信。

![示意图展示代理模式的工作机制，以及它如何与上下文、大型语言模型 (LLM) 和工具交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为人工智能 (AI) 智能体提供了与外部工具和服务通信的方式。借助 MCP，智能体可以实时获取最新信息（使用资源）并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问工具和资源。MCP 服务器充当人工智能 (AI) 智能体与外部工具和服务（例如现有应用程序编程接口 (API) 或 npm 本地工具包）之间的桥梁。每个 MCP 服务器都代表智能体可访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的应用程序编程接口 (API)，允许人工智能 (AI) 智能体创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：通过 Playwright 提供浏览器自动化功能，允许 AI 智能体访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器，可提供各种工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，方便发现并参与生态建设。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查源代码、验证发布者并考虑安全影响。仅使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件配置 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 应能看到已经配置好的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

该 `github` 配置随项目模板提供，因此 Copilot 能在上一练习中读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时要求你登录 GitHub。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控浏览器，正好可用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 配置，使文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志可让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面界面可显示浏览器的 Codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 执行端到端测试，因此通常已安装所需浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并要求你确认是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过，VS Code 会在下一步中 Copilot 首次需要其工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 操作真实浏览器。智能体可打开页面、导航、应用筛选并读取结果，然后总结观察情况，无需你亲自操作应用或离开对话，就能快速确认功能是否符合预期。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而非屏幕截图。智能体会基于带标签的结构化元素（按钮、链接、列表项）进行推理，就像辅助技术一样。因此，简单的功能检查也能顺便验证基本无障碍情况。

连接服务器并运行应用后，请 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，按步骤检查筛选功能并汇报观察结果。请将报告与议题中的验收标准对照；如果发现异常，可提出后续问题或要求 Copilot 修复代码，然后再创建 PR。

> [!NOTE]
> 测试要求应用正在 `http://localhost:4321` 运行。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示更改文件的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建 PR。也可以让已连接 GitHub 工具的 Copilot 查找相关议题并创建关联 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 发送以下提示，请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会提示你登录 GitHub；按提示授权即可。
5. 查看 Copilot 搜索议题、找到正确议题并创建 PR 的过程。
6. 选择 Copilot 生成的链接来审查 PR，但暂时**不要合并**。

## 总结和后续步骤

在本练习中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 提供新工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在浏览器中手动测试筛选功能。
- 发布了分支，并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 还有其他用途。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么受到关注？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
