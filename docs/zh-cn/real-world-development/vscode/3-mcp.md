---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

您刚才使用智能体模式构建了筛选功能。创建拉取请求前，应先确认该功能在浏览器中可以正常运行。无需手动点击应用来测试，而是连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器为您测试功能，然后发布分支并创建 PR。

本练习将完成以下任务：

- 了解什么是模型上下文协议 (MCP)，以及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用 Playwright MCP 服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当智能体模式能够访问编辑器以外的资源时，它的能力会大幅增强。模型上下文协议 (MCP) 为 Copilot 提供了这种能力，是智能体与外部工具和服务通信的标准方式。

![智能体模式内部工作原理及其如何与上下文、LLM 和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问外部工具和服务。这使它们能够使用资源获取最新信息，并使用工具代您执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理双方之间的通信，例如使用现有 API 或 NPM 包等本地工具。每个 MCP 服务器都提供一组不同的工具和资源，供 AI 智能体访问。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体可以创建新存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让 AI 智能体可以打开网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可以访问不同工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，方便用户发现并参与生态系统建设。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样谨慎对待 MCP 服务器。使用前，请仔细审查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 应该会看到已配置好的 `github` 服务器：

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

项目模板已包含 `github` 条目，因此 Copilot 能在上一练习中读取您的待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 通过 HTTP 连接到该服务器，并在 Copilot 首次使用其工具时要求您通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好能用于测试您的功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件内容如下：

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

与远程 `github` 服务器不同，`playwright` 是一台**本地**服务器：VS Code 会在您的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记要求 Playwright 在不显示窗口的情况下运行浏览器，这对于没有桌面环境的 codespace 是必要的。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器提供的工具时，VS Code 会启动服务器并询问您是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查其配置，然后选择信任，以便启动服务器。

手动启动服务器是可选操作。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要其工具时启动服务器，并提示您信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了可以操作的真实浏览器。无需亲自点击应用检查工作，智能体可以打开网页、导航、应用筛选条件并读取结果，然后向您总结观察到的内容。无需离开对话，即可快速确认功能是否符合预期。

Playwright MCP 服务器使用网页的[无障碍访问树][playwright-mcp-server]，而不是截图。这意味着智能体会根据带有标签的结构化元素（按钮、链接和列表项）进行推理，与辅助技术的工作方式相同。因此，快速功能测试也能顺便检查一些基本的无障碍访问情况。

连接服务器并启动应用后，要求 Copilot 测试刚刚构建的筛选功能：

```text
使用 Playwright MCP 服务器，在 http://localhost:4321 打开正在运行的应用，并验证新的游戏筛选功能：

1. 前往游戏页面并记录列出了多少款游戏。
2. 应用类别筛选，并确认列表只显示该类别中的游戏。
3. 清除筛选条件，再应用发行商筛选，并确认列表只显示该发行商的游戏。
4. 同时应用类别和发行商筛选，并确认结果同时符合两个条件。

报告每个步骤中的观察结果，并指出任何行为异常之处。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐项完成测试步骤，并报告结果。根据议题中的验收标准审查其总结。如果发现问题，可以继续询问，或让 Copilot 修复代码后再创建拉取请求。

> [!NOTE]
> 执行测试时，应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器。如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，让团队审查更改。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如访问 github.com 或使用 GitHub 命令行界面 (CLI)。既然已在使用 GitHub Copilot，不妨让它代为创建 PR。它可以找到相关议题并将 PR 与议题关联起来。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在智能体下拉列表中选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 要求 Copilot 创建 PR：

   ```plaintext
   在存储库中查找与按类别和发行商筛选相关的议题。从当前分支创建新的拉取请求，并将它关联到正确的议题。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求您通过 GitHub 身份验证，请按照提示操作。
5. 留意 Copilot 如何搜索议题、找到正确的议题并创建 PR。
6. 选择 Copilot 生成的链接来审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习完成了以下任务：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在浏览器中手动测试筛选功能，然后再发布。
- 发布分支并为筛选工作创建拉取请求。

您使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍访问审查等专门任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
