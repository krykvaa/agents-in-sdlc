---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前面的几项练习中，您让 Copilot 云智能体处理了三个不同任务，以改进用户体验并添加功能。云智能体虽以异步、自主方式运行，但监控这些任务仍然很重要。

有多种工具可用于管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，可以查看所有带有未关闭拉取请求的智能体任务、探索已执行的操作，甚至引导正在进行的会话。

本课将完成以下任务：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以趁此机会添加浅色模式。既然正在更新网站样式并添加切换功能，将浅色模式一并加入也很合理。现在您要引导智能体的工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含分配给 Copilot 的任务，例如：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择正在运行的任务之一，查看 Copilot 已执行的操作，例如：
    - 从存储库签出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 根据 Copilot 的工作状态和所采用的方法，列出的步骤可能会有所不同。

4. 同时留意右侧显示的拉取请求 (PR) 窗格。在此处可以查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

现在您已了解当前正在执行的任务，接下来要求 Copilot 在处理高对比度模式时添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。此时应会看到类似以下内容的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    我们在添加高对比度模式的同时，也添加浅色模式。还应提供一个开关，让用户选择所需的显示模式。
    ```

    ![智能体页面截图，突出显示引导活动中的云智能体会话对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 等待 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务。发送消息时，它会先完成当前步骤，然后将新请求加入工作流程。

和之前一样，这需要几分钟。等待期间，可以暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面——监控和引导 GitHub Copilot 云智能体任务的中心位置。在此任务控制中心中，可以跟踪所有活动和已完成的任务、审查智能体正在执行的工作，甚至重定向进行中的任务以调整范围或提供其他指导。

您了解了以下内容：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能的工作。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
