---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，结合议题要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

您已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选功能。现在将这些能力整合到一个具有明确 QA 流程的自定义智能体中。

本课将完成以下任务：

- 了解自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

创建拉取请求 (PR) 前，Tailspin Toys 希望一致地检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择该角色。

您使用的各项自定义功能各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具。QA 配置则告诉 Copilot 如何使用这些能力评估需求并报告发现。它不会替代这些功能，也不需要另开会话。

## 创建 QA 配置

创建功能 PR 前，让 Copilot 创建一个可复用的 QA 配置。该配置将说明 QA 要执行的检查以及必须遵守的界限。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 要求默认智能体创建新的自定义智能体：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义智能体。它应根据议题和已商定的需求检查功能、遵循存储库指令、运行 quality-checks 技能、使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   它应提供报告，将每项要求标记为通过、失败或受阻，并附上支持证据。修改实现代码前必须先询问，并且不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新智能体前，先确认 Copilot 记录了预期的 QA 流程和界限。这样可以避免在只需要验证功能时，智能体进行不完整或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 为可选项，但添加后智能体会有清晰的显示名称。
3. 阅读配置指令，确认 QA 会从需求开始、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前先询问，并且不会提交更改或创建拉取请求。
5. 如果生成的配置遗漏上述职责或界限，请先要求默认智能体修订，再继续。

## 根据议题运行 QA

Copilot CLI 会在会话开始时加载项目智能体。创建配置文件后，恢复同一筛选会话，再选择 QA，以便它使用已有的议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建智能体，它可能不会显示在可用智能体列表中，但仍然可以使用上面的命令激活。

2. 使用以下提示要求 QA 智能体审查功能：

   ```plaintext
   根据议题和计划中确定的决定审查筛选功能。它是否已准备好创建 PR？
   ```

3. QA 智能体开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

您已将可复用的专用角色加入工作流程并审查其结果。本课完成了以下任务：

- 了解自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查其发现。

实现、技能更新、QA 配置、测试和验证报告现已准备好接受审查。接下来，[将这些内容汇总到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
