---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们还需要处理数据、外部服务，并允许 Copilot 使用更多自动化工具。MCP 服务器可以让 Copilot 超越 CLI 内置功能，访问更多工具和服务。

本课将完成以下任务：

- 了解什么是模型上下文协议 (MCP)，以及 Copilot CLI 如何使用它。
- 如果尚不可用，则添加 Playwright MCP 服务器。
- 要求智能体操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证 UI 更新，仍需实际与 UI 交互。您希望 Copilot 像用户一样使用正在开发的网站，从而进一步自动化更改流程，并更有信心地确认更新符合预期。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问最新信息，并代您执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组不同的工具和资源，供 AI 智能体访问。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，方便用户发现并参与生态系统建设。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样谨慎对待 MCP 服务器。使用前，请审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 中尚未提供 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入以下内容：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 使用 Playwright 让 Copilot 探索功能

之前您已手动确认功能符合预期。现在使用 Copilot 中的 Playwright MCP 服务器执行同样的测试。

1. 使用以下提示，要求 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用并使用 Playwright MCP，根据议题和我们的计划检查筛选功能。告诉我哪些功能正常、哪些不正常，但不要进行任何更改。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它会自行判断。不过，说明要使用的服务器有助于获得更一致的结果，也可以节省一些 token。

2. 观察 Copilot 如何列出浏览器中的各项操作步骤，以确认功能正常。
3. 阅读报告并确认所有行为符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，然后向您提供报告。

## 总结和后续步骤

恭喜，您已通过 Copilot CLI 使用 Playwright MCP 服务器在真实浏览器中探索了功能。本课完成了以下任务：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚未可用，则添加该服务器。
- 让智能体操作浏览器并探索筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专门角色中。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
