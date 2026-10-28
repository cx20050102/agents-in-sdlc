---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你使用 GitHub Copilot 完成了多个改善用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换在内的无障碍功能。现在检查代码更改；必要时向 Copilot 提供反馈，进一步改进结果。

### 场景

如前所述，加入生成式人工智能（AI）并不会改变软件设计和开发运维（DevOps）的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。现在先审查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的实现，然后再为其他团队成员启用审查。

## 安全与 GitHub Copilot 云智能体

Copilot 云智能体会在无人监督的情况下异步执行任务，因此设置了一些安全限制：

- Copilot 只有存储库读取权限，并且只能写入它用于修改代码的分支。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时工作环境。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议（MCP）服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），也就是为代码添加文档的 PR。此处使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息一下或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![“Files changed”选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改可能有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 规范和注释头。这些内容来自练习 2 开始时查看的自定义指令文件。

6. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应出现在 PR 检查区域的队列中。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像在评论中 @提及团队其他成员那样，在 PR 评论或代码行内评论中使用 `@copilot` 提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，无法规定必须提出的具体请求。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明其用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并使用 `@copilot` 提及 Copilot。可以采用上述建议，或提出其他代码文档改进要求。
2. 选择 **View Session**，观察 Copilot 执行修改。注意它会启动新会话来处理更改。
3. 选择 **Back to pull request** 返回 PR。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，PR 中应出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。确认后，可将 PR 从草稿转换为准备审查状态，再合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改正常后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准的提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流应出现在 PR 检查区域的队列中。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**可以切换到 Codespace 中的该分支，手动测试相关游戏功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能。和之前一样，可以继续迭代并请求云智能体修改，例如调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，分配此议题时，尚未更新自定义指令）。与之前一样，在 **Conversation** 选项卡中添加评论即可提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中在控制中心追加要求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。此时请等待它完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现时，重点检查：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的层叠样式表（CSS）或样式更改。
   - 无障碍属性（无障碍富互联网应用（ARIA）标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更改正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应出现在 PR 检查区域的队列中。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以切换到 Codespace 中的该分支，手动测试无障碍功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后设置仍然有效。

无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像前面一样在评论中提及 **@copilot**，请求更新。

## 可选练习 — 继续委派任务

云智能体适合处理真实待办事项，让你能转而处理其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体开发环境的全部课程。整个过程中，你：

- **检查了存储库附带的自定义指令文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**处理无障碍工作，添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为控制中心**，监控并引导无障碍会话。
- **审查并迭代改进云智能体的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾与后续步骤

云智能体开发环境的课程已完成。若要继续探索，可以结合其他开发环境中的课程：

- 🖥️ **[VS Code 开发环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 开发环境](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

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
