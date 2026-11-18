---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，您已与 GitHub Copilot 协作完成了多项任务，以改进用户体验并添加功能。您让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现无障碍访问功能（包括高对比度和浅色模式切换开关）。接下来，我们将查看代码更改，并在必要时向 Copilot 提供反馈以改进其工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。因此，在为团队其他成员启用审查前，先审查 GitHub Copilot 为创建文档、相关游戏功能和无障碍访问功能提出的更改。

## 安全性和 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此系统设置了一些安全限制，以确保操作安全：

- Copilot 只能读取存储库，并且**只能**写入用于编写代码的分支。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时工作环境。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源（包括 MCP 服务器）的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含为代码添加的文档。您将通过 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**正常现象**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。您也可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟，可以稍作休息或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览新更新的代码，其中包含新创建的 TSDoc 文档注释及其他文档。实际更改内容可能有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自您在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 您应会看到工作流在拉取请求检查区域中排队。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 进行更改

与 Copilot 协作处理拉取请求并非单向过程。您可以像在拉取请求或代码内评论中提及团队成员一样提及 Copilot。Copilot 会读取这些评论，并启动另一个会话来处理请求。由于结果具有非确定性，我们无法提供固定的提示文本。可以要求 Copilot 更新以下内容：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论并像提及用户一样标记 **@copilot**，要求它更改生成的文档。可以使用上面的建议，也可以提出其他有关代码文档的要求。
2. 选择 **View Session**，观察 Copilot 执行工作。留意 Copilot 如何启动新会话来进行更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿状态改为就绪，然后合并到主分支。

![将 PR 转为就绪状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来审查 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 您应会看到工作流在拉取请求检查区域中排队。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 并启动终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。与之前一样，您可以通过迭代让 Copilot 云智能体进行更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，分配此任务时，您还没有更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍访问功能

最后，审查使用无障碍访问自定义智能体实现的功能。此 PR 应同时包含您在练习 3 中要求的高对比度模式，以及在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其留意以下内容：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的 local storage。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍访问属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 您应会看到工作流在拉取请求检查区域中排队。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍访问功能。打开 Codespace 并启动终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    随后启动应用，并在浏览器中测试高对比度和浅色模式切换开关，确认功能正常且在重新加载页面后仍会保留设置。

可以看到，无障碍访问自定义智能体帮助 Copilot 根据无障碍最佳实践实现这些功能。如果发现无障碍问题或改进机会，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

当您把实际待办事项交给云智能体并处理其他事务时，云智能体最能发挥作用。可以创建一些新议题并分配给 Copilot，以养成这种工作习惯。以下是一些建议：

- 在游戏详情页上创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

您已完成云智能体练习。在这些课程中，您：

- **检查了此存储库自带的自定义指令文件**，以便之后查看它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**处理无障碍访问，为网站添加高对比度和浅色模式切换开关。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导正在运行的无障碍访问会话。
- **审查并迭代改进云智能体创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

您已完成云智能体练习。如果想继续探索，其他练习可以补充本次实践的内容：

- 🖥️ **[VS Code 练习](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 练习](../../cli/)** — 在终端中通过 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接，交付两个经过审查的更改。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
