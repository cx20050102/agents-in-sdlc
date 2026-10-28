---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。任务完成后，它会创建草稿拉取请求（PR），供开发者审查。这样可以将繁琐任务交给智能体处理，加快开发进度，让开发者专注于更重要的工作。

本练习将使用 Copilot 云智能体探索以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债。最初受雇开发网站的承包商没有留下完善的文档，事实上几乎完全没有文档。首先，团队希望为应用程序中所有导出的函数添加 TSDoc（TypeScript 文档注释）。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页上展示相关游戏，也就是同类别的其他游戏，方便用户继续浏览。目前不要求实现精致的成品，只需能用于用户体验（UX）验收测试即可。这项任务目前并非最高优先级，其他议题更重要。

这些任务很容易被推迟，因此适合分配给 Copilot 云智能体异步处理。开发者可以先处理其他工作，之后再回来审查 Copilot 的成果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。就像与其他开发者协作一样，有多种方式向它分配工作，其中包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求跟踪进度、设置环境并开始处理任务。无论会话仍在进行还是已经完成，你都可以查看其过程。解决方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，使用它并不意味着可以跳过思考。即使看似简单的操作也可能包含复杂细节，因此要[谨慎界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作人工智能（AI）编程伙伴：分阶段工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果你已经完成自定义指令练习，就见过 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。分配任务前，先只读查看存储库中已有的指令文件，稍后便能观察它们的影响。

在 GitHub 网页界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意 PR 中生成的 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自上述指令文件。后续练习审查 PR 时还会再次提到。

## 为 Copilot 云智能体设置开发环境

开发代码通常需要特定环境，并运行一些设置脚本以确保环境正常。将任务分配给 Copilot 时也是如此，因为它会像软件工程师（SWE）一样执行任务。

云智能体使用 [GitHub Actions][github-actions] 执行任务。你可以创建专用的设置工作流来定制环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 中，并在智能体开始工作前运行。这样可以提供所需的开发工具和依赖项。为便于实验，本项目已预先配置该工作流：它会提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用程序所需的迁移和填充后的 SQLite 数据库。

```yaml
name: "Copilot Setup Steps"

# 允许你在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为 *你的步骤* 设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，为构建和测试提供数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

尽管大家都知道文档的重要性，许多项目仍然存在信息过时或缺少文档的问题。这类技术债常常得不到处理，降低工作效率，也让维护代码库或引入新团队成员变得更加困难。Copilot 擅长生成文档，因此非常适合将这类议题分配给 Copilot 云智能体。在后台生成所需文档后，你将在后续练习中审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 PR，其标题类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定合适的处理方案。

13. 查看列表及其计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定方案，以及生成代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让你可以专注于其他工作。现在利用这个特性，再给云智能体分配一项任务，然后转而为应用添加功能。

## 在游戏详情页添加相关游戏部分

GitHub Copilot 云智能体的一个优势是可以拆分工作：你处理一组任务时，它可以处理另一组。即使为设计团队添加相关游戏部分不一定需要很长时间，也可以把时间用于其他工作。现在将这项任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论旁应出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在后台处理第二项请求。云智能体的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。现在来创建并使用自定义智能体。

## 总结与后续步骤

本练习探索了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体异步处理，例如解决技术债、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本练习中，你：

- 自定义了代码生成环境。
- 了解了如何确保安全地执行操作。
- 了解了明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体在后台工作时，接下来创建并使用自定义智能体。[Copilot 云智能体也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义指令。

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
