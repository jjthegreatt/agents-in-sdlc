---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能获取相应上下文。[指令文件][instruction-files]是提供代码生成指导的重要方式。

在本练习中，你将：

- 探索如何通过存储库自定义指令和按路径限定的指令文件，向 Copilot 提供项目上下文、编码指南和文档规范。
- 使用现有指令生成筛选功能的第一个数据切片（发布商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。
- 重新发送相同提示，观察生成的代码如何采用新规范。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能不会始终遵循你设置的规范。Copilot 具有概率性；本练习的目标是观察更新指令前后行为的整体变化，而不是逐字匹配输出。

## 指令文件

### 场景

Tailspin Toys 制定了一系列开发规范和要求，包括：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应使用深色模式，并具有现代风格。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应有一段注释，说明文件用途。

借助指令文件，可以确保 Copilot 了解这些要求，并按项目实践执行任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定编码规范、库，甚至希望添加的注释类型。指令既可适用于整个存储库，也可针对特定文件类型提供任务级上下文。

指令文件分为两类：

- `.github/copilot-instructions.md`：针对该存储库的每次请求都会发送此文件。它应包含项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定语言（如 TypeScript 或 Astro）或任务（如创建 UI 组件或单元测试）提供指南。

> [!NOTE]
> 在集成开发环境 (IDE) 中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一处编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持[其他规范的指令文件][custom-instructions-support]，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本工作坊不详细讲解指令文件的编写方法，但示例项目展示了一种常见做法：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目说明、结构和全局编码规范。
- 使用 `*.instructions.md` 文件为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清晰，并提供正反示例。

指令文件没有唯一的组织方式，AI 的使用方式也不止一种。可以通过实践找出适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护完善的指令文件。浏览项目中的指令时，你可能会看到用户界面 (UI) 更新和 [Astro][astro-instructions] 等任务对应的文件。
>
> Copilot 也可以帮助生成指令文件。不同环境的操作有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应的练习会在相关步骤中说明。
>
> 如需模板或入门示例，可探索包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

先阅读存储库中自带的指令文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。注意项目为 Astro、Drizzle 数据层、测试等提供了专用指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它通过相对于存储库根目录的 glob 模式指定适用文件；例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看项目中用于创建单元测试的专用指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目文件的链接。这样可以将大型指令集拆分为可复用的小文件，并让 Copilot 参考现有示例。此处的路径相对于指令文件所在目录，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码文档。接下来会加入 TSDoc 文档注释和文件注释标题的规范。

## 创建更改分支

遵循最佳实践，为这项更改创建一个分支。

1. 返回上一个练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始干净的对话。不同练习使用的模式和模型有所不同；相关步骤会在适用时说明。

先使用当前指令文件发送提示，观察自定义指令的影响；稍后更新指令并重新发送相同提示，以比较差异。

1. 关闭之前练习遗留的编辑器选项卡，避免 Copilot 获取无关上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意，建议的辅助函数是带类型的函数：它将 `db` 客户端作为第一个参数，并返回带类型的发布商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层规范。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释标题。

> [!CAUTION]
> Copilot 具有概率性，因此即使未要求，它也可能添加文档注释。这种情况没有问题；本练习关注的是更新指令后行为是否更一致。

## 添加新的存储库规范

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码规范，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常接近第 27 行）。注意该部分记录了编码规范，但尚未要求代码文档，因此前一步生成的辅助函数没有文档注释。
3. 在现有规范下方添加以下 Markdown，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级别（`.github/copilot-instructions.md`）以提供全局指导，也可以放在 `*.instructions.md` 文件中以适用于特定语言、文件类型或任务。项目级规范（例如文档注释规则）应放在存储库级文件中。

## 重新发送提示并观察变化

现在重新发送相同提示，观察 Copilot 是否会应用新规范。

1. 返回 Copilot Chat，选择 **New Chat** 清空上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 重新发送之前的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意建议的文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意建议的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目规范，并将其应用到下一练习会继续使用的代码中。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，也由源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，接下来创建拉取请求，并将其与待办事项中的文档规范议题关联。Copilot 可以使用项目已连接的 GitHub 工具查找议题、创建 PR 并将其合并。

> [!NOTE]
> 这是本工作坊中第一次让 Copilot 代表你操作 GitHub。项目模板已配置工作区中的 GitHub 工具，无需额外设置。Copilot 首次使用其中一个工具时，VS Code 可能会提示登录 GitHub；请按提示授权。稍后练习会介绍此连接使用的模型上下文协议 (MCP)。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的议题，并根据当前分支创建 PR，说明指令更新和新的发布商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 请求代表你执行 GitHub 操作时，请审查命令，并在适当时选择 **Allow**。
5. PR 创建后，请 Copilot 合并 PR 并将当前分支恢复为 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot 请求执行 GitHub 操作和 shell 命令时，请审查命令，并在适当时选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你探索了 Copilot 如何从项目指令文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成以下工作：

- 发送代码生成提示，观察 Copilot 如何依据**现有**指令生成代码。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。
- 重新发送相同提示，观察建议的代码如何采用新规范。
- 提交指令更新和辅助函数基础，以便下一练习继续构建。

接下来，在[代理模式][next-lesson]中应用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
