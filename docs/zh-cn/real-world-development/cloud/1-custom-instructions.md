---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可提供此类指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将完成以下任务：

- 了解项目上下文、编码规范和文档标准如何通过存储库自定义指令及按路径限定范围的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，这里不会运行“前后对比”提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，可以看到指令文件如何影响其结果。

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

## 添加新的存储库标准

本练习中唯一需要编辑的内容是添加一项项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后在本练习中向云智能体分配议题时，它会遵循这项规则。

编辑前，请先创建一个分支（云智能体会读取议题目标分支中的指令；即使如此，本练习仍会提交这些更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，要求在代码中使用 TSDoc 文档注释和文件注释头，以改进代码建议。

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

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。在下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云智能体才能获取这些指令。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并该请求。下一练习中云智能体处理议题时就会读取这些指令。

> [!TIP]
> 如果想在本次研讨会中直接使用 `main` 分支，可以跳过创建分支的步骤，并直接向 `main` 提交更改。创建分支是为了让研讨会流程更贴近真实项目中的工作方式。

## 总结和后续步骤

您已了解 Copilot 如何读取本项目的指令文件，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。接下来，您会看到云智能体在本练习剩余部分创建的拉取请求中如何遵循该标准。

下一步，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更有效的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
