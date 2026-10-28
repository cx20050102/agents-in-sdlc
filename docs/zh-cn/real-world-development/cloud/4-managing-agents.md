---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 Copilot 云智能体处理了三项不同任务，改善用户体验并添加功能。云智能体虽然能够自主异步工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有创建了未关闭拉取请求（PR）的智能体任务，检查已执行的操作，甚至引导正在进行的会话。

在本课中，你将：

- 查看智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将实现高对比度模式的任务交给智能体后，团队发现也适合同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，可以顺便实现这个模式。现在引导智能体，要求它除高对比度模式外也添加浅色模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成用于修改游戏的应用程序编程接口（API）。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 的工作环境。
   - 设置模型上下文协议（MCP）服务器。
   - 执行完成分配任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和执行方式而变化。

4. 同时查看右侧显示的拉取请求（PR）窗格，其中包含 PR 和更改的文件，可用于进一步监控。

## 引导云智能体

了解当前正在执行的任务后，现在要求 Copilot 在实现高对比度模式的同时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。会话标题会因 Copilot 使用的名称和当前工作状态而异。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。出现类似消息时，表示智能体已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示“Steer active session while Copilot is working”对话框的云智能体任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 已回应，并将新请求纳入工作流程。

## 让 Copilot 继续处理任务

与之前一样，Copilot 会继续处理更新后的任务。完成你发送消息时正在执行的步骤后，它会将新请求纳入后续工作流程。

这同样需要几分钟。等待时，可以回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过这个控制中心，可以跟踪所有进行中和已完成的任务、审查工作内容，并重定向正在进行的任务以调整范围或提供补充指导。

本课中，你：

- 使用控制中心和智能体页面监控了云智能体任务。
- 重定向了正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

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
