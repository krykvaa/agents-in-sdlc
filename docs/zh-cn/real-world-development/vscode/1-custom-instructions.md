---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]指导代码生成。

本练习将完成以下任务：

- 了解项目上下文、编码规范和文档标准如何通过存储库自定义指令及按路径限定范围的指令文件传递给 Copilot。
- 使用当前指令生成用于筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。
- 重新运行相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合您设置的所有标准。Copilot 的结果具有非确定性。本练习的目的在于观察更新指令后行为的**整体变化趋势**，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应添加一段注释，说明该文件的用途。

借助指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好设置，帮助它更好地理解编码风格和要求。可以借此指定首选的编码约定、库以及注释风格，从而获得更贴合需求的建议和代码片段。您可以为整个存储库创建指令，也可以针对特定文件类型创建指令，为相应任务提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库向 Copilot 发出请求时，都会附带此指令文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如 TypeScript 或 Astro 文件，或者创建 UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

创建指令文件的完整介绍超出了本研讨会的范围。示例项目展示了具有代表性的方法，主要原则如下：

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，并通过示例清楚说明应如何编写代码以及应避免哪些做法。

指令文件没有唯一正确的写法，AI 的使用方式也不止一种。通过实践可以找到适合项目和场景的方法。

> [!TIP]
> 使用 GitHub Copilot 的项目都应准备一套完善的指令文件。浏览本项目中的文件时，可以看到针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会在相关处介绍具体方式。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目的自定义指令文件

花些时间阅读此存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。查看 **Code standards** 下的 **GitHub Actions Workflows** 指导。这些内容适用于您与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob 模式，决定该指令适用于哪些文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会符合条件。
5. 查看此项目中有关创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。此处的路径相对于指令文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求在代码中编写文档。接下来，您将添加 TSDoc 文档注释和文件注释头的规则。

## 创建更改分支

遵循最佳实践，为更改创建一个分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的对话。不同练习所用模式和模型各不相同；相关步骤会在适用处说明。

先发送提示，观察当前指令文件的效果。稍后更新指令后，再次发送相同提示，比较两次结果。

1. 关闭之前练习中打开的编辑器选项卡，确保 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   在 src/lib/publishers.ts 中创建或更新一个数据访问辅助函数，返回所有发行商的列表，每个发行商包含名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个类型化函数：第一个参数是 `db` 客户端，返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md`（适用于 `src/lib/*.ts`）中的数据层约定。
7. 留意建议代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的结果具有非确定性，因此即使未明确要求，它也可能添加文档注释。如果发生这种情况也没关系，本练习重点是观察更新指令后的**一致性**改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应位于第 27 行附近）。此处记录了项目编码标准，但尚无代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，以要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中创建全局指令，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指令。存储库级文件适合放置您刚添加的文档注释等项目级标准。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦到正确的文件。
3. 发送**相同的提示**：

   ```plaintext
   在 src/lib/publishers.ts 中创建或更新一个数据访问辅助函数，返回所有发行商的列表，每个发行商包含名称和 ID。应用文件更改。
   ```

4. 留意建议文件现在以类似以下内容的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 留意建议函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含 ID 和名称的所有发行商列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

您刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会在此基础上继续构建。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，也需要使用版本控制管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新的终端窗口。
2. 在终端中运行以下命令，确认指令和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有文件更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更改（例如测试），也请暂存这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并将其关联到待办事项中的文档标准议题。也可以手动完成，但项目已连接 GitHub 工具，因此让 Copilot 帮忙查找议题、创建用于关闭该议题的 PR，然后合并它。

> [!NOTE]
> 这是第一次让 Copilot 代表您在 GitHub 上操作。项目模板已将 GitHub 工具连接到工作区，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示您登录 GitHub。按照提示完成登录。后续练习会介绍此连接的工作方式（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选择 **Agent**。
3. 要求 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的 publisher 辅助函数基础：

   ```
   查找与更新指令文件相关的议题。从当前分支创建新的 PR，说明该 PR 会关闭此议题，并注明我们还为后续筛选工作添加了发行商辅助函数基础。
   ```

4. Copilot 开始查找议题并创建 PR。
5. 当 Copilot 提示允许其代表您执行 GitHub 操作时，请先检查命令，再酌情选择 **Allow**。
6. 创建 PR 后，要求 Copilot 合并 PR 并让本地分支返回 main，发送以下提示：

   ```
   将 PR 合并到 main。然后在本地切回 main，并拉取最新代码，使本地内容保持最新。
   ```

7. 当 Copilot 提示允许其代表您执行 GitHub 操作和运行 shell 命令时，请先检查命令，再酌情选择 **Allow**。

现在，您已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

您已了解 Copilot 如何读取本项目的指令文件，并在 VS Code 中使用 Copilot Chat 完成以下任务：

- 发送代码生成提示，观察 Copilot 如何遵循**现有**指令。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 重新发送相同提示，观察建议的代码如何遵循新标准。
- 将指令更新和辅助函数基础提交到 `main`，以便下一练习在此基础上继续构建。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 在整个代码库中添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更有效的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
