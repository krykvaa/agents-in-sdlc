---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。任务完成后，它会创建一个草稿拉取请求，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发人员专注于更重要的工作。

本练习将探索以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 编写范围明确的议题为何重要。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商留下的文档状况不佳——实际上，您会发现代码完全缺少文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现功能。他们希望在每款游戏的详情页面上展示相关游戏，即同一类别中的其他游戏，以便访客继续浏览。他们暂时不需要完善的实现，只需要一个可用于验收测试用户体验的版本。目前这项工作受到了阻碍，但优先级较高的其他议题需要先处理。

这两项工作都很容易被降级到较低优先级，因此很适合分配给 Copilot 云智能体。它可以异步处理任务，让开发人员继续完成其他工作，之后再回来审查 Copilot 的结果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。与开发人员协作类似，可以通过多种方式启动任务，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。您可以在 Copilot 会话运行期间或完成后查看详情。解决方案准备好供您审查时，Copilot 会在拉取请求中提及您。

## 编写范围明确的指令

GitHub Copilot 并非魔法工具，使用它并不能跳过思考。看似简单的操作也可能隐藏复杂问题，因此请[谨慎限定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、尝试并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其如何指导 Copilot。如果已经完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。将工作分配给 Copilot 云智能体前，请只读浏览本存储库现有的指令文件，以便稍后观察它们的影响。

在 GitHub Web 界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自指令文件，我们会在之后的练习中审查拉取请求时再次介绍。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要配置特定环境并运行一些设置脚本，确保环境就绪。将任务分配给以类似软件工程师方式工作的 Copilot 时也是如此。

云智能体使用 [GitHub Actions][github-actions] 执行任务。您可以创建一个[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，让它在智能体开始工作前运行，以便安装所需的开发工具和依赖项。本实验已预先配置此工作流，以确保流程顺利并提供实践机会。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤实际需要的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在都是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 留意 `workflow_dispatch` 触发器。它允许您从存储库的 **Actions** 选项卡手动运行工作流，以便验证工作流是否成功，而不必等待 Copilot 运行它。

## 添加文档

大家都知道文档的重要性，但大多数项目的信息要么已过时，要么完全缺失。这类技术债务经常得不到处理，会拖慢工作效率，并增加维护代码库或帮助新开发人员加入团队的难度。Copilot 擅长创建文档，因此很适合将这项工作分配给 Copilot 云智能体。它会在后台生成所需文档，您可以在后续练习中回来审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    我们组织要求在有助于理解代码时，为函数和方法添加 TSDoc 文档注释。但最近的更新没有遵循这项标准。请更新现有代码，在能帮助说明行为的地方添加文档注释。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 创建待办事项列表可能需要几分钟。它需要设置环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 审查列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 您可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适的方案，以及创建代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们能专注于其他工作。现在就利用这一点：先将另一项任务分配给 Copilot 云智能体，然后自己编写代码为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：您可以处理一组任务，而智能体处理另一组任务。为设计团队添加相关游戏部分可能不会花太多时间，但也可以把这段时间用于其他工作。现在将此任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    我们希望在每款游戏的详情页上展示相关游戏，帮助访客发现更多游戏。设计团队想要探索用户体验并进行验收测试。要求如下：

   - 在 `src/lib/` 中添加数据访问辅助函数，返回与指定游戏属于同一类别的其他游戏，但不包括该游戏本身
   - 在游戏详情页上添加“相关游戏”部分，并使用此辅助函数
   - 处理游戏没有相关游戏的情况
   - 为新辅助函数创建单元测试
   - 创建 PR 前，确保所有测试均已通过
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题中的第一条评论旁会出现 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二项请求。它的工作方式类似软件工程师，因此无需一直监控，等完成后再审查即可。接下来，我们将了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理任务。您可以用 Copilot 解决技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

您了解了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 编写范围明确的议题为何重要。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，接下来可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用我们在前面模块中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
