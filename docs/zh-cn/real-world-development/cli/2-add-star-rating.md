---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发浏览器中检查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试对话后，现在可以开始对项目进行第一次更改。先做一个小改进：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。接下来让智能体显示评分、审查更改，并将其作为第一个拉取请求合并。

本课将完成以下任务：

- 在功能分支中开始一段专注的 Copilot 对话。
- 要求智能体对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已显示评分。不过，主页上的游戏卡片目前只显示标题、类别、发行商和说明。作为热身练习，让智能体在每张卡片上显示现有评分。这是一个小而独立的更改，非常适合第一次会话。

## 对话的组成

**对话**是您使用 Copilot CLI 完成任务的工作区。与 Copilot app 不同，普通 CLI 对话使用终端当前签出的存储库和 Git 分支，而不会创建专用工作树。保存的对话允许您稍后返回相同讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三项内容：您发送的提示和智能体的回复、智能体探索和编辑文件时执行的工具操作，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

开始新的对话，实现此功能。

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，请使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确认开始新会话。
5. 使用以下提示请求更改：

   ```plaintext
   在游戏列表页的游戏卡片中显示每款游戏的 starRating（满分为 5 分）。如果评分为 null，则显示“暂时没有评分”。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目、查找用于显示游戏详情的文件并编写必要代码。您已使用 Copilot CLI 添加了一项新功能。

## 审查差异

即使是小型更改，在合并前也需要审查所有 AI 生成的内容。现在直接在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个已更改文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示“暂时没有评分”。
3. 确认测试覆盖这两种情况。
4. 检查 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的结果具有非确定性，因此您得到的具体代码可能有所不同。请审查其行为，不要期待某种特定实现。

## 检查更改

只阅读代码并假设它可以正常运行还不够。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面 (UI)。

1. 要求 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中检查星级评分更改。告诉我 URL，并让服务器保持运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示满分为五分的评分。
4. 返回 Copilot，要求它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

功能已创建，现在创建一个拉取请求 (PR)，将新代码合并到项目中。

1. 要求默认智能体提交更改：

   ```plaintext
   为已审查的星级评分更改编写合适的提交消息并提交。
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 在 Copilot CLI 中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择显示的 URL，打开 PR。
4. 审查更改的文件和检查结果。
5. 确认可以合并后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！您已使用 GitHub Copilot CLI 发布了第一个更改。本课完成了以下任务：

- 在功能分支中开始一段专注的 Copilot 对话。
- 指示智能体修改游戏卡片。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并第一个拉取请求。

接下来，从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
