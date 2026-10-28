---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的工作流程通常包括：找到所有相关文件、进行修改、运行测试、调试、找出遗漏文件并再次修改，循环往复。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同的工作方式。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 使用 Copilot 智能体模式探索项目、找到相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨用户界面（UI）和数据层实现新功能。
- 在合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站前，先探索 Tailspin Toys 网站，了解它目前的功能。

此网站是一个带开发者主题的桌游众筹平台，允许用户浏览游戏列表并查看详情。它是一个 Astro 应用程序，在构建时将页面渲染为静态超文本标记语言（HTML）。页面在 frontmatter 中通过 Drizzle 对象关系映射（ORM）直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能则通过使用标准文档对象模型（DOM）API 的小型 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。可在 GitHub Codespace 中按以下步骤运行：

1. 返回 Codespace。上一练习结束后，你应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

    ```bash
    git checkout main
    git pull
    git checkout -b filtering-vscode
    ```

4. 运行以下命令启动网站：

    ```bash
    npm run dev
    ```

    Astro 开发服务器准备就绪后，应会显示类似以下内容的横幅，其中包含 URL：

    ```bash
    🚀 Tailspin Toys is ready!
       Astro server: http://localhost:4321
       Press Ctrl-C to stop.
    ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 时，系统会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

网站启动后，可以浏览其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图像和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 与 Copilot 一起查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型的选择因练习而异；每个步骤会在相关处说明。

网站初始版本可以正常运行，但我们希望添加新功能。先查看待办事项。根据模板设置项目时，系统已自动创建议题；让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

    ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致，以便获得良好体验。不过，由于大语言模型具有概率性，你收到的建议可能与工作坊中的说明不同。这完全正常。

> [!NOTE]
> 由于大语言模型（LLM）具有概率性，Copilot 可能会使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 查看议题待办事项：

    ```plaintext
    Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
    ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还根据它判断对用户最有用的程度，为这些议题排定了优先级。

## 查看指令文件

让智能体生成代码前，先查看将用于向 Copilot 提供上下文的指令文件。本练习将使用[用户界面（UI）指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 针对不同文件类型的具体指令链接，包括：
     - Astro
     - Tailwind 层叠样式表（CSS）
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这样可以复用内容，将复杂指令拆分为较小的部分，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用程序进行三项更改：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前必须运行测试并确保通过。Copilot 智能体模式可以处理这些任务，现在开始添加功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

    ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

    ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

    ```plaintext
    Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
    ```

6. 观察 Copilot 探索项目并找到相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

    ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 选项，因此不必在工作进行时立即选择。

7. Copilot 提示运行测试时，选择 **Continue**。

    ![Copilot Chat 窗格中提示用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，也可能看到测试暂时失败。这没有关系。Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未检测到错误。

    ![Copilot 智能体模式对话完成后的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他人工智能（AI）工具生成的代码。

10. 返回正在运行的网站浏览器，试用新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了功能。你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用 Copilot 智能体模式探索项目、找到相关文件并协调更改。
- 在合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 附加探索练习——实现分页

游戏列表不断扩大后，需要实现分页。运用本练习学到的技能，提示 Copilot 更新网站并实现分页。编写提示时可考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 确定分页方式，例如是否允许用户选择每页大小，还是使用固定值。
- 提供必要指导，让 Copilot 按照预期实现功能。
- 可能需要通过提供更多上下文和要求更改来迭代使用 GitHub Copilot。这是与 Copilot 协作的正常方式。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
