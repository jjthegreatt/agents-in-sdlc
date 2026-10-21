---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并运行测试。开发者通常需要找到相关文件、进行更改、运行测试、调试、发现遗漏的文件，再次修改……过程可能不断重复。

Copilot 代理模式正是为此而设计的。

代理模式更自主地在集成开发环境 (IDE) 中工作，类似开发者：先探索项目结构，再进行所需修改、运行测试等任务，并自动修复发现的问题。接下来使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体名称相似，但适用于不同场景。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 使用 Copilot 代理模式探索项目、定位相关文件并协调修改。
- 使用 GitHub Copilot 代理模式跨用户界面 (UI) 和数据层实现新功能。
- 在将代码合并到代码库前，审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表变长，你希望允许用户按类别和发布商筛选。上一练习中已添加发布商辅助函数；现在使用 Copilot 代理模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

进行更改前，先了解 Tailspin Toys 网站当前提供的功能。

该网站是一个以开发者为主题的棋盘游戏众筹平台，可列出游戏并显示游戏详情。它是一个 Astro 应用，会在构建时将页面渲染为静态超文本标记语言 (HTML)。页面在 frontmatter 中通过 Drizzle ORM 直接查询本地 SQLite 数据库；没有独立的后端应用程序编程接口 (API) 或客户端用户界面 (UI) 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能通过范围明确的 Astro `<script>` 和标准文档对象模型 (DOM) API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习后应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，网站服务器现在由 Codespace 托管。

### 浏览网站

网站启动后，浏览其现有功能：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、说明、发布商和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始干净的对话。不同练习使用的模式和模型有所不同；相关步骤会在适用时说明。

网站当前版本可正常运行，但还需要增加功能。先查看待办事项。使用模板设置项目时，系统已自动创建一组议题；请 GitHub Copilot 显示这些条目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划和实现跨文件更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并不偏好某个模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此教程采用了该模型，希望代码建议较为一致。由于大型语言模型 (LLM) 具有概率性，你收到的建议可能与示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于大型语言模型 (LLM) 的概率性，Copilot 可能会使用不同的模型上下文协议 (MCP) 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 查看议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 审查生成的议题列表。

注意，Copilot 还根据它认为对用户最有用的程度对条目进行了排序。

## 审查指令文件

让智能体生成代码之前，先查看为 Copilot 提供上下文的指令文件。本练习会使用[用户界面 (UI)][ui-instructions] 指令文件，其中说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向 Astro、Tailwind CSS 和 Drizzle 等文件类型专用指令的链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在目录。这样可以复用指令、将复杂内容拆分为易于管理的小文件，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要更新应用的三个部分：

- 添加或完善数据层 (`src/lib/`) 中剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页以显示筛选 UI。

合并前还必须运行并通过测试。Copilot 代理模式可以完成这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，再选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选中 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。处理过程中，编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo**，因此无需在处理中途选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中询问是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，也可能出现测试失败。这没有问题；Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务并且未检测到错误。

   ![Copilot 代理模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常且代码已审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。你了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 让 Copilot 代理模式探索项目、定位相关文件并协调修改。
- 在将代码合并到代码库前，审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为此功能创建拉取请求。

### 附加探索练习：实现分页

随着游戏列表增长，需要启用分页。使用本练习中学到的方法，请 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 决定分页的实现方式，例如允许用户选择每页大小，或固定每页大小。
- 提供 Copilot 所需的指导，确保实现符合你的预期。
- 可能需要反复迭代，要求 GitHub Copilot 修改并补充上下文。这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
