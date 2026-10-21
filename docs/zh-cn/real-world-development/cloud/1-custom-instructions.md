---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要采用特定方式完成，或 Copilot 需要了解背景信息，就应确保它能获取相应指导。[指令文件][instruction-files]可帮助 Copilot 理解你想做什么以及希望如何完成。

在本练习中，你将：

- 探索如何通过存储库自定义指令和按路径限定的指令文件，向 Copilot 提供项目上下文、编码指南和文档规范。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。

> [!NOTE]
> 与 VS Code 和命令行界面 (CLI) 工作环境不同，本练习不会使用前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此很难实时并排展示指令的影响。后续审查云智能体创建的拉取请求时，可以看到指令的作用。

## 指令文件

### 场景

Tailspin Toys 制定了一系列开发规范和要求，包括：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应使用深色模式，并具有现代风格。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应有一段注释，说明文件用途。

通过指令文件，可以确保 Copilot 了解这些要求，并按项目实践执行任务。

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
> 每个使用 GitHub Copilot 的项目都应维护完善的指令文件。浏览项目指令时，你可能会看到针对用户界面 (UI) 更新和 [Astro][astro-instructions] 等任务的专用文件。
>
> Copilot 也可以帮助生成指令文件。不同环境的操作有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`；对应的练习会在相关步骤中说明。
>
> 如需模板或入门示例，可探索包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间阅读存储库自带的指令文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。注意项目为 Astro、Drizzle 数据层、测试等提供了专用指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它通过相对于存储库根目录的 glob 模式指定适用文件；例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看项目中用于创建单元测试的专用指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目文件的链接。这样可以将大型指令集拆分为可复用的小文件，并让 Copilot 参考现有示例。此处的路径相对于指令文件所在目录，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码文档。接下来会加入 TSDoc 文档注释和文件注释标题的规范。

## 添加新的存储库规范

接下来只需编辑一次：添加一项项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释标题。后续练习中，云智能体会在处理所分配的议题时读取这项规范。

编辑前先创建分支（云智能体会读取议题目标分支上的指令；此处仍通过单独提交整洁地保存更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码规范，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常接近第 27 行）。注意该部分记录了项目编码规范，但尚未要求添加代码文档，这也是此前生成的辅助函数缺少文档注释的原因。
3. 在现有规范下方添加以下 Markdown，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所见，指令文件可以放在存储库级别（`.github/copilot-instructions.md`）以提供全局指导，也可以放在 `*.instructions.md` 文件中以适用于特定语言、文件类型或任务。项目级文件适合记录代码注释规则等全局规范。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习将把议题分配给 Copilot，届时它会从 `main` 创建分支，因此必须先将指令更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，云智能体将读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过创建分支，直接提交到 `main`。创建分支是为了模拟真实项目中的处理方式。

## 总结和后续步骤

你探索了 Copilot 如何从项目指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级规范。后续练习会看到云智能体创建的拉取请求如何体现该规范。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
