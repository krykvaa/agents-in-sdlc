---
title: "第 4 课 - 使用自定义指令指导 Copilot"
description: "探索存储库指令、添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应确保 Copilot 可以获取相关指导。[指令文件][instruction-files]不仅说明您希望生成*什么*代码，也会说明代码应采用*什么*结构。现在已经构建了筛选功能，接下来查看 Copilot 使用的指令、添加文档标准并将其应用于代码。

本课将完成以下任务：

- 了解存储库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（如有可注入的 `db` 参数，也要说明）。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时应同步更新注释。
- 保留现有格式和 lint 指南。

借助指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好设置，帮助它更好地理解编码风格和要求。可以借此指定首选的编码约定、库以及注释风格，从而获得更贴合需求的建议和代码片段。您可以为整个存储库创建指令，也可以针对特定文件类型创建指令，为相应任务提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库向 Copilot 发出请求时，都会附带此指令文件。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他指令格式的支持情况各不相同。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目的自定义指令文件

项目模板已包含一组指令文件。修改文件前，先查看现有内容，以便了解更改会产生怎样的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。此处的指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了决定指令适用文件的 glob 模式。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其指向其他指令文件和项目现有文件的链接。这样可以拆分并重复使用较大的指令集，也可以为 Copilot 提供代码示例。

## 更新指令以符合团队指导

现有文件是不错的起点，但还有不足之处。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在最后一条项目符号下方添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在会话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选会话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前签出的分支为 `game-filters-cli`。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了指令和代码指导。请更新之前生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff` 并阅读已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

您了解了 Copilot CLI 如何读取指令文件，并将新标准应用到了功能中。本课完成了以下任务：

- 了解存储库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
