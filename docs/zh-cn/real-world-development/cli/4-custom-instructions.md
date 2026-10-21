---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档规范，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

上下文对于生成式人工智能 (AI) 至关重要。如果任务需要采用特定方式完成，就应让 Copilot 能够获取相应指导。[指令文件][instruction-files]不仅描述期望的代码，还会说明代码应如何组织。完成筛选功能后，你将探索 Copilot 使用的指令、添加文档规范，并将其应用到代码中。

在本课中，你将：

- 探索存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码规范。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 制定了一系列开发规范和要求，包括：

- 注释应说明意图和不明显的决策，而不是重复代码内容。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果存在可注入的 `db` 参数，也应记录。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，应同步更新注释。
- 保留现有格式和 lint 指南。

通过指令文件，可以为 Copilot 提供相关信息，使它按这些规范完成任务。

## 指令文件

自定义指令为 Copilot 提供上下文和偏好，帮助它理解代码风格和需求。指令可以说明首选编码规范、库，甚至希望添加的注释类型。可以为整个存储库创建指令，也可以针对特定文件类型提供任务级上下文。

常见的指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库向 Copilot 发出请求时，都会发送此指令文件。内容应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指南。

> [!NOTE]
> 不同环境对其他指令格式的支持有所不同。使用特定格式前，请参阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

起始项目中已包含一组指令文件。先了解现有内容，再观察更改带来的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。这些指令适用于此存储库中的所有 Copilot 交互。
4. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等相关指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它使用 glob 模式确定适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目文件的链接。这种方式可以将较大的指令集拆分为可复用的小文件，并引用示例供 Copilot 遵循。

## 更新指令文件以符合团队规范

现有文件已经打下基础，但还需要补充一项规范。修改核心的 `copilot-instructions.md`，要求新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分。
2. 在该部分最后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的指导

Copilot CLI 会在会话开始时加载存储库指令。保存指令后，退出并恢复筛选对话，这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录运行以下命令，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选对话，并确认已检出 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。注意新增的 TSDoc 注释，并确认其准确说明代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何从指令文件获取上下文，并将新规范应用到功能中。在本课中，你：

- 探索了存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码规范。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，以确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
